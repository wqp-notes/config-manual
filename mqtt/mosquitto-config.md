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
`allow_anonymous 允许匿名`
`password_file 密码文件`
`acl_file 访问控制文件`

1. 打开mosquitto.conf文件，找到allow_anonymous节点，修改后：allow_anonymous false
2. 配置用户名、密码：在配置文件找到password_file节点，修改后：password_file /home/appuser/mosquitto/pwfile（pwfile为自定义文件,路径为自定义路径）
3. 配置用户、主题：在配置文件找到acl_file节点，修改后：acl_file /home/appuser/mosquitto/aclfile（aclfile为自定义文件,路径为自定义路径）
4. 在配置文件找到#user mosquitto,下1行添加user root
5. 编写pwfile文件内容：
`mosquitto_passwd –c /home/appuser/mosquitto/pwfile admin（admin为用户名，按回车输入密码）`
`注意：在添加第二个用户时不再添加-c`
6. 编写aclfile文件内容：
```
# This only affects clients with username "aiot_pub_client"
user aiot_pub_client
topic write aiot/#

# This only affects clients with username "aiot_sub_client"
user aiot_sub_client
topic read aiot/#
```
7. 补充:
  * 编译找不到openssl/ssl.h；【解决方法】——安装openssl
  * 编译过程找不到ares.h；【解决方法】——sudo apt-get install libc-ares-dev
  * 编译过程找不到uuid/uuid.h；【解决方法】——sudo apt-get install uuid-dev
  * 使用过程中找不到libmosquitto.so.1；提示：error while loading shared libraries: libmosquitto.so.1: cannot open shared object file: No such file or directory；【解决方法】——修改libmosquitto.so位置：
  > 创建链接：sudo ln -s /usr/local/lib/libmosquitto.so.1 /usr/lib/libmosquitto.so.1
  > 更新动态链接库：sudo ldconfig
  * make: g++：命令未找到；【解决方法】——sudo apt-get install g++
  * 启动mosquitto,执行:`mosquitto [–c 配置文件绝对路径] [-d daemon] [-p port] [-v verbose]`
  








