### nginx安装

1. 第一步如下：
```
apt-get update 更新软件源中的所有软件列表。 
apt-get upgrade 更新软件。 
apt-get dist-upgrade 更新系统版本。如果你对新版本软件的需求不是那么迫切，***可以不执行***
```
2. 第二步执行：apt-get install nginx, 当前版本是1.14.2

3. 查看nginx安装的版本:nginx -V

4. 在浏览器中输入http://ip地址:80回车即可看到nginx主页面

5. 默认安装nginx相关目录说明：
```
默认的网站根目录：/var/www/html
nginx配置文件目录：/etc/nginx/
nginx主配置文件位置：/etc/nginx/nginx.conf

nginx常用管理命令：
启动nginx: sudo systemctl start nginx
关闭nginx：sudo systemctl stop nginx
设置nginx开机启动：sudo systemctl enable nginx
```

