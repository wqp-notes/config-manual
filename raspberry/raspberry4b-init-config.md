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
  3.2. 将第1行修改为:  
  deb http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ buster main contrib non-free rpi
  deb-src http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ buster main contrib non-free rpi
```
4. 修改系统更新源配置:
```
  4.1. sudo nano /etc/apt/sources.list.d/raspi.list
  4.2. 将第1行修改为:  
  deb http://mirrors.tuna.tsinghua.edu.cn/raspberrypi/ stretch main ui
  deb-src http://mirrors.tuna.tsinghua.edu.cn/raspberrypi/ stretch main ui
```
5. 同步更新源
  `sudo apt-get update`
6. 对已安装软件包进行更新及升级
  `sudo apt-get upgrade`

#### 更改pip源（可选）
1. ssh方式登录树莓派
2. 
```
新建~/.pip/pip.conf文件，写入其地址。阿里云、中科大、豆瓣等都有pip源。
[global]
index-url = http://pypi.douban.com/simple/
```

### 安装中文字体
```
安装中文字体
sudo apt-get install ttf-wqy-zenhei ttf-wqy-microhei
```

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
1. 略


### 4. 开机自启动配置
1. 编写launcher.sh文件，文件内容如下：
```
#!/bin/bash
export JAVA_HOME=/aiot/java/jdk1.8.0_221
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
nohup java -jar /aiot/server/aiot-edge-stream-0.0.1-SNAPSHOT.jar > /aiot/server/java-nohup.out 2>&1 &
nohup python3 /aiot/server/aiot-edge-sensor-0.0.1/source/main/core/launcher.py > /aiot/server/python-nohup.out 2>&1 &
```
2. 将该脚本文件标记为可执行文件，在脚本当前目录下执行
`chmod a+x launcher.sh`
3. 进入到cd /etc/目录，打开nano rc.local文件，在文件的**底部exit 0之前**添加: /绝对路径/launcher.sh，重启reboot



### 5. VNC开启配置
1. 官方系统开启vnc:
  * 临时开启执行 `vncserver即可，会临时分配连接ip+端口地址供连接使用`
  * 永久开启执行 `sudo raspi-config，选择5 Interfacing Options回车，再选择“VNC”回车；最后点选“Finish”完成即可`
  * 在vnc客户端直接输入ip就可以连接成功
2. 非官方系统开启vnc:
  * <a href="https://www.realvnc.com/en/connect/download/vnc/">vnc服务端下载</a> ,然后进行vncserver安装，注意操作系统及版本
  * 服务端安装成功并开启vncserver服务
  * 打开vnc客户连接vnc服务即可
  

### 6. 开机自启动chromium浏览器，并打开指定地址
1. 在 /usr/share/xsessions目录下创建一个新的chromium.desktop文件,内容如下：
```
[Desktop Entry]
Name=Chromium
Comment=Iot Data Gather Chromium
Type=Application
Exec=/usr/lib/chromium-browser --no-first-run –start-maximized –start-fullscreen --kiosk-printing --kiosk http://app-health.daanlab.com/edgeSenseWindow/index.html#/dashboard?boxId=YK20191112ED001
```








