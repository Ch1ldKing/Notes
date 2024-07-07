Classification(Supervised)标记数据训练，非标数据预测
Naive Bayes Classification assume that conditional independence parameter, making the training easy and fast
![image.png](https://s2.loli.net/2024/07/03/xXI2a8etmJKgqfh.png)
as you see ,we assumed that the parameters are independent , so we are able to calculate conditional probabilities like this, you can calculate the conditional probabilities just with Prior separately
![image.png](https://s2.loli.net/2024/07/03/XH1kKm7frSdpzLi.png)
我昨天完成了培训课程的MVC构建，遇到的问题是课程项和计划的关联以及课程项如何包含多个subject，解决方法是将计划和课程项的id进行关联，并在计划中对于课程项进行重命名。把subject建立多对一关系。今天我要完成的是对培训报名的MVC构建