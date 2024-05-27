###### 目标
规约，前置后置条件，欠定规约、非确定规约、陈述式、操作式规约、规约的强度及其比较
# 规约
#### 作用
1. 给自己和别人写出设计决策：如final、数据类型定义
2. 作为契约，服务端与客户端达成一致
3. 调用方法时双方都要遵守
4. 便于定位错误
5. 解耦，无需告诉客户端具体实现，变化也无需通知客户端，扮演**防火墙**角色
6. 判断行为等价性
## 内容
1. 输入输出的数据类型
2. 功能和正确性
3. 性能
## 行为等价性
站在客户端视角看一个行为是否具有等价性。如果两个函数都**符合同一个Spec**，则他们等价
## Spec的结构
1. **前置条件**：对客户端的约束，在使用方法时必须满足的条件，用`@param`，并用`@requires`进行说明
3. **后置条件**：对开发者的约束，方法结束时必须满足的条件，用`@return`和`@throws`，并用`@effects`进行描述
4. 契约：前置条件满足了，则后置条件必须满足
Spec不需要说明方法内部变量和类的私有方法或变量
😀方法内部尽量不要修改传入的参数，不要设计mutating的spec，容易引发错误。除非必须是mutator方法的spec，否则避免使用mutable的类与方法。
📕因为程序中很有可能有**多个变量指向同一个可变对象**（别名），在类的实现体或客户端**保存别名**的情况下，可能导致修改并产生bug
同时避免使用可变的全局变量
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
# 设计规约
## 对规约分类
通过规约的<mark style="background: #BBFABBA6;">确定性、陈述性、和强度</mark>来判断“哪个更好”
### 按强度
前置条件更弱且后置条件更强
也可能无法比较：
+ 存在某些实现同时满足 𝑆1​ 和 𝑆3​，也存在某些实现只满足 𝑆1​ 或 𝑆3​
+ 没有实现同时满足两者
### 欠定/确定规约
确定：给定一个满足precondition的输入，其输出是**唯一的、明确的**
欠定：同一个输入可以有多个合法输出，通常有确定的实现
非确定：同一个输入，多次执行时得到的输出可能不同
### 操作式/声明式规约
操作式：例如伪代码，用它解释服务端实现的细节。但最好不要使用它，把实现细节放在实现体内部注释而不是规约中
声明式：没有内部实现的描述，只有输入得到输出
🌰例子：![image.png](https://s2.loli.net/2024/05/27/uCVDdlh7ESWUqiQ.png)
第一个说了传到一个新的类，但这是具体实现细节
第二个说了遍历所有元素，这也是具体实现细节
## 图例规约
规约限定了范围，可选择落在规约中的任意具体实现![image.png](https://s2.loli.net/2024/05/27/dUwMg2x4pasChIn.png)
更强的规约表示为更小的区域。比如更强的后置条件、更弱的前置条件都意味着实现的自由度更低![image.png](https://s2.loli.net/2024/05/27/RxC4LrI6HmskEwD.png)
## 设计好的规约
好的方法：并非代码好，而是spec的设计，使client用着舒服，开发者编着舒服
### 1.内聚的
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
### 2.信息丰富的
不能引起客户端的歧义
🌰例子：客户端不知道返回null是因为原来绑定的值是null，还是因为不存在旧值
```Java
static V put(Map<K,V> map, K key, V val)
/**
* requires: `val`可以为`null`，`map`可以包含`null`值
* effects: 将`(key, val)`插入到映射中，如果存在相同的键，则覆盖旧值。返回该键的旧值，如果不存在旧值，则返回`null`
*/
```
### 3.足够强
太弱的spec，client不放心、不敢用 (因为没有给出足够的承诺)。
开发者应尽可能考虑各种特殊情况，在post-condition给出处理措施
🌰例子：客户端在得到Exception的时候，不知道哪些元素被添加了，需要自己定位。应该完善exception
```Java
static void addAll(List<T> list1, List<T> list2)
effects: adds the elements of list2 to list1,
         unless it encounters a null element,
         at which point it throws a NullPointerException
```
### 4.太强实现难度大
这个没啥说的
### 5.使用抽象类型
给方法实现体与客户端更大的自由度
```java
static ArrayList<T> reverse(ArrayList<T> list)
effects: returns a new list which is the reversal of list, i.e.
		 newlist[i] == list[n-i-1]
		 for all 0 <= i < n, where n = list.size()
```
### 前置条件/后置条件的一些问题
❓是否检验前置条件：通常来说，使用方法检验前置条件，成本昂贵
因此，通常选择使用前置条件，把责任交给client
❓减弱前置条件：客户端不喜欢太强的前置条件，因此通常是减弱，并用抛出异常来替代，并且要尽可能在错误根源处fail，避免错误扩散，难以定位
❓是否使用前置条件：(1) check的代价；(2) 方法的使用范围
+ 如果只在类的内部使用该方法(private)，那么可以不使用前置条件，在使用该方法的各个位置进行check——责任交给内部client；
+ 如果在其他地方使用该方法(public)，那么必须要使用前置条件，若client端不满足则方法抛出异常。
