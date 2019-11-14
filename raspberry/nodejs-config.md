## 树莓派NodeJs安装配置

### 1. NodeJs安装
1. 在<a href="https://nodejs.org/en/download/">官网下载Linux Binaries (ARM)对应的的包</a> ;
   或者在指定目录下执行 `wget https://nodejs.org/dist/v8.12.0/node-v8.12.0-linux-armv7l.tar.xz`
2. 使用命令tar -xvf node-v8.12.0-linux-armv7l.tar.xz解压文件
3. 进入node解压目录下的bin目录, `ls -l查看当前文件夹下是否有名叫 node 的可执行文件；./node -v 查看nodejs的版本；./npm -v 查看npm的版本`

### 2. NodeJs配置
1. 配置node链接:
  * 第1种配置node环境变量
  * 第2种配置链接方式：  
    `sudo ln /home/pi/node/bin/node /usr/local/bin/node 配置node的链接`  
    `sudo ln -s /home/pi/node/lib/node_modules/npm/bin/npm /usr/local/bin/npm 配置npm的链接`
2. 测试node配置:
  * 换个文件夹执行node -v会输出node版本信息
  * 若执行 npm –v可能会报错，如Error: Cannot find module '/usr/local/bin/node_modules/npm/bin/npm-cli.js，如果如果报错了需要修改npm链接内容：sudo nano /usr/local/bin/npm ，然后把两处相对路径 $basedir 改成绝对路径 /home/pi/node， 如下所示：
  ```
  NODE_EXE="/home/pi/node/node.exe"
  NPM_CLI_JS="/home/pi/node/lib/node_modules/npm/bin/npm-cli.js"
  ```




