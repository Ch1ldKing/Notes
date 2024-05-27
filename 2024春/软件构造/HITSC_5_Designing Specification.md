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