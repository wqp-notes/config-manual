# Mosquitto安装及配置

## Mosquitto安装

#### CentOS环境安装
1. 在root用户下更新gcc命令，执行：`yum -y update gcc`
2. 在root用户下安装g++命令，执行：`yum -y install gcc+ gcc-c++`
3. 下载源代码包，执行：`wget http://mosquitto.org/files/source/mosquitto-1.6.7.tar.gz`
4. 进入到下载目录解压，执行：`tar zxvf mosquito-1.6.7.tar.gz`
5. 进入目录，执行：`cd mosquito-1.6.7`
6. 进行编译，执行：`make`
7. 进行安装，执行：`sudo make install`
> 补充：安装过程无出错，表示安装成功


#### RaspberryPi环境安装
1. 略


## Mosquitto配置

### 配置用户、密码、主题访问控制
说明：
> allow_anonymous 允许匿名
> password_file 密码文件
> acl_file 访问控制文件





