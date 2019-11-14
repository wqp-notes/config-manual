### Linux目录结构
1. bin (binaries)存放二进制可执行文件
2. sbin (super user binaries)存放二进制可执行文件，只有root才能访问
3. etc (etcetera)存放系统配置文件
4. usr (unix shared resources)用于存放共享的系统资源
5. home 存放用户文件的根目录
6. root 超级用户目录
7. dev (devices)用于存放设备文件
8. lib (library)存放跟文件系统中的程序运行所需要的共享库及内核模块
9. mnt (mount)系统管理员安装临时文件系统的安装点
10. boot 存放用于系统引导时使用的各种文件
11. tmp (temporary)用于存放各种临时文件
12. var (variable)用于存放运行时需要改变数据的文件

### Linux文件权限
linux文件权限描述格式解读：**`-rwxrw-r--`**
1. r 可读权限，w可写权限，x可执行权限（也可以用二进制表示 111 110 100 --> 764）
2. 第1位：文件类型（d 目录，- 普通文件，l 链接文件）
3. 第2-4位：所属用户权限，用u（user）表示
4. 第5-7位：所属组权限，用g（group）表示
5. 第8-10位：其他用户权限，用o（other）表示
6. 第2-10位：表示所有的权限，用a（all）表示

### Linux常用命令
说明 **`命令格式：命令 -选项 参数（选项和参数可以为空）`**
1. 切换用户 `su root`
2. 关机 `shutdown -h now`
3. 重启 `reboot`
4. 查看操作系统位数 `getconf LONG_BIT`
5. 查看系统主机名称 `hostnamectl`
6. 配置文件/目录权限 `chmod a+r,u+w,u+x,g+w 文件名/目录, 示例：chmod a+r,u+w,u+x,g+w a.txt`
7. 创建目录 `mkdir 目录名, 示例：mkdir directory`
8. 创建文件 `touch /home/a.txt`
9. 删除当前目录,包含子目录 `rm -rf *`
10. 删除当前目录下所有文件和目录 `rm -r *`
11. 文件复制命令cp：
```
命令格式：cp [-adfilprsu] 源文件(source) 目标文件(destination)
              cp [option] source1 source2 source3 ...  directory
    参数说明：
    -a:是指archive的意思，也说是指复制所有的目录
    -d:若源文件为连接文件(link file)，则复制连接文件属性而非文件本身
    -f:强制(force)，若有重复或其它疑问时，不会询问用户，而强制复制
    -i:若目标文件(destination)已存在，在覆盖时会先询问是否真的操作
    -l:建立硬连接(hard link)的连接文件，而非复制文件本身
    -p:与文件的属性一起复制，而非使用默认属性
    -r:递归复制，用于目录的复制操作
    -s:复制成符号连接文件(symbolic link)，即“快捷方式”文件
    -u:若目标文件比源文件旧，更新目标文件
    如将/test1目录下的file1复制到/test3目录，并将文件名改为file2,可输入以下命令：
    cp /test1/file1 /test3/file2
```
12. 文件移动命令mv:
```
命令格式：mv [-fiv] source destination
    参数说明：
    -f:force，强制直接移动而不询问
    -i:若目标文件(destination)已经存在，就会询问是否覆盖
    -u:若目标文件已经存在，且源文件比较新，才会更新
    如将/test1目录下的file1复制到/test3 目录，并将文件名改为file2,可输入以下命令：
    mv /test1/file1 /test3/file2
```
13. 文件删除命令rm:
```
命令格式：rm [fir] 文件或目录
    参数说明：
    -f:强制删除
    -i:交互模式，在删除前询问用户是否操作
    -r:递归删除，常用在目录的删除
    如删除/test目录下的file1文件，可以输入以下命令：
    rm -i /test/file1

```
14. 解压tar类型文件:
```
tar zxvf ./jdk-8u2210linux-arm64-vfp-hflt.atr.gz –C /aiot/java/
  -C:解压的文件从当前目录复制到指定目录
```




