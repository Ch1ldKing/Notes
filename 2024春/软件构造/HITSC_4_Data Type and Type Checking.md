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
❓检查些啥
