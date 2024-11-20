# Problem
![2cd82d29d4dca90750a5dfaaddb87d3.png](https://s2.loli.net/2024/11/19/hC4QdJiVpF8Zucm.png)
修改一下出问题的代码，在config.cc中
![image.png](https://s2.loli.net/2024/11/19/rJVmTxjyOQp1AE2.png)
然后就可以正常编译了
sudo make，sudo make install
![image.png](https://s2.loli.net/2024/11/19/u19QpmKVXzSZcMW.png)
# Step
## 1. 安装bochs
实验环境：ubuntu 22.04.5+bochs 2.6
1. 安装依赖`sudo apt-get install build-essential xorg-dev libgtk2.0-dev`
2. 清除构建缓存`make dist-clean`再执行配置脚本`./configure --enable-debugger --enable-disasm --enable-debugger-gui` 得到Makefile文件
3. 执行sudo make。遇到报错
![2cd82d29d4dca90750a5dfaaddb87d3.png](https://s2.loli.net/2024/11/19/hC4QdJiVpF8Zucm.png)
修改一下出问题的代码，在config.cc中
![image.png](https://s2.loli.net/2024/11/19/rJVmTxjyOQp1AE2.png)
然后就可以正常编译了
sudo make，sudo make install
![image.png](https://s2.loli.net/2024/11/19/u19QpmKVXzSZcMW.png)

4. 输入 bochs --help，成功安装
## 2. 编译Linux000
1. 先安装as86编译程序`sudo apt install bin86`
2. 在linux000 code下执行`sudo make`，得到image文件，把它移动到/linux000下，和bochs配置文件放在一起
   ![image.png](https://s2.loli.net/2024/11/20/BLhnMe4bk1HyZrV.png)
## 3. 执行bochs仿真程序
1. 先了解.bxrc文件，这是bochs的配置文件