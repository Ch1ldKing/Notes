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
📕概念：
+ 仓库：CMDB
+ 工作拷贝：开发者本地的项目拷贝
+ 文件：一个配置项SCI
+ 版本：所有文件在某个时间点的状态集合
+ 变化