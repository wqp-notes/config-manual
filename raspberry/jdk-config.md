1. 到官网下载jdk,注意是aarch版本

2. 上传jdk安装包到/usr/local/src/目录下

3. 在/usr/local/目录下新建java目录

4. 解压上传的tar.gz包到/usr/local/java/目录下,执行命令：tar -zxvf jdk-11.0.9_linux-aarch64_bin.tar.gz -C /usr/local/java/

5. 配置jdk环境

5.1 进入/etc/profile.d/目录下，并新建java.sh文件

5.2 编辑java.sh文件，添加发下内容：
```
#这里需要改为自己jdk的解压路径
export JAVA_HOME=/usr/local/jdk1.8.0_241 
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=.:JAVA_HOME/lib/dt.jar:JAVA_HOME/lib/tools.jar
```

5.3 执行命令source /etc/profile,使新添加的java.sh脚本生效

5.4 输入java --version查看环境配置是否已正常





