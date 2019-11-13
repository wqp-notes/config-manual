# 树莓派4b初始化配置

### 1. 系统部分配置

#### 烧录镜像
1. 下载官方镜像：<p><a href ="https://www.raspberrypi.org/downloads/raspbian/">树莓派官方镜像地址</a></p>
2. 准备好tf卡+win32diskimager软件工具进行烧录镜像到tf卡中

#### 无线网络配置
1. 在本地创建wpa_supplicant.conf文件
2. 文件内容如下所示：
```
country=CN
	ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
	update_config=1
	network={
    		ssid="wi-fi name here"
    		psk="wi-fi password here"
    		key_mgmt=WPA-PSK
	}
	network={
    		ssid="another wi-fi name here"
    		psk="another wi-fi password here"
    		key_mgmt=WPA-PSK
	}
```
**注意：如果无线网没有密码或者采用WEP加密方式的话，key_mgmt应设为NONE，密码字段由psk改成wep_key0即可**

3. 开启ssh功能：
 `在boot目录下新建ssh文件(无后缀名),启动树莓派可开启ssh功能`
4. 首次启动不清楚树莓派网络连接ip时，可使用局域网扫描工具找到树莓派的ip地址（扫描工具：advanced_ip_scanner）

#### ROOT用户配置
1. 树莓派首次登录帐号/密码
`pi/raspberry`
2. 修改树莓派pi帐号密码
`sudo passwd pi`
3. 修改树莓派系统root帐号密码
  > 1. sudo passwd root
  > 2. sudo passwd --unlock root
  > 3. 进入cd /etc/ssh/目录，打开nano sshd_config文件，将文件内PermitRootLogin without-password修改为PermitRootLogin yes并保存
4. 修改系统环境变量：cd /etc/profile

#### 更改软件源（可选）
1. ssh方式登录树莓派
2. 备份源文件：
```
  sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
  sudo cp /etc/apt/sources.list.d/raspi.list /etc/apt/sources.list.d/raspi.list.bak
```
3. 修改软件更新源配置：
```
  3.1. sudo nano /etc/apt/sources.list
  3.2. 将第1行修改为：deb http://mirrors.ustc.edu.cn/raspbian/raspbian/ buster main contrib non-free rpi
```
4. 修改系统更新源配置:
```
  4.1. sudo nano /etc/apt/sources.list.d/raspi.list
  4.2. 将第1行修改为：deb http://mirrors.ustc.edu.cn/archive.raspberrypi.org/debian/ buster main ui
```
5. 同步更新源
  `sudo apt-get update`
6. 对已安装软件包进行更新及升级
  `sudo apt-get upgrade`

#### 更改pip源（可选）
1. ssh方式登录树莓派
2. 略

### 2. JDK安装
1. ssh方式登录树莓派
2. 进入cd /etc/目录,打开nano profile文件
3. 在文件末尾加入：
```
  export JAVA_HOME=/usr/share/jdk1.8.0
  export PATH=$JAVA_HOME/bin:$PATH
  export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
  **注意要把当前目录 “.”，也添加进去**
```
4. 补充：
```
  在安装openjava时，如果系统已安装过openjava,可执行update-alternatives --config java查看安装的目录，树莓派4b系统默认安装openjdk路径在：/usr/lib/jvm/java-11-openjdk-arm64/bin/java
```

### 3. AIOT程序部署


### 4. 开机自启动配置






