# 软件视角Views
三维度八视图
Code-level:代码逻辑，函数、方法、类等
component-level：物理组织，包，库，文件等
Moment：某一时刻
Period：某一段时间的变化
![](https://s2.loli.net/2024/05/25/stGjPdfL95KU786.png)
## Build，Code，Moment
一段具体代码实现，也可用AST，或者类图。用它们表示一段代码
## Build，Code，Period
一段代码的变化（Git追踪）
## Build，Component，Moment
某一时刻的项目结构
这部分讲解了库的知识
### 库
![](https://s2.loli.net/2024/05/25/S7GmFeZA8roEXwg.png)
编译时，需要告诉编译器采用了哪些文件。静态链接把库文件拷入代码，
## Build，Component，Period
软件版本管理，SCI（配置项）
### VCS
控制版本
## Runtime
1. 可执行程序：编译后的程序，可以放入内存执行
2. 库：常用指令，被可执行程序调用
3. 配置和数据文件：被可执行程序从硬盘中读取
4. 分布式程序：多个程序互联，如客户端和服务器端交互
## Run,Code,Moment
内存里变量层面的状态
## Run,Code,Period
一段时间内发生的日志
## Run,Component,Moment
UML不同组件之间的交互
## Run,Component,Period
系统层面的日志，组件（程序）运行状态

# 视角转化
比如，代码到组件是设计，构建，编译等
从构建到运行是debug，testing等
![](https://s2.loli.net/2024/05/25/rHjJkf7CtMGYu6R.png)

# 质量因素
Correctness、Robustness、Extendibility、Reusability、 Compatibility 、Efficiency、Portability、Ease of use、Functionality、Timeliness、other
## Correctness
按照 规约 执行
1. 测试与调试
2. 防御式编程
3. 形式化方法
## Robustness
健壮性：针对异常
未被spec覆盖的情况就是异常情况
## 可拓展性 
ADT设计
## Compatibility
兼容性，由保持设计的同构性得到。通过标准化文件和接口等实现
 
# 对质量因素的平衡
我们是一个工程师的工作，需要平衡需求与质量，以实现在有限的资金内实现效益最大化
虽然需要折中，但“**正确性**”绝不能与其他质量因素折中
## OOP
正确性和健壮性：通过spec、testing、checkrep等，实现更可靠的程序
可拓展性和复用性：模块化程序
![](https://s2.loli.net/2024/05/25/wLWpd4Is5ZiT8Fm.png)

# 软件中五个关键因素
1. 优雅的代码
2. 设计出可复用
3. 低复杂性，易拓展
4. 没有错误，安全应对bug（做不到一点bug都没有）
5. 高效的运行
## 可理解性
构建：代码可理解性高，并且有spec，设计模式；项目结构可理解性高。一段时间内是代码重构和版本控制
运行：日志格式
![](https://s2.loli.net/2024/05/25/oUh1LQ3E9PuaTkc.png)
## 可复用性
构建：代码上采用ADT/OOP，采用设计模式，分离接口等；组件上采用API和库实现
运行：动态链接其他库和组件
![image.png](https://s2.loli.net/2024/05/25/qEHBjAuQkxYDJ2O.png)
## 可维护性/易拓展性
构建：代码上模块化设计，降低耦合度、SOLID六大设计模式；组件上同样为SOLID；一段时间是VCS
![image.png](https://s2.loli.net/2024/05/25/1aiV2KmZGlrBHbe.png)
## 健壮性
构建：代码上错误处理、测试有限、Exception、防御式编程等；一段时间上持续整合并测试
运行：单元；整体测试；Debug和内存转储；一段时间上日志输出
![image.png](https://s2.loli.net/2024/05/25/GMVaR3zU4xYDKcC.png)
## 表现
构建：代码调优
运行：代码上空间复杂度（内存管理）、时间复杂性（IO性能），组件上分布式系统；一段时间内是对性能进行数据收集、分析、调优，组件上多线程


