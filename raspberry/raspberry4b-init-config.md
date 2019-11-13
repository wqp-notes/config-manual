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
```
1. sudo passwd root
2. sudo passwd --unlock root
3. 进入/etc/ssh/目录，打开nano sshd_config文件，将文件内PermitRootLogin without-password修改为PermitRootLogin yes并保存
```

4. 

#### 更改软件源（可选）


#### 更改pip源（可选）



### 2. JDK安装



### 3. AIOT程序部署


### 4. 开机自启动配置






