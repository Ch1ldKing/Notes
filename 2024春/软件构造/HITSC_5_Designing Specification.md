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
### 强度
前置条件更弱且后置条件更强
也可能无法比较：
+ 存在某些实现同时满足 𝑆1​ 和 𝑆3​，也存在某些实现只满足 𝑆1​ 或 𝑆3​
+ 没有实现同时满足两者
## 欠定/确定规约
确定：给定一个满足precondition的输入，其输出是**唯一的、明确的**
欠定：同一个输入可以有多个合法输出，通常有确定的实现
非确定：同一个输入，多次执行时得到的输出可能不同
## 操作式/声明式规约
操作式：例如伪代码，用它解释实现的细节。但最好不要这么做，把实现细节放在实现体内部注释而不是规约中
声明式：没有内部实现的描述，只有输入得到输出