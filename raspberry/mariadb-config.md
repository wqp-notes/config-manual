### mariadb数据安装

1. 第一步如下：
```
apt-get update 更新软件源中的所有软件列表。 
apt-get upgrade 更新软件。 
apt-get dist-upgrade 更新系统版本。如果你对新版本软件的需求不是那么迫切，***可以不执行***
```

2. 第二步执行：apt-get install mariadb-server，数据库版本为源中最新版本

3. 测试是否安装成功,会出现如下内容：
``` 
[root@raspberrypi ~]$ mysql
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 50
Server version: 10.3.22-MariaDB-0+deb10u1 Raspbian 10
Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
MariaDB [(none)]>
```
4. 进入数据库进行修改root账号密码：
```
MariaDB [(none)]> use mysql;
MariaDB [mysql]> update user set plugin='mysql_native_password' where user='root';
MariaDB [mysql]> UPDATE user SET password=PASSWORD('123456') WHERE user='root';
MariaDB [mysql]> flush privileges;
exit
```
更改完后重启数据库：systemctl restart mariadb

5. 配置MariaDB可远程连接
5.1 打开目录/etc/mysql/mariadb.conf.d/50-server.cnf的文件，然后注释掉下面bind-address这一行：
```
# Instead of skip-networking the default is now to listen only on
# localhost which is more compatible and is not less secure.
# bind-address            = 127.0.0.1
```
5.2 再进入数据库操作如下：
```
$ mysql -u root -p
$ 输入密码
MariaDB [(none)]> use mysql;
MariaDB [mysql]> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION;
MariaDB [mysql]> flush privileges;
exit
```
5.3 修改完后执行重启命令：systemctl restart mariadb即可




