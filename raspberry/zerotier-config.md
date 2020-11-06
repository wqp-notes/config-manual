### 树莓派内网穿透工具

1. 登录网站ttps://my.zerotier.com,创建帐号
2. 进入`https://my.zerotier.com/network`点击Create创建网络,记住Network ID

#### 给树莓派安装客户端，操作如下：
- 在命令行内执行：`curl -s https://install.zerotier.com | sudo bash`，然后等待完成安装，会出现*** Success! You are ZeroTier address [ d896f5fb8d ]，这一行表示安装成功
- 下载安装完成后，通过指令：sudo zerotier-cli join <公网上的Network ID> 连接服务器
- 通过指令：sudo zerotier-cli listnetworks，可以查看连接网络，公网会为当前设备分配固定ip地址
- 查看zerotier网络状态指令：netstat -lntp|grep zerotier

#### 配置开机启动zerotier
- 编辑 vim /etc/rc.local,//在 exit 0  所在行之前前添加代码:zerotier-one -d 直接启动程序即可


#### 给电脑安装客户端，操作如下：
- 下载客户端安装包，https://download.zerotier.com/dist/ZeroTier%20One.msi，并安装下载的软件
- 打开软件配置公网上的Network ID即可，公网会为当前设备分配固定ip地址






