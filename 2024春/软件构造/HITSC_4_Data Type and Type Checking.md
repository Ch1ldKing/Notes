###### 目标
静态/动态，可变/不变，Snapshot图，集合类，NULL
# 数据类型
## 基本数据类型
没啥好说的
## 对象数据类型
String，Integer等
区别![image.png](https://s2.loli.net/2024/05/26/9EFUAtrvbKayho3.png)
### Boxed primitives
将基本类型**包装**为对象类型，但不常用，通常是为了集合类而用
### 层次结构
对象结构有extends（继承）等关系
### 操作
既可以是普通的运算符，也可以是Object对象的方法或函数，比如`bigint1.add(bigint2)`add()是一个方法
# Static vs. dynamic data type checking
## 类型检查
检查数据类型和值是否符合
+ 静态类型在编译阶段进行类型检查
+ 动态类型在运行阶段进行类型检查
注意，类似如下的东西会报错，需要转换：
```Java
String five = 5; // ERROR!
```
```
ERROR:
test.java.2: incompatible types 
found: int
required: java.lang.String
String five = 5;
```
### 静态类型检查
👍静态类型检查在编译阶段就发现错误，避免将bug带入运行阶段，提高正确性健壮性
❓检查些啥
+ 语法错误
+ 类名错误
+ 参数数量
+ 参数类型
+ 返回值类型
### 动态类型检查
动态类型在运行阶段，才能根据当前确定的值进行检查，其错误由Exception给出
❓检查些啥
+ 非法参数值
+ 非法返回值
+ 越界index
+ 空指针
# 可变/不可变数据
❓首先，**赋值**的本质：在内存特定区域开辟一段空间，写入特定值，再将该空间与变量（引用）关联到一起（即先写值，再讲变量与内存关联起来）
## 改变的问题
+ 改变一个变量：将变量指向另一个值的存储空间，本质是更改了关联
  `String a = “abc”; a = “def”;`
+ 改变一个变量的值：将存储空间中的值重新写入`Date a = new Date(2024, 1, 1); a.setMonth(2);`
我们尽量避免变化，以免带来问题
## Immutability
一种重要的设计原则：不变性
不变数据类型：一旦被创建，其值不能改变
🌰例子：其实最终，a对应的值是变成def了，但我们所说的不变是，“abc”在内存中的值不变化。只是把a指向def了
![image.png](https://s2.loli.net/2024/05/27/NlkCZeqhbaDjX5B.png)
不同的是StringBuilder，他可以修改内存空间里的值![](https://s2.loli.net/2024/05/27/zPJxpsVTHjo6iWZ.png)
❓有什么区别？值不是一样吗？有多个变量引用同一个内存空间的时候，会出现不同：![](https://s2.loli.net/2024/05/27/kl4XEjMsmxDT3u6.png)
#### 优劣
不可变类型虽然保证安全和设计，但对其频繁修改会产生大量的临时拷贝(需要垃圾回收)
可变类型：
1. 最少化拷贝以提高效率
2. 使用可变数据类型，可获得更好的性能
3. 适合于在多个模块之间共享数据
**安全性**：![image.png](https://s2.loli.net/2024/05/27/uyPBLNFAeSdGf4Y.png)

### final
😀所以引入final，不能改变指向关系。如果这是不变数据类型，则添加final的字段的值更改会报错。
因此，尽量使用final来作为方法的输入参数和局部变量，表明了这个值不会被更改
📕final类无法派生子类，final变量不能改变值和引用（引用就是关联），final的方法不能被子类重写
#### final与Immutability的区别
不变对象：指向值不能被修改
可变对象：**有方法时**可以修改指向的值

不变引用：变量与内存空间的关联关系不可修改
可变引用：可以修改


