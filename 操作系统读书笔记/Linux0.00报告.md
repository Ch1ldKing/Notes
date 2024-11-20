# Problem
![2cd82d29d4dca90750a5dfaaddb87d3.png](https://s2.loli.net/2024/11/19/hC4QdJiVpF8Zucm.png)
修改一下出问题的代码，在config.cc中
![image.png](https://s2.loli.net/2024/11/19/rJVmTxjyOQp1AE2.png)
然后就可以正常编译了
sudo make，sudo make install
![image.png](https://s2.loli.net/2024/11/19/u19QpmKVXzSZcMW.png)
# Step
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
## 回答问题
### 1. 请简述 `head.s` 的工作原理
#### head.s做了什么？
1. 初始化系统环境，包括全局描述符表（GDT）、中断描述符表（IDT）、以及任务状态段（TSS）和局部描述符表（LDT）
2. 配置并开启定时器中断
3. 实现从内核模式到用户模式的切换
4. 实现基本的任务切换功能（Task 0 和 Task 1 之间切换）
5. 支持系统调用机制
#### 1. 初始化环境
将GDT数据段选择子加载到段寄存器DS，然后初始化SS
```asm
startup_32:
    movl $0x10, %eax
    mov %ax, %ds
    lss init_stack, %esp
```
#### 2. 设置GDT
通过`lgdt`命令，将GDT的大小和位置加载到GDTR寄存器
```asm
setup_gdt:
    lgdt lgdt_opcode
    ret

lgdt_opcode:
	.word (end_gdt-gdt)-1	#计算GDT大小，按16位存储
	.long gdt		        #存储基地址，按32位整型存储
```
#### 3. 设置IDT
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

| 字段```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp | 值        |
| -------------------------------------- | -------- |
| 中断处理程序 (低16位)                          | `0x1234` |
| Segment Selector                       | `0x0008` |
| Reserved                               | `0x00`   |
| Type and Attr.                         | `0x8E00` |
| Offset (High)                          | `0x5678` |

当中断 `0x20` 发生时：

1. CPU 读取 IDT 描述符：
    - 加载 `Segment Selector` (`0x0008`) 到 `CS`。
    - 加载 `Offset` (`0x56781234`) 到 `EIP`。
2. CPU 使用 `0x0008` 查找 GDT，找到内核代码段的描述符。