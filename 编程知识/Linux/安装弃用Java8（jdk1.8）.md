Debian 11 弃用 java8
## 官网下载安装包
https://www.java.com/en/download/manual.jsp
## 解压
```
cd /usr/local/lib //解压到文件夹命令
mkdir java //新建文件夹
chmod 7777 java //更改文件夹权限
tar -zxvf jdk-8u231-linux-x64.tar.gz -C /usr/local/lib/java //解压到文件夹命令
```
## 配置环境变量
`vim /etc/profile`
```
export JAVA_HOME=/usr/local/lib/java/jdk1.8.0_231 
export JRE_HOME=${JAVA_HOME}/jre  
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib  
export  PATH=${JAVA_HOME}/bin:$PATH
```

## 重新执行profile文件
`source `