*本文请配合大纲食用*

最近很多同学都在吐槽一门课：软件构造。🤬有的同学说这是一门“文科课”：“这么多概念，不是设计理念，就是教我怎么写注释，跟politics一样教条”

这位同学说的也没什么错，确实是学了这些东西和概念，什么不变量啊，一揽子设计模式啊，什么测试策略和规约，确实很多概念。但问题在于，它并不教条。

# “软件构造”是一种思想
❓为什么这么讲
 1. 首先，这是一门我认为最接近上班的**实际生产**的课程，实际上，这是一门未来工作的“减负课”，旨在提出一系列方法来参考我们的生产活动，也就是代码。
 2. 其次，这门课其实是在帮你从学生向程序员进行**思想上的改变**：我们写代码不再是写一段程序、写一个功能、写一个算法，而是要把砖块筑成大厦。在思考一个程序的时候，我们不再会思考“先定义一个变量，再通过循环遍历，最后返回值”，而是思考“为了这个功能，我需要设计哪些实体类，同时我需要测试、设计 RI，设计父类子类”。显然，我们的视角已经上升了一到两个层次。
 3. 这是一门“**经验学科**”，前人总结了一系列方法，能够规范代码的行为，并通过这门课把这些方法喂到我们嘴里。如果某一天一群“有素质”的程序员写了一个程序，你并不会觉得有什么。但一个没学过“软件构造”的人进来了，一切功能都能跑，但你改的时候，就是头疼的要命。你会不会破口大骂呢
 4. 当一大堆理念传授给你的时候，往往你学会的就不是方法和理念，而是理念背后的**思想**。我敢打赌，即使你课都没好好听，在借鉴（copy）实验作业的过程中，熬夜看 PPT 的时候，也有一种设计思想润物细无声般进入了你的大脑：
	+ 测试优先：这能保证我的程序及时发现错误，减少后期成本
	+ 写好测试策略和规约等注释文档：这能保证我和其他程序员不用读复杂的代码，也能知道这段代码干了什么
	+ 设计好 ADT：这能将现实中的实体引入到虚拟世界，未来的工作就是基于现实优化的网络世界。同时，做好防护（AF，RI），能帮你迅速找到许多 bug，好事一桩
	+ 做好 AF 、RI和表示泄露处理：这能保证我的服务端和客户端之间不需要“知根知底”就能进行交流与开发。要知道，当你把一切都告诉别人，别人往往不是帮你，而是害你
	+ OOP 的理念：我更愿意称其一种“模块化”的理念。当一个复杂的程序，可以被拆解为一个一个类，并且通过 Override、Overload等实现类之间的继承，你通过抽象出来的接口，最终拼接成一个完整的程序。而当你维护时，发现一切已经在最初设计时就铺平道路，听着就爽
	+ 复用性、拓展性、健壮性、正确性：如果一个程序满足这四点，那么它就是一个比较完美的程序。从开发到运维，真正的全过程生命周期，都被包含在内。所以这就是一个程序需要满足的东西，简单而纯粹
因此，这些才是软件构造所学的。即使你忘记了具体有哪些设计方法，忘记了 `@Overload`怎么用，忘记了 Javadoc 怎么写，但这些思想是不会消散的

# 多年以后，你可能需要回顾的东西
本文章显然不是为了考试，而是在说“我学了什么有用的”。主要基于上面提到的思想进行拓展：
## 测试优先
### “测”什么
![image.png](https://s2.loli.net/2024/05/25/bOkzoa51PhlgEtR.png)
### 怎么“测”
🧐**好的测试？**
1. 能发现错误
2. 不冗余
3. 有最佳特性
4. 别太复杂也别太简单
首先，要明确单元测试是需要根据 spec 进行的，这个后面会说
因此，我们需要一些测试的方法：
#### 黑盒测试
##### 等价类划分
根据spec去分析等价类，从不同角度：正负、奇偶、整数非整数
##### 边界值分析
边界上的值：比边界略大略小、无限大无限小、0
##### 如何设计
每个维度被覆盖一次即可
![image.png](https://s2.loli.net/2024/05/25/BjOlEJysPecaoiZ.png)
#### 白盒测试
考虑内部实现细节，根据程序代码运行时可能走过的所有路径进行测试，比如进入或不进入循环/if，是否抛出异常，为每种路径至少覆盖一次
举个例子：
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
每一种可能运行过的逻辑都要覆盖到
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
#### 测试覆盖度
已有的测试用例有多大程度覆盖了被测程序
#### 回归测试
每次都要完整的测试整个系统才能保证其他功能不受影响，且配合良好
### 告诉别人咋“测”的
记录一下测试策略呗
🌰例子：左侧为spec，右侧测试用例，包括分区维度：对三个维度进行取值，取了哪几种值，并解释为何这样取值。对于一些测试方法，可以解释它覆盖了什么部分![image.png](https://s2.loli.net/2024/05/25/EPILdMnU39Htrk6.png)
## 写好规约
### 规约干嘛的
1. 给自己和别人写出设计决策：如final、数据类型定义
2. 作为契约，服务端与客户端达成一致
3. 调用方法时双方都要遵守
4. 便于定位错误
5. 解耦，无需告诉客户端具体实现，变化也无需通知客户端，扮演**防火墙**角色
6. 判断行为等价性
### 规约都有啥
1. **前置条件**：对客户端的约束，在使用方法时必须满足的条件，用`@param`，并用`@requires`进行说明
2. **后置条件**：对开发者的约束，方法结束时必须满足的条件，用`@return`和`@throws`，并用`@effects`进行描述
3. 契约：前置条件满足了，则后置条件必须满足
### 怎么写好规约
规约其实是你的代码设计方案
#### 1.内聚的
Spec描述的方法应单一、简单、易理解
分离：规约做了两件事，所以要分离开形成两个方法。可以使spec更容易理解，且耦合性低应对变化。如下
```Java
public static int LONG_WORD_LENGTH = 5;
public static String longestWord;

/**
 * Update longestWord to be the longest element of words, and print
 * the number of elements with length > LONG_WORD_LENGTH to the console.
 * @param words list to search for long words
 */
public static void countLongWords(List<String> words) {}
```
#### 2.信息丰富的
不能引起客户端的歧义
🌰例子：客户端不知道返回null是因为原来绑定的值是null，还是因为不存在旧值
```Java
static V put(Map<K,V> map, K key, V val)
/**
* requires: `val`可以为`null`，`map`可以包含`null`值
* effects: 将`(key, val)`插入到映射中，如果存在相同的键，则覆盖旧值。返回该键的旧值，如果不存在旧值，则返回`null`
*/
```
#### 3.足够强
太弱的spec，client不放心、不敢用 (因为没有给出足够的承诺)。
开发者应尽可能考虑各种特殊情况，在post-condition给出处理措施
🌰例子：客户端在得到Exception的时候，不知道哪些元素被添加了，需要自己定位。应该完善exception
```Java
static void addAll(List<T> list1, List<T> list2)
effects: adds the elements of list2 to list1,
         unless it encounters a null element,
         at which point it throws a NullPointerException
```
#### 4.使用抽象类型
给方法实现体与客户端更大的自由度
```java
static ArrayList<T> reverse(ArrayList<T> list)
effects: returns a new list which is the reversal of list, i.e.
		 newlist[i] == list[n-i-1]
		 for all 0 <= i < n, where n = list.size()
```
#### 5.避免可变量
😀方法内部尽量不要修改传入的参数，不要设计mutating的spec，容易引发错误。除非必须是mutator方法的spec，否则避免使用mutable的类与方法。
📕因为程序中很有可能有**多个变量指向同一个可变对象**（别名），在类的实现体或客户端**保存别名**的情况下，可能导致修改并产生bug
🌰例子：
客户端为了用户隐私，因此隐藏了id前5位
```Java
char[] id = getMitId("bitdiddle");
for (int i = 0; i < 5; ++i) {
    id[i] = '*';
}
System.out.println(id);
```
服务端担心效率，所以采用了cache全局可变变量(char\[\]可变)
```Java
private static Map<String, char[]> cache = new HashMap<String, char[]>();

public static char[] getMitId(String username) throws NoSuchUserException {
    // see if it's in the cache already
    if (cache.containsKey(username)) {
        return cache.get(username);
    }

    // ... look up username in MIT's database ...

    // store it in the cache for future lookups
    cache.put(username, id);
    return id;
}
```
由于char\[\]可变，修改前五位会导致Map中的数据也被更改。所以最好采用String
## 设计好 ADT
ADT是由操作定义的，与其内部如何实现无关
### ADT都有啥
1. 构造器
2. 观察器
3. 生产器
4. 变值器（定义了是否可变）
### 咋设计 ADT
#### 简洁一致
#### 表示独立性
能够实现不论服务端代码如何改变 ADT 的内部具体实现，客户端对于ADT的使用不会变，仍然满足 spec，也就是 ADT 的**本质**没有变
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
#### 不变量
一个东西在程序中是不会改变的，这样满足一些设计的同时我们可以很轻易的判断程序是否出错，但要记得防止不变量被修改
##### 如何防止不变量被修改
![image.png](https://s2.loli.net/2024/05/28/97RkQloITW5FfDG.png)
###### RI
表示不变量RI：某个具体的“表示”是否是“合法的”
也就是R的一个子集，这里面都是合法的输入，是一些限定条件
❓为什么是表示不变量：在方法执行完后，要仍然保持住所设定好的RI，运行后这个值仍然在 R 的子集当中
###### AF
抽象函数：R和A之间映射关系的函数，即如何去解释R中的每一个值为A中的每一个值
也就是映射到客户端那里的值，需要具体解释是如何映射的
###### checkRep()
通过一个方法`checkRep()`来保证不变量任何时候都不会改变，所有可能改变rep的地方都要检查。可以借此替代前置条件，方法是在每个方法中加入checkRep，并抛出Exception
### 咋测试ADT
这就用到测试优先思想了，无论何时你要记得测试一段程序
1. 用observers测试creators、producers、mutators
2. 调用creators、producers、mutators等产生或修改结果来测试 observers
## OOP设计理念
### Object
Object 由类组成，定义了方法和变量
#### 静态方法与实例方法
```java
class Difference {
    public static void main(String[] args) {
        display(); // 调用静态方法，无需对象
        Difference t = new Difference();
        t.show(); // 调用实例方法，需要对象
    }

    static void display() {
        System.out.println("Programming is amazing.");
    }

    void show() {
        System.out.println("Java is awesome.");
    }
}
```
### 接口 Interface
接口与接口，接口与类之间可以继承和拓展
![](https://s2.loli.net/2024/05/28/KZsRqFWpyzvXwNJ.png)
接口中可以通过**静态工厂**来实现类似构造器的作用，能够防止客户端直接接触到具体实现类![image.png](https://s2.loli.net/2024/05/28/L7iAYM3ckamQfU8.png)
`default`可以实现接口的统一功能，无需在各个实现类中重复实现
![image.png](https://s2.loli.net/2024/05/28/H8TXmKyMAJEflFS.png)
### 封装
1. 使用接口类型声明变量
2. 客户端仅使用接口中定义的方法
3. 客户端代码无法直接访问属性，通过封装get方法等来防止泄露。
4. private只能在当前类中访问，protected可允许子类访问，public允许任何类访问
### 继承和重写
final变量不允许重引用；final方法不允许重写，final类不允许拓展继承
📕**抽象的思想**：抽象，意思是提取共同特征，你不了解每一个的具体，但是你了解他们的抽象。例如，抽象类接口，则它是提取的特征，所有人都该实现它。
#### Overriding
完全相同的Signature，使用哪个运行时决定
父类型三种情况：
1. 被重写函数体不为空，大多数子类可复用，也可以重写
2. 函数实现体为空，则子类型需要这个功能时需要重写
3. 如果该方法为抽象方法，其没有实现体，则所有子类都需要实现
在重写中，可通过super来利用父类的功能。但注意如果调用**父类的构造器**，必须是实现体的第一调语句
#### 抽象类
至少包含一个抽象方法，可以有属性
抽象方法必须没有实现体
#### 多态

# 小结
软件构造固然是一门概念很多的课，但其重点在于其背后蕴藏的思想，我们称其为“一个程序猿的自我修养”。生活中很多事情何尝不是如此：不识庐山真面目，只缘身在此山中。不必拘泥于考试背背背的束缚，而是参悟其背后的道理；不必拘泥于眼前的苟且，向更远大的方向去走，总一天回头时，发现原来已经在明灯的指引下走了很远的路。让这门课成为程序猿之路的领路人，是课程的目的，也是我们应领悟的道理。
