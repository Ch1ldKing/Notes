###### 目标
1. 测试优先
2. 模块设计：等价划分、边界值分析
3. 覆盖度
本节内容如下
![image.png](https://s2.loli.net/2024/05/25/bhR9qot8cKFjpwT.png)
# Software testing
测试是为了“破坏”
###### 好的测试？
1. 能发现错误
2. 不冗余
3. 有最佳特性
4. 别太复杂也别太简单
## 测试等级
回归测试包含三类
单元、集成、系统，对应不同的级别
![image.png](https://s2.loli.net/2024/05/25/bOkzoa51PhlgEtR.png)
## 一些概念
1. 静态和动态测试：静态只能发现一些语法错误或者死循环（IDE的代码检查），而动态测试检查逻辑上的问题，通过结果来执行
2. 测试和Debug：发现错误和消除错误
3. **白盒测试**对内部代码结构进行测试，**黑盒测试**对程序表现的东西测试，比如输入得到什么输出
## 软件测试是困难的
1. 穷举+暴力不行，不可能全覆盖
2. 偶然测试不能覆盖所有可能性
3. 靠统计数据也不行，产生错误的往往是极少出现的数据
4. bug出现不符合概率分布，没有统计规律
# Test Case
**测试用例**：输入+执行条件+期望结果
最可能发现错误，且不重复冗余
# 测试优先的编程
## 步骤
1. spec
2. 根据spec写测试用例
3. 写代码
## Spec
1. 参数的类型及约束，比如sqrt()的参数得是非负数（前置条件）
2. 返回值的类型，以及它和输入有什么关系（后置条件）
3. 异常说明
4. 描述本函数的功能
写测试用例就是找出spec的bug
😀好处？<mark style="background: #BBFABBA6;">节省大量的debug时间</mark>
## 测试驱动的开发TDD
TDD开发过程：非常短的一种重复开发周期，将需求转化为测试用例
1. 编写失败的测试
2. 编写代码使测试通过
3. 重构代码以提高质量
4. 重复以上步骤
# 单元测试
对最小单元测试，隔离各个模块，容易定位错误和调试
*** 
测试用例要提供一系列数据的条件，包括本地数据结构、边界条件、路径和错误处理等 ，由驱动程序给入被测模块；被测模块依赖的模块则由**桩程序**模拟
![image.png](https://s2.loli.net/2024/05/25/nEiHwG5vhxb6qga.png)
# Junit
## 一些用法
1. `@Test`注解，用于引入Junit
2. 一些assertion方法，`assertEquals(`~~expected~~:`2,`~~actual~~:`Math.max(1,2))`,`assertTrue`,`assertFalse`等
3. `@Before`或`setUp()`在测试前进行初始化（如一些类的引入和构造），`@After`或`tearDown()`在测试后回收资源
4. 测试的包组织结构需要与src保持一致
# 黑盒测试
只检查功能，不管你内部如何实现
黑盒的测试用例就是检查程序是否符合规约，用尽可能少的测试用例发现更多错误
## 1. 等价类划分 Equivalence Partitioning
❓概念：将被测函数的输入域划分为等价类，从等价类中导出测试用例
😎划分：按照输入数据的约束条件（前置条件）划分，从而使每个等价类代表**满足或违反**的**有效或无效**数据的集合，最终从没个等价类选出一个座位测试用例
![image.png](https://s2.loli.net/2024/05/25/OPGrEJvy6mKUfwi.png)
根据spec去分析等价类，从不同角度：正负、奇偶；特殊情况：上限，下限，0
***
🌰例子：应该选第三个，选一个对于当前问题最有特点的测试，而其他的都是很普通的满足spec的测试，对于翻转来说，奇偶是一个很容易出错的问题![image.png](https://s2.loli.net/2024/05/25/hKR92k8WMA4yDNV.png)
## 2.边界值分析
意义：大量的错误发生在输入域的边界而不是中央，在等价类划分时，将边界作为等价类加入
❓会有哪些bug：程序行为在边界可能会突变；某些是特殊情况，需要通过防御等特殊处理
🌰如何分析边界：给出两个例子![image.png](https://s2.loli.net/2024/05/25/v2yrSQmTe7soi9J.png)
![image.png](https://s2.loli.net/2024/05/25/ul61L8tsderOmUH.png)
#### 两种方法
1. 笛卡尔积：每个维度的每个取值，都要**相乘**组合。比如Test1覆盖了边界值a，第一种组合是(a,b,c)，后面还会出现(a,d,c)等
2. 覆盖每个取值：每个维度的每个取值只需**被一个测试用例**覆盖。还是刚才的例子，但第二次不会出现a了，即出现(e,d,c)等，也就是只被一个测试用例覆盖
🌰例子，一个测试用例能做到覆盖多个维度的测试，并且不需要重复![image.png](https://s2.loli.net/2024/05/25/BjOlEJysPecaoiZ.png)
# 白盒测试
与黑盒不同，要考虑内部实现细节，需要根据**程序执行路径**来设计测试用例
❓我的理解，根据程序代码运行时可能走过的所有路径进行测试，比如进入或不进入循环/if，是否抛出异常，为每种路径至少覆盖一次
🌰例子
```Java
public class Division {
    public int divide(int numerator, int denominator) {
        if (denominator == 0) {
            throw new IllegalArgumentException("Denominator cannot be zero");
        }
        return numerator / denominator;
    }
}
```
> 覆盖如下：
> 1. **语句覆盖**：
    - 测试用例1：`divide(4, 2)`，预期结果：`2`
        - 覆盖语句：`if (denominator == 0)` (false) 和 `return numerator / denominator;`
    - 测试用例2：`divide(4, 0)`，预期结果：抛出 `IllegalArgumentException`
        - 覆盖语句：`if (denominator == 0)` (true) 和 `throw new IllegalArgumentException("Denominator cannot be zero");`
> 2. **分支覆盖**：
    - 测试用例1：`divide(4, 2)`，预期结果：`2`
        - 覆盖 `if (denominator == 0)` 的false分支。
    - 测试用例2：`divide(4, 0)`，预期结果：抛出 `IllegalArgumentException`
        - 覆盖 `if (denominator == 0)` 的true分支。
> 3. **路径覆盖**：
    - 测试用例1：`divide(4, 2)`，预期结果：`2`
        - 覆盖路径：进入方法 -> `if`条件为false -> 执行除法并返回。
    - 测试用例2：`divide(4, 0)`，预期结果：抛出 `IllegalArgumentException`
        - 覆盖路径：进入方法 -> `if`条件为true -> 抛出异常。
> 4. **条件覆盖**：
    - 测试用例1：`divide(4, 2)`，预期结果：`2`
        - 确保条件 `denominator == 0` 为false。
    - 测试用例2：`divide(4, 0)`，预期结果：抛出 `IllegalArgumentException`
        - 确保条件 `denominator == 0` 为true。
# 测试覆盖度
代码覆盖度：已有的测试用例有多大程度覆盖了被测程序
测试效果：路径覆盖>分支覆盖>语句覆盖
测试难度：路径覆盖>分支覆盖>语句覆盖
逐步增加测试用例，达到覆盖标准即可
# 自动化测试和回归测试
自动进行测试，每次变更后都自动运行(CI)，以尽早发现缺陷
自动回归测试能保证系统功能没有受到新代码的影响
**持续集成**：持续将新功能进行测试，打包，集成到系统中
# 记录测试策略
目的：在代码评审过程中，其他人可以理解你的测试，并评判你的测试是否足够充分。记录根据什么来选择测试用例
🌰例子：左侧为spec，右侧测试用例，包括分区维度：对三个维度进行取值，取了哪几种值，并解释为何这样取值。对于一些测试方法，可以解释它覆盖了什么部分![image.png](https://s2.loli.net/2024/05/25/EPILdMnU39Htrk6.png)
