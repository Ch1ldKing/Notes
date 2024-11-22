本报告分为两部分，问题回答和实验步骤。评分请见问题回答
# 问题回答
### 1. 请简述 `head.s` 的工作原理
#### head.s做了什么？
1. 初始化系统环境，包括全局描述符表（GDT）、中断描述符表（IDT）、以及任务状态段（TSS）和局部描述符表（LDT）
2. 配置并开启定时器中断
3. 实现从内核模式到用户模式的切换
4. 实现基本的任务切换功能（Task 0 和 Task 1 之间切换）
#### 1. 初始化环境
将GDT数据段选择子加载到段寄存器DS，然后初始化SS
```asm
startup_32:
    movl $0x10, %eax
    mov %ax, %ds
    lss init_stack, %esp
```
#### 2. 设置IDT
此处使用默认的中断处理程序ignore_int，通过循环初始填充256个IDT
```asm
setup_idt:
    lea ignore_int, %edx       #EDX高16位为中断处理程序的高16位
    movl $0x00080000, %eax     #EAX高16位为代码段选择子，用于指向描述符
    movw %dx, %ax              #EAX低16位为中断处理程序的低16位
    movw $0x8E00, %dx          #中断门，EDX的低16位为描述符类型
    lea idt, %edi              #EDI指向IDT起始地址
    mov $256, %ecx             #ECX计数，填充256个IDT描述符
rp_sidt:
    movl %eax, (%edi)
    movl %edx, 4(%edi)         #高16位为中断处理程序
    addl $8, %edi              #指向下一个EDI
    dec %ecx                   #减一
    jne rp_sidt
    lidt lidt_opcode
    ret

lidt_opcode:
	.word 256*8-1	
	.long idt		
```
##### 中断执行示例：
假设中断向量 `0x20` 对应的 IDT 描述符如下：

| 字段<div style="width:380px"></div> | 值<div style="width:280px"></div> |
| :-------------------------------: | :------------------------------: |
|           中断处理程序 (低16位)           |             `0x1234`             |
|              代码段选择子               |             `0x0008`             |
|                保留                 |              `0x00`              |
|               描述符类型               |             `0x8E00`             |
|           中断处理程序（高16位）            |             `0x5678`             |

当中断 `0x20` 发生时：
1. CPU 读取 IDT 描述符：
    - 加载 *代码段选择子* (`0x0008`) 到 CS寄存器
    - 加载 *中断处理程序* (`0x56781234`) 到 EIP寄存器
2. CPU 使用 `0x0008` 查找 GDT，找到描述符，此处是内核代码段
3. 跳转到pc中的 `CS:EIP = 0x0008:0x56781234`，开始执行中断处理程序
#### 3. 设置GDT
通过`lgdt`命令，将GDT的大小和位置加载到GDTR寄存器
```asm
setup_gdt:
    lgdt lgdt_opcode
    ret

lgdt_opcode:
	.word (end_gdt-gdt)-1	#计算GDT大小，按16位存储
	.long gdt		        #存储基地址，按32位整型存储
```
#### 4. 设置定时中断和系统中断
先设置8253定时芯片，然后初始化定时中断描述符和系统调用中断描述符，填充到对应IDT描述符
```asm
movb $0x36, %al
movl $0x43, %edx
outb %al, %dx
movl $11930, %eax           #时钟频率100Hz
movl $0x40, %edx            #通道0设置成每10ms发送中断请求信号
outb %al, %dx
movb %ah, %al
outb %al, %dx

movl $0x00080000, %eax      #定时中断描述符
movw $timer_interrupt, %ax 
movw $0x8E00, %dx
movl $0x08, %ecx             
lea idt(,%ecx,8), %esi
movl %eax,(%esi)
movl %edx,4(%esi)
movw $system_interrupt, %ax #系统调用中断描述符
movw $0xef00, %dx
movl $0x80, %ecx
lea idt(,%ecx,8), %esi
movl %eax,(%esi)
movl %edx,4(%esi)
```
#### 5. 设置LDT和TSS
该head.s设置了两个任务，各具有一个LDT和TSS
```asm
movl $TSS0_SEL, %eax            #初始用Task0，后面可通过定时中断切换
ltr %ax                         #TSS选择子加载到TR寄存器

movl $LDT0_SEL, %eax 
lldt %ax                        #LDT选择子加载到LDTR寄存器

ldt0:	
	.quad 0x0000000000000000
	.quad 0x00c0fa00000003ff	
	.quad 0x00c0f200000003ff	

tss0:	
	.long 0 			
	.long krn_stk0, 0x10		
	.long 0, 0, 0, 0, 0		/* esp1, ss1, esp2, ss2, cr3 */
	.long 0, 0, 0, 0, 0		/* eip, eflags, eax, ecx, edx */
	.long 0, 0, 0, 0, 0		/* ebx esp, ebp, esi, edi */
	.long 0, 0, 0, 0, 0, 0 		/* es, cs, ss, ds, fs, gs */
	.long LDT0_SEL, 0x8000000	/* ldt, trace bitmap */

	.fill 128,4,0
```
#### 6.从内核模式切换到用户模式
设置用户模式的段选择子和栈地址，然后执行Task0
```asm
pushl $0x17          # 用户模式的数据段选择子
pushl $init_stack    # 用户模式的栈顶地址
pushfl               # 保存当前标志寄存器
pushl $0x0f          # 用户模式的代码段选择子
pushl $task0         # 用户模式代码的入口地址
iret                 # 切换到用户模式
```
#### 7. 定义用户模式任务
```asm
task0:
    movl $0x17, %eax
    movw %ax, %ds
    movb $65, %al              # 打印 'A' 
    int $0x80
    movl $0xfff, %ecx
1:  loop 1b
    jmp task0

task1:
    movl $0x17, %eax
    movw %ax, %ds
    movb $66, %al              # 打印 'B'
    int $0x80
    movl $0xfff, %ecx
1:  loop 1b
    jmp task1
```
### 2. 请记录 `head.s` 的内存分布状况，写明每个数据段，代码段，栈段的起始与终止的内存地址
运行到head.s的末尾，查看GDT表，观察各个段的内存起始和大小![image.png](https://s2.loli.net/2024/11/20/G1aoQ9lmjiHcJhS.png)
经过计算，总结段内存地址如下：

|     段类型     |   起始地址    |    终止地址    |    大小    |      描述      |
| :---------: | :-------: | :--------: | :------: | :----------: |
|   **代码段**   |   `0x0`   | `0x7FFFFF` | 0x7FFFFF |   内核的可执行代码   |
| **数据段（栈段）** |   `0x0`   | `0x7FFFFF` | 0x7FFFFF |   全局变量、堆栈等   |
|   **显示段**   | `0xB8000` | `0xBAFFF`  |  0x2FFF  |    用于屏幕输出    |
|  **TSS0**   |  `0xBF8`  |  `0xC60`   |   0x68   | TSS0，忙碌任务状态段 |
|  **LDT0**   |  `0xBE0`  |  `0xC20`   |   0x40   |   局部描述符表 0   |
|  **TSS1**   |  `0xE78`  |  `0xEE0`   |   0x68   | TSS1，可用任务状态段 |
|  **LDT1**   |  `0xE60`  |  `0xEA0`   |   0x40   |   局部描述符表 1   |
### 3. 简述 `head.s` 57 至 62 行在做什么？
找到57-62行代码所在内存地址，断点`b 0x9d`，然后单步调试![image.png](https://s2.loli.net/2024/11/20/n9tJCk57veASQYU.png)
下图表明压栈的值和栈状态
![image.png](https://s2.loli.net/2024/11/20/8wCtAHfaLvgZpRS.png)
这几行代码是从**内核模式切换到用户模式**
1. `pushl $0x17`将0x17压入栈，0x17为00010111，右移3位索引为2，权限为3，表示用户模式的数据段
2. `pushl $init_stack`将用户模式的栈压入栈
3. `pushfl`将当前标志寄存器EFLAGS压入栈
4. `pushl $0x0f`将0x0F压入栈，0x0F为00001111，右移3位索引为1，权限为3，表示用户模式的代码段
5. `pushl $task0` 将用户模式的任务地址压入栈（此处是从task0开始执行）
6. `iret`中断返回，这一步做了以下几件事：
	1. 恢复CS和EIP：从栈中弹出 `0x0F` 和 `task0`，分别加载到 CS 和 EIP
	2. 恢复EFLAGS：弹出标志寄存器值，恢复到EFLAGS
	3. 恢复SS和ESP：从栈中弹出 `0x17` 和 `init_stack`，分别加载到SS和ESP。
	4. 根据CS中的权限值是 3，将CPU的特权级切换到用户模式
### 4. 简述 `iret` 执行后， `pc` 如何找到下一条指令？
刚刚我们提到CS和EIP恢复出来了。
在用户模式下，CS作为代码段选择子，对应GDT或LDT中的段描述符。
而EIP是下一条指令的线性地址（offset），所以**下一条指令地址为CS+EIP**。
所以pc根据`CS:EIP`找到下一条指令。本处即为用户代码段task0
### 5. 记录 `iret` 执行前后，栈是如何变化的？
执行iret之前的状态如下图，把问题3中提到的用户模式地址压入栈![image.png](https://s2.loli.net/2024/11/20/8wCtAHfaLvgZpRS.png)
执行iret之后，可以看到
![image.png](https://s2.loli.net/2024/11/20/i5nsY6PQ3bgXd9R.png)
此时栈段指向用户模式数据段，切换为了用户模式的栈，`0x17`就是用户模式栈的基地址。此时栈顶指针也被替换为弹出的用户模式的栈起始地址。
总结`iret`**前后栈变化**如下：
1. 弹出EIP和CS的值，为用户模式下一条指令的地址，`CS:EIP`
2. 弹出EFLAGS的值，为用户模式下标志寄存器的值
3. 弹出ESP和SS的值，为用户模式下栈的地址，`SS:ESP`
### 6. 当任务进行系统调用时，即 `int 0x80` 时，记录栈的变化情况
执行`int $0x80`之前，可以注意到int 0x80的下一条指令地址是`0x10eb`
![image.png](https://s2.loli.net/2024/11/20/jROxtes1EapMhA9.png)
执行`int $0x80`之后，用户模式的SS:ESP，EFLAGS，CS:EIP都被压栈
![image.png](https://s2.loli.net/2024/11/20/wOLG7W3kc2vHnCY.png)
栈中内容变化总结如下

| 栈地址   | 执行 `int 0x80` 之前内容 | 执行 `int 0x80` 之后内容 | 说明                    |
| ----- | ------------------ | ------------------ | --------------------- |
| 0xE4C | 无内容（内核栈顶）          | `0x000010eb`       | 用户模式的 `EIP`（下一条指令地址。  |
| 0xE50 | 无内容                | `0x0000000f`       | 用户模式的 `CS`（代码段选择子）    |
| 0xE54 | 无内容                | `0x00000246`       | 用户模式的 `EFLAGS`（标志寄存器） |
| 0xE58 | 无内容                | `0x00000bd8`       | 用户模式的 `ESP`（用户栈顶地址）   |
| 0xE5C | 无内容                | `0x00000017`       | 用户模式的 `SS`（用户栈段选择子）   |
### 7. 调试跟踪 `jmpi 0,8` ，解释如何寻址
1. 找到所在内存地址并打断点`b 0x7c4c`![[Pasted image 20241122103033.png]]
2. 单步运行，观察变化，代码段寄存器CS变为0x08，EIP变为0x00。![[Pasted image 20241122103128.png]]
3. 这个变化是因为**寻址**。`jmpi`是远跳转指令，参数`0,8`分别表示：
   - `0`是偏移地址offset
   - `8`是段选择子，其与GDT或LDT中的某个段描述符对应。
   在此处，段选择子的结构如下：
```
  15       3      2      0
+------------+------+------+
|  Index     | TI   | RPL  |
+------------+------+------+
```
   如果是8的话，最后四位是1000，index即为1，Ti为0表示GDT表，所以**指向代码段**，权限级别为0（内核级别）。
   加载段选择子`8`到CS寄存器后，从GDT中取出该段的描述符，也就是代码段，读取它的基地址和段长。此处得到基地址为`0x00`
```
! 代码段描述符定义
.word 0x07FF ! 8Mb - limit=2047 (2048*4096=8Mb) 
.word 0x0000 ! base address=0x00000 
.word 0x9A00 ! code read/exec 
.word 0x00C0 ! granularity=4096, 386
```
   然后在代码段的基础上加上偏移量`0`，因此EIP为0。所以寻址结果就是`CS:EIP`为`0x08:0x00`，而实际物理地址为段描述符对应的基址加偏移，即为
   `线性地址 = 0x00000000 + 0x0000 = 0x00000000`，可以与截图中跳转后第一条指令地址对应。


# 实验准备和步骤
## 1. 安装bochs
实验环境：ubuntu 22.04.5+bochs 2.6
1. 安装依赖`sudo apt-get install build-essential xorg-dev libgtk2.0-dev`
2. 清除构建缓存`make dist-clean`再执行配置脚本`./configure --enable-debugger --enable-disasm --enable-debugger-gui` 得到Makefile文件
3. 执行sudo make。遇到报错
![2cd82d29d4dca90750a5dfaaddb87d3.png](https://s2.loli.net/2024/11/19/hC4QdJiVpF8Zucm.png)
修改一下出问题的代码，在config.cc中
![image.png](https://s2.loli.net/2024/11/19/rJVmTxjyOQp1AE2.png)
然后就可以正常编译了
sudo make，sudo make install
![image.png](https://s2.loli.net/2024/11/19/u19QpmKVXzSZcMW.png)

4. 输入 bochs --help，成功安装
## 2. 编译Linux000
1. 先安装as86编译程序`sudo apt install bin86`
2. 在linux000 code下执行`sudo make`，得到image文件，把它移动到/linux000下，和bochs配置文件放在一起
   ![image.png](https://s2.loli.net/2024/11/20/BLhnMe4bk1HyZrV.png)
## 3. 执行bochs仿真程序
1. 先了解.bxrc文件，这是bochs的配置文件
   - `megs: 16`为选择运行内存大小，默认为32MB
   - `romimage: file=$BXSHARE/BIOS-bochs-latest`选择机器的bios
   - `vgaromimage: file=$BXSHARE/VGABIOS-lgpl-latest`选择机器的VGA Bios
   - `floppya: 1_44="Image", status=inserted`表明是使用Image软盘镜像文件，a:为软盘设备的意思
   - `boot: a`为从软盘启动，`boot: c`为从硬盘启动
   - `log: bochsout.txt`输出日志文件
   - `display_library: x, options="gui_debug"`配置图形化界面
2. 执行`bochs -q -f linux000_gui.bxrc`，报错![image.png](https://s2.loli.net/2024/11/20/w72cnqAIhNPVej9.png)
   执行`sudo apt install libcanberra-gtk-module`，然后再次运行成功
3. **查看`0x7c00`** ：重启bochs，输入命令`b 0x7c00`在该处打断点，然后点击continue，观察到的命令就是`0x7c00`处装载的内容，也就是**MBR主引导记录**，将硬盘中第一个扇区的内容中的引导加载程序(boot.s)和分区表加载到内存地址`0x7c00`![image.png](https://s2.loli.net/2024/11/20/9VbODe1ckhT6NCA.png)
## 4. 调试`head.s`
1. 先打断点`b 0`，因为head.s被引导程序放置在此处
2. 运行到head.s的末尾，查看GDT表，观察各个段的内存起始和大小![image.png](https://s2.loli.net/2024/11/20/G1aoQ9lmjiHcJhS.png)
3. 找到57-62行代码所在内存地址，断电`b 0x9d`，然后单步调试![image.png](https://s2.loli.net/2024/11/20/n9tJCk57veASQYU.png)可以看到eflags被操作
