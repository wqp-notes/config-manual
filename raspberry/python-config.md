## Python相关配置

#### Python项目打包配置
1. 在项目根目录编写setup.py文件,如下所示：
```
#!/usr/bin/python3
# _*_ coding: UTF-8 _*_

# 打包执行命令：python setup.py sdist
from distutils.core import setup

setup(
    name='aiot-edge-sensor',
    version='0.0.1',
    description='aiot edge sense gather package',
    license='MIT',
    author='wqp',
    author_email='907898929@qq.com',
    url='None',
    packages=['source.main.core',
              'source.main.core.common',
              'source.main.core.dsl8b20',
              'source.main.core.dht11',
              'source.main.core.pzem',
              'source.main.core.wristband']
)
```
2. 补充：
  > 打包分为2类：
  > 第1类：sdist（source distribution）
   `sdist使用方式：$ python setup.py sdist --formats=gztar,zip`
  
  > 第2类：bdist（built distribution）
   `bdist使用方式：$ python setup.py bdist --formats=rpm`
