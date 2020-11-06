### 树莓派内网穿透工具

1. 登录网站ttps://my.zerotier.com,创建帐号
2. 进入https://my.zerotier.com/network点击Create创建网络,记住Network ID

#### 给树莓派安装客户端，操作如下：
- 在命令行内执行：`curl -s https://install.zerotier.com | sudo bash`，然后等待完成安装，会出现*** Success! You are ZeroTier address [ d896f5fb8d ]，这一行表示安装成功
- 下载安装完成后，通过指令：sudo zerotier-cli join <公网上的Network ID> 连接服务器
- 通过指令：sudo zerotier-cli listnetworks，可以查看连接网络


#### 给电脑安装客户端，操作如下：
- 下载客户端安装包，https://download.zerotier.com/dist/ZeroTier%20One.msi，并安装下载的软件

- 






