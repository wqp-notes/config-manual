
## 树莓派OpenCV安装

#### 1. 操作步骤
打开树莓派命令窗口执行  
```
pip3 install opencv-python
pip3 install opencv-contrib-python
```

#### 2. 编写代码测试安装
1. `import cv2`
2. 补充: 如果出现了错误如 `ImportError: libjasper.so.1: cannot open shared object file: No such file or directory`, 则按如下操作：
```
pip3 install opencv-contrib-python==3.3.0.9 -i https://www.piwheels.org/simple # 安装3309版本
sudo apt-get update #安装依赖库
sudo apt-get install libhdf5-dev
sudo apt-get install libatlas-base-dev
sudo apt-get install libjasper-dev
sudo apt-get install libqt4-test
sudo apt-get install libqtgui4
sudo apt-get update
 
python3
import cv2 # 检查导入成功
```



