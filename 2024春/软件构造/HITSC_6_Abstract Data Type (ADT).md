# Abstraction and User-Defined Types
📕数据抽象：指由一组操作所刻画的数据类型
传统的类型关注数据的具体表示，而抽象类型强调“作用于数据上的操作”，无需关注数据如何存储，而是设计并使用操作
即**ADT是由操作定义的，与其内部如何实现无关！**
比如，定义一个抽象的Bool类，Bool可以由很多的东西实现，但它的操作决定了它的性质
![image.png](https://s2.loli.net/2024/05/27/cG5dP7om1JSClVT.png)
# 类型与操作
## 类型
可变类型和不可变类型
**区别**：可变类型提供了可改变内部数据的值的操作，而不变类型只能构造新的对象
## 操作
### 构造器
初始化对象的状态，包括底层数据结构等，同时验证不变性，有以下说法：
1. 可以不带参数，并创造一个空对象
2. 可以不带参数，并创造一个有初始默认值的对象
3. 可以带参数，创造一个初始化对象
### 变值器
如果返回值为void，则必然意味着它改变了对象的某些内部状态
也有可能非空
# ADT举例
![image.png](https://s2.loli.net/2024/05/27/Lxfc6sNFXORrZMC.png)
# 设计ADT
设计好的ADT，靠“经验法则”，提供一组操作，设计其行为规约 spec
1. 设计简洁、一致的操作
2. 足以支持client对数据所做的所有操作需要，且用操作满足client需要的难度要低
3. 要么抽象、要么具体，不要混合---要么针对抽象设计，要么针对具体应用的设计
# 表示独立性
表示独立性：client使用ADT时无需考虑其内部如何实现，ADT内部表示的变化不应影响外部spec和客户端
这意味着当我们对代码进行优化时，我们无需通知客户端也无需修改客户端代码，因为优化后仍然满足spec。这就是RI的作用
🌰例子：
```Java
违反RI
/**
 * Represents a family that lives in a household together.
 * A family always has at least one person in it.
 * Families are mutable.
 */
class Family {
    public List<Person> people;
    public List<Person> getMembers() {
        return people;
    }
}

void client1(Family f) {
    Person baby = f.people.get(f.people.size() - 1); // 直接访问内部表示，违反封装
    // ...
}
```
问题：直接暴露了people的内部，并且没有封装好。查询方法依赖于具体内部有多少个元素这类的实现细节
```Java
改进RI
/**
 * Represents a family that lives in a household together.
 * A family always has at least one person in it.
 * Families are mutable.
 */
class Family {
    // 使用Set代替List，以避免重复元素
    public Set<Person> people;

    /**
     * @return a list containing all the members of the family, with no duplicates.
     */
    public List<Person> getMembers() {
        return new ArrayList<>(people);
    }
}

void client3(Family f) {
    // 通过getMembers方法获取成员列表，而不是直接访问内部表示
    Person anybody = f.getMembers().get(0); 
    // ...
}
```
1. 采用Set，防止成员重复，同时改进了List的实现细节问题
2. 通过getmembers来获取成员列表，而不是直接暴露内部people，防止修改
3. getMembers不直接返回people，而是复制一个列表，防止表示暴露
# 测试一个ADT
1. 用observers测试creators、producers、mutators
2. 调用creators、producers、mutators等产生或修改结果来测试 observers
这么做有风险，有可能依赖的其他方法有错误导致测试结果失效
🌰例子：测试策略：测试观察者时要调用所有的构造器和生产者
```java
//测试构造器
valueOf():
	true, false
	string = produced by valueOf()
//测试观察者
length():
	string len = 0, 1, n
	string = produced by valueOf(), produced by substring()
//测试观察者
charAt():
	string len = 1, n
	i = 0, 1, len - 1
	string = produced by valueOf(), produced by substring()
//测试生产者
substring():
	string len = 0, 1, n
	start = 0, middle, len
	end = 0, middle, len
	end-start = 0, n
	string = produced by valueOf(), produced by substring()
```
在测试时，其实是组合起来测试的，能保证所有方法之间的交互，同时覆盖全面，如下，每种测试都采用了多种混合的方式![image.png](https://s2.loli.net/2024/05/28/HLDR9IFbWtXwvuT.png)
# 不变量Invariants
一个任何时候都不发生改变的量，用来保持程序的正确性，发现错误
## 防止不变量被修改
不能产生表示泄露
用private 和final 进行修饰来实现一些变量不会被外部获取或修改
🌰例子：比如这个，当设定timestamp为final时，![image.png](https://s2.loli.net/2024/05/28/Z9BshOIbWmlJAXF.png)
d也指向了t的timestamp，则对tweet的timestamp产生了修改，导致表示泄露，修改了不变量，导致Tweet不能保证它是不变量![image.png](https://s2.loli.net/2024/05/28/DdqjAX8PCyt7KU1.png)
# RI 和 AF
![image.png](https://s2.loli.net/2024/05/28/97RkQloITW5FfDG.png)
## RI
表示不变量RI：某个具体的“表示”是否是“合法的”
也就是R的一个子集，这里面都是合法的输入，是一些限定条件
❓为什么是表示不变量：在方法执行完后，要仍然保持住所设定好的RI
## AF
抽象函数：R和A之间映射关系的函数，即如何去解释R中的每一个值为A中的每一个值
也就是映射到客户端那里的值，需要具体解释是如何映射的
## checkRep()
通过一个方法`checkRep()`来保证任何时候都是不变量，所有可能改变rep的地方都要检查
