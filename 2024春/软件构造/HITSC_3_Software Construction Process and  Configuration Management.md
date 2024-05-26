###### 目标
软件开发流程及模式，敏捷开发，软件配置管理SCM，Git，软件构造过程和构造工具
# SDLC
![image.png](https://s2.loli.net/2024/05/26/5YdSeJfrZwklayp.png)
# 传统软件开发模型
## 瀑布模型
线性模型
优点：划分阶段，管理简单
缺点：不迭代，所以缺少灵活性，也难以适应需求；并且用户看不到原型，导致风险高，前期错误后期发现
## 增量模型
瀑布串行，容易适应需求增加
![image.png](https://s2.loli.net/2024/05/26/85enyImTOWbNP7S.png)
## V模型
左侧开发，右侧测试，并行进行，并且每个阶段都要验证
优点：阶段划分，质量高，早发现错误
缺点：顺序执行，难以应对需求变化，成本高
![image.png](https://s2.loli.net/2024/05/26/pIdgzAxEXi3kJQO.png)
## 原型过程
开发原型，不断迭代，用户试用评审，反馈修改，直到用户满意![image.png](https://s2.loli.net/2024/05/26/6eKFw8vDQYRS2fm.png)
## 螺旋模型
多轮迭代，每一轮是瀑布，每一轮都有目标，迭代时严格验证![image.png](https://s2.loli.net/2024/05/26/LZm9DxgtYB6npyQ.png)
# 敏捷开发
快速迭代和小规模改进，**Agile = 增量 + 迭代**
敏捷宣言：
1. **个体和互动胜过流程和工具**（Individuals and interactions over processes and tools）：
    - 强调团队成员之间的合作和交流，而不是过度依赖于严格的流程和工具。
2. **工作的软件胜过详尽的文档**（Working software over comprehensive documentation）：
    - 重点是交付能够实际运行的软件，而不是花费大量时间编写和维护详细的文档。
3. **客户合作胜过合同谈判**（Customer collaboration over contract negotiation）：
    - 注重与客户的持续合作和沟通，而不是仅仅根据合同进行工作。
4. **响应变化胜过遵循计划**（Responding to change over following a plan）：
    - 强调能够灵活应对变化的能力，而不是严格按照预先制定的计划执行。
📕重点：用户参与，小步骤迭代，确认验证
😀一些理念：结对编程，CI持续集成，taskbord与客户共同制定开发计划与迭代周期，频繁发布小版本便于用户反馈，TDD测试驱动，持续改进代码结构，有编码标准和集体代码所有权
# SCM和VCS
SCM软件配置管理：追踪控制软件变化
SCI软件配置项：软件中发生变化的基本单元（比如文件）
基线：记录软件变化中的稳定时刻
CMDB配置管理数据库：存储软件配置项变化信息和基线
## 版本控制
作用：便于回滚、比较差异、备份历史版本、获取备份、合并
支持多开发者协作并记录开发者行为，便于审计
结构：线性和分支结构
📕一些概念：
+ 仓库：CMDB
+ 工作拷贝：开发者本地的项目拷贝
+ 文件：一个配置项SCI
+ 版本：所有文件在某个时间点的状态集合
+ 变化：版本间差异
+ HEAD：当前工作的版本
#### VCS
Local：本地的，不能共享协作
Centralized：仓库存储在独立服务器
Distributed：仓库存储独立的服务器以及每个开发者的机器
![image.png](https://s2.loli.net/2024/05/26/XABbEj7v5LnIsw6.png)
# Git
## 仓库
本地git仓库、工作目录（就是本地的文件系统）、暂存区（隔离工作目录与Git仓库）
之间的三种行为：修改、暂存、提交
![image.png](https://s2.loli.net/2024/05/26/djMsBo5pDywgYLu.png)
同时包括远程仓库
## Node Tree
图解：
+ 通常情况下，commit指向一个父亲
+ 多个commit指向同一个父亲，那么他们是多个分支
+ 一个commit指向两个父亲，它是分支的合并
+ HEAD是当前的commit源
+ branch是分支名字
![image.png](https://s2.loli.net/2024/05/26/drgNPEyUph69x8F.png)
## commit
commit中有作者等信息，且其指向一颗树，树中有指向文件的指针，如果未发生变化，则不需要重复存储这两个版本的文件
![image.png](https://s2.loli.net/2024/05/26/N1LJvCnOPzqdVKw.png)
如果文件变化了，则会存储两个不同的文件，两个tree指向不同的文件![image.png](https://s2.loli.net/2024/05/26/Wnl15LPqvAZgh3z.png)
## Git优点
存储的是文件，不只是代码行，无需在回滚提交时大量进行修改操作，节省时间
![image.png](https://s2.loli.net/2024/05/26/mZrnhOfUJIpMsAv.png)
## Merge
git fetch可获取远程分支的变化，并选择是否与本地分支进行合并
git merge可以合并两个分支，并在合并前进行代码审查
## 合作
Fork和pull request 使用户之间可以向其他人仓库提交修改申请，以实现多人合作。每个人都fork仓库并向总项目提交PR
# 软件构造过程
![image.png](https://s2.loli.net/2024/05/26/u1qKpT2CkZlt3Uw.png)
## 1.Programming
### 语言
从用途上划分
+ Programming languages (e.g., C, C++, Java, Python)编程语言
+ Modeling languages (e.g., UML) 建模语言
+ Configuration languages (e.g., XML) 配置语言
+ Build languages (e.g., XML) 构建语言
从形态上划分
+ Linguistic-based 基于语言学的构造语言
+ Mathematics-based (formal) 基于数学的形式化构造语言
+ Graphics-based (visual) 基于图形的可视化构造语言
### 编程工具
IDE：
+ 源代码编辑器、智能代码补全工具、代码重构工具
+ 文件管理
+ 库管理
+ 软件逻辑可视化（转化为图）
+ GUI构造器
+ 编译器和解释器
+ 自动Build工具（编译、链接、打包、测试、部署）
+ VCS
+ 外部工具
### 建模语言
可视化系统设计，自动生成代码，便于验证与交流
### 配置语言
描述软件的配置参数和初始化设置，比如环境、组件链接等
## 2.代码评审
静态代码分析
要求风格，设计等
## 3.动态性能分析
执行程序观察现象，度量性能与状态
## 4.调试和测试
测试发现错误，调试定位错误根源
## 5.重构
不改变功能，但通过结构等方式对代码优化，使其逻辑更清晰，设计风格更优，减少不必要的代码行
## 6.构建
编译、打包、静态分析、测试、生成文档、部署等，并实现自动化，借助构建工具
🌰Cmake，Maven，Gradle，Webpack等