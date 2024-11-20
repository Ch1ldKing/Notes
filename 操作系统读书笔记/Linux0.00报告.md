# Problem
![2cd82d29d4dca90750a5dfaaddb87d3.png](https://s2.loli.net/2024/11/19/hC4QdJiVpF8Zucm.png)
修改一下出问题的代码，在config.cc中
![image.png](https://s2.loli.net/2024/11/19/rJVmTxjyOQp1AE2.png)
然后就可以正常编译了
sudo make，sudo make install
![image.png](https://s2.loli.net/2024/11/19/u19QpmKVXzSZcMW.png)
# Step
## 1. 编译Linux000
1. 先安装as86编译程序`sudo apt install bin86`
2. 在linux000 code下执行`sudo make`，得到image文件，把它移动到/linux000下，和bochs配置文件放在一起
   ![image.png](https://s2.loli.net/2024/11/20/BLhnMe4bk1HyZrV.png)
## 2. z