
#### 写在操作之前
1. python版本号3.8.0
2. tensorflow版本号1.15.0
3. 安装依赖:
 + pip install tf_slim

#### 安装tensorflow(cpu版本,gpu同样操作)

1. 首先打开网站https://tensorflow.google.cn/install/pip?hl=zh-CN 

2. 创建python虚拟环境，参照第1步打开的页面找到 创建虚拟环境（推荐）模块操作即可

2. 安装tensorflow, 参照第1步打开的页面滑动到页面最底部，打开软件包位置模块，选择当前电脑环境所支持的版本进行安装操作

3. 我安装windows cpu版本的方式是：pip install https://storage.googleapis.com/tensorflow/windows/cpu/tensorflow_cpu-2.3.0-cp38-cp38-win_amd64.whl 回车即可

4. 安装过程中其它方式均参照第2步打开的链接进行核实处理

#### 安装tensorflow detection api库(自定义数据并训练)

1. 在官方网站https://github.com/tensorflow/models/tags 下载最新版本的源码,下载到本地后进行解压操作,本人下载版本为2.3.0

2. 在官方网站https://github.com/protocolbuffers/protobuf/tags 下载最新版本的源码,下载到本地的进行解压操作,本人下载版本为3.14.0

3. 把protoc-3.14.0-win64包里面bin目录下的protoc.exe复制到models/research目录粘贴

4. 在models/research目录下打开命令行窗口,执行protoc object_detection/protos/*.proto --python_out=.  如果不报错那就没问题，models\research\object_detection\protos下会出现对应的python文件

5. 在python虚拟环境目录下的site-package目录下新增tensorflow_model.pth文件(python环境在启动时会默认添加文件内的环境变量)，文件内容如下：
```
D:\software\ai\workspace\tensorflow\models-2.3.0\research
D:\software\ai\workspace\tensorflow\models-2.3.0\research\object_detection
D:\software\ai\workspace\tensorflow\models-2.3.0\research\slim
```


6. 安装Tensorflow model以及slim

 + 进入到models\research> python setup.py build
 + 进入到models\research> python setup.py install
 + 进入到models\research> cd slim
 + 进入到models\research\slim> python setup.py build
 + 进入到models\research\slim> python setup.py install
 + 安装完成后执行：python .\object_detection\builders\model_builder_test.py测试，如果没有报错，则说明安装成功

  


