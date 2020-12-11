
## 使用TensorFlow Object Detection Api 环境搭建、训练自定义的数据集、输出模型、使用模型目标检测

### 环境搭建

#### 写在操作之前（环境准备）

提前说明，本文件所描述的内容是基本windows环境，仅cpu方式操作整个流程

1. 操作系统windows10 64位

1. 安装python版本号3.8.0

2. 安装python相关依赖包
 + pip install tensorflow==1.15.0
 + pip install tf_slim==1.1.0
 + pip install pycocotools-windows==2.0.0.2
 + pip install numpy==1.19.3

3. 打开https://tensorflow.google.cn/install/pip?hl=zh-CN 认真阅读里面的内容，会有很大帮助

4. <strong style color='red'>在任务盘符内新建一个目录，示例：d:ai\tftest, 这个目录后面所有操作处理是基于这个目录</strong>

#### 新建python虚拟环境
这样做的目录是建立一个新的环境，在环境准备时添加的python相关依赖包都可以安装在这里面
 + 创建一个新的虚拟环境,在cmd里执行：``` python -m venv --system-site-packages '你自己新虚拟环境保存的目录'， 示例:python -m venv --system-site-packages 'd:\python\venvs\tensorflow115```
 
 + 激活虚拟环境,cmd 进入到 '你自己新虚拟环境保存的目录'\Scripts\activate
 
#### 安装tensorflow
 在环境准备阶段已正常安装了tensorflow可忽略这部分内容，这里仅描述注意事项
  + 只安装tensorflow 1.15.0版本，建议不要去安装tf 2.x的版本，原因是在后面执行model_main.py脚本时，里面有使用到trans_contrib模块，这个只在1.x里有，2.x里被移除了，所以安装版本时慎重
  + tf 1.15.0版本在安装时要选择支持cpu的版本，不要选择支持gpu的版本，原因是本人电脑没有gpu,如果你的电脑有gpu配置，可以选择gpu的版本

#### 下载TensorFlow Object Detection Api包
访问官方网站https://github.com/tensorflow/models/tags 下载最新版本的源码,下载到本地后进行解压操作,本人下载版本为2.3.0，建议保持一致
  + 也可以直接下载压缩文件，下载完成后直接解压到d:ai\tftest
  + 在官方网站https://github.com/protocolbuffers/protobuf/tags 下载最新版本的源码,下载到本地的进行解压操作,本人下载版本为3.14.0，下载后解压文件，把protoc-3.14.0-win64包里面bin目录下的protoc.exe复制到models/research目录
  + 在models/research目录下打开命令行窗口,执行protoc object_detection/protos/*.proto --python_out=.  如果不报错那就没问题，models\research\object_detection\protos下会出现对应的python文件
  + 切换到python虚拟环境目录下的site-package目录下新增tensorflow_model.pth文件(python环境在启动时会默认添加文件内的环境变量)，文件内容如下：
```
D:\software\ai\workspace\tensorflow\models-2.3.0\research
D:\software\ai\workspace\tensorflow\models-2.3.0\research\object_detection
D:\software\ai\workspace\tensorflow\models-2.3.0\research\slim
```
 + 编译、安装Tensorflow model以及slim
```
1.进入到models\research> python setup.py build
2.进入到models\research> python setup.py install
3.进入到models\research> cd slim
4.进入到models\research\slim> python setup.py build
5.进入到models\research\slim> python setup.py install
6.安装完成后执行：python .\object_detection\builders\model_builder_test.py测试，如果没有报错，则说明安装成功
```






