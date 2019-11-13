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
linux文件权限描述格式解读：
`-rwxrw-r--`
1. r 可读权限，w可写权限，x可执行权限（也可以用二进制表示 111 110 100 --> 764）
2. 第1位：文件类型（d 目录，- 普通文件，l 链接文件）
3. 第2-4位：所属用户权限，用u（user）表示
4. 第5-7位：所属组权限，用g（group）表示
5. 第8-10位：其他用户权限，用o（other）表示
6. 第2-10位：表示所有的权限，用a（all）表示







