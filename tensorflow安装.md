
## 使用TensorFlow Object Detection Api 环境搭建、训练自定义的数据集、输出模型、使用模型目标检测

### 一、环境搭建

#### 1、写在操作之前（环境准备）

提前说明，本文件所描述的内容是基本windows环境，仅cpu方式操作整个流程，实验的目录是训练自己收集的图片，使用tf object detection api包训练自己的模型，并使用新模型检测新的图片

1. 操作系统windows10 64位

1. 安装python版本号3.8.0

2. 安装python相关依赖包
 + pip install tensorflow==1.15.0
 + pip install tf_slim==1.1.0
 + pip install pycocotools-windows==2.0.0.2
 + pip install numpy==1.19.3

3. 打开https://tensorflow.google.cn/install/pip?hl=zh-CN 认真阅读里面的内容，会有很大帮助

4. <strong><font color='red'>在任务盘符内新建一个目录，示例：d:ai\tftest, 这个目录后面所有操作处理是基于这个目录</font></strong>

#### 2、新建python虚拟环境
这样做的目录是建立一个新的环境，在环境准备时添加的python相关依赖包都可以安装在这里面
 + 创建一个新的虚拟环境,在cmd里执行：``` python -m venv --system-site-packages '你自己新虚拟环境保存的目录'， 示例:python -m venv --system-site-packages 'd:\python\venvs\tensorflow115```
 
 + 激活虚拟环境,cmd 进入到 '你自己新虚拟环境保存的目录'\Scripts\activate
 
#### 3、安装tensorflow
 在环境准备阶段已正常安装了tensorflow可忽略这部分内容，这里仅描述注意事项
  + 只安装tensorflow 1.15.0版本，建议不要去安装tf 2.x的版本，原因是在后面执行model_main.py脚本时，里面有使用到trans_contrib模块，这个只在1.x里有，2.x里被移除了，所以安装版本时慎重
  + tf 1.15.0版本在安装时要选择支持cpu的版本，不要选择支持gpu的版本，原因是本人电脑没有gpu,如果你的电脑有gpu配置，可以选择gpu的版本

#### 4、下载TensorFlow Object Detection Api包
访问官方网站https://github.com/tensorflow/models/tags 下载最新版本的源码,下载到本地后进行解压操作,本人下载版本为2.3.0，建议保持一致
  + 也可以直接下载压缩文件，下载完成后直接解压到d:ai\tftest\models-2.3.0
  + 在官方网站https://github.com/protocolbuffers/protobuf/tags 下载最新版本的源码,下载到本地的进行解压操作,本人下载版本为3.14.0，下载后解压文件，把protoc-3.14.0-win64包里面bin目录下的protoc.exe复制到models/research目录
  + 在models/research目录下打开命令行窗口,执行protoc object_detection/protos/*.proto --python_out=.  如果不报错那就没问题，models\research\object_detection\protos下会出现对应的python文件
  + 切换到python虚拟环境目录下的site-package目录下新增tensorflow_model.pth文件(python环境在启动时会默认添加文件内的环境变量)，文件内容如下：
```
D:\ai\tftest\models-2.3.0\research
D:\ai\tftest\models-2.3.0\research\object_detection
D:\ai\tftest\models-2.3.0\research\slim
```
 + 编译、安装Tensorflow model以及slim
```
1.进入到models-2.3.0\research> python setup.py build
2.进入到models-2.3.0\research> python setup.py install
3.进入到models-2.3.0\research> cd slim
4.进入到models-2.3.0\research\slim> python setup.py build
5.进入到models-2.3.0\research\slim> python setup.py install
6.安装完成后执行：python .\object_detection\builders\model_builder_test.py测试，如果没有报错，则说明安装成功
```

### 二、训练自定义的数据集
#### 1、准备图片样本及标数据
 + 新建目录 mkdirs d:\ai\tftest\images, 在images目录下新建train、test 两个目录
 + 图片数据本人准备了120张，是jpg格式的图片，每张图片的尺寸不做要求，100张图片文件保存至d:\ai\tftest\images\train目录，另外20张保存到d:\ai\tftest\images\test目录
 + 打开精灵标注助手这个工具软件，如果没有自己去下载，下载链接 http://www.jinglingbiaozhu.com/
 + 我们采用工具软件标注好数据，小工具输出的文件是每张图片对应的xml文件
 + train目录、test目录下的图片都要处理好
#### 2、 xml格式数据转tensorflow支持的record格式
在转换的时候采用python脚本直接转换即可
 + 第1步xml转csv,脚本如下：
 ```python
 # xml格式转csv格式
import sys
import glob
import pandas as pd
import xml.etree.ElementTree as et


def xml_to_csv(_path, _out_file):
    xml_list = []
    _path = str(_path).replace('\\', '/')
    for xml_file in glob.glob(_path + '/*.xml'):
        tree = et.parse(xml_file)
        root = tree.getroot()
        for member in root.findall('object'):
            value = (root.find('filename').text,
                     int(root.find('size')[0].text),
                     int(root.find('size')[1].text),
                     member[0].text,
                     int(member[4][0].text),
                     int(member[4][1].text),
                     int(member[4][2].text),
                     int(member[4][3].text)
                     )
            xml_list.append(value)
    column_name = ['filename', 'width', 'height', 'class', 'xmin', 'ymin', 'xmax', 'ymax']
    xml_df = pd.DataFrame(xml_list, columns=column_name)
    xml_df.to_csv(_out_file, index=None)
    print('Successfully converted xml to csv')


if __name__ == '__main__':
    xml_to_csv(sys.argv[1], sys.argv[2])
 ```
 注：
 - 在执行脚本的时候，需要传递2个参数，第1个是图片xml文件所在的目录，第2个参数是输出的csv文件保存完整路径(包含文件后缀格式)，示例: python xml_2_csv.py d:\ai\tftest\images\train\outputs d:\ai\tftest\images\train\train.csv
 - test目录的xml同样需要为csv格式
 
 + 第1步csv转record,脚本如下：
 ```python
 # csv格式转record格式
import os, sys
import io
import pandas as pd
import tensorflow.compat.v1 as tf

from PIL import Image
from object_detection.utils import dataset_util
from collections import namedtuple, OrderedDict

# 指定csv文件
csv_input_path = r'd:\ai\tftest\images\train\train.csv'
# 指定输出record文件路径
record_output_path = r'd:\ai\tftest\images\train\train.record'


# TO-DO replace this with label map
def class_text_to_int(row_label):
    if row_label == 'XYFAN':  # 需改动(这里对应你自己在标注数据时的Label名称，是一一对应的，如果存在多个，在这里添加elif处理)
        return 1 # 这里的1是索引，如果存在多个标签，依次为2,3,...
    else:
        None

def create_tf_example(row):
    # full_path = os.path.join(os.getcwd(), 'images', '{}'.format(row['filename']))
    full_path = os.path.join(os.path.dirname(csv_input_path), '{}'.format(row['filename']))
    with tf.gfile.GFile(full_path, 'rb') as fid:
        encoded_jpg = fid.read()
    encoded_jpg_io = io.BytesIO(encoded_jpg)
    image = Image.open(encoded_jpg_io)
    width, height = image.size

    filename = row['filename'].encode('utf8')
    image_format = b'jpg'
    xmins = [row['xmin'] / width]
    xmaxs = [row['xmax'] / width]
    ymins = [row['ymin'] / height]
    ymaxs = [row['ymax'] / height]
    classes_text = [row['class'].encode('utf8')]
    classes = [class_text_to_int(row['class'])]

    tf_example = tf.train.Example(features=tf.train.Features(feature={
        'image/height': dataset_util.int64_feature(height),
        'image/width': dataset_util.int64_feature(width),
        'image/filename': dataset_util.bytes_feature(filename),
        'image/source_id': dataset_util.bytes_feature(filename),
        'image/encoded': dataset_util.bytes_feature(encoded_jpg),
        'image/format': dataset_util.bytes_feature(image_format),
        'image/object/bbox/xmin': dataset_util.float_list_feature(xmins),
        'image/object/bbox/xmax': dataset_util.float_list_feature(xmaxs),
        'image/object/bbox/ymin': dataset_util.float_list_feature(ymins),
        'image/object/bbox/ymax': dataset_util.float_list_feature(ymaxs),
        'image/object/class/text': dataset_util.bytes_list_feature(classes_text),
        'image/object/class/label': dataset_util.int64_list_feature(classes),
    }))
    return tf_example

def main():
    writer = tf.python_io.TFRecordWriter(record_output_path)
    examples = pd.read_csv(csv_input_path)
    for index, row in examples.iterrows():
        tf_example = create_tf_example(row)
        writer.write(tf_example.SerializeToString())
    writer.close()


if __name__ == '__main__':
    main()
 ```
 注：在执行脚本前需要确认 csv_input_path、record_output_path 这2个值的设置正确
 
 #### 3、训练模型参数配置
 在目录下新增 picrecognize.pbtxt文件(文件名随便，后缀必须是*.pbtxt格式)，里面的内容如下：
 ```
 item {
  name: "XYFAN"
  id: 1
  display_name: "XYFAN"
}
item {
  name: "XYFAN2"
  id: 2
  display_name: "XYFAN2"
}

 ```
 说明一下：
 + name就是你在使用小工具软件进行标识的时候填写的Label名称，id为csv文件转record文件的脚本里面class_text_to_int函数返回的整数值，需要一一对应上，display_name为显示名称，不做强制要求，是在实时显示时展示使用
 + 如果标注的数据集里面有多个检测对象，这里的item都需要一一对应起来维护正确

本人使用的训练模型为ssd_mobilenet_v2_coco，所以也需要到官网去下载对应版本的模型文件，本人下载的是ssd_mobilenet_v2_coco_2018_03_29.tar.gz，自行在网络上搜索下载，下载后解压到d:\ai\tftest目录即可

从d:\ai\tftest\models-2.3.0\research\object_detection\samples\configs\目录下找到ssd_mobilenet_v2_coco.config文件，然后复制1份到d:\ai\tftest目录下，进行修改里面指定位置的参数，如下所示：
```
# SSD with Mobilenet v2 configuration for MSCOCO Dataset.
# Users should configure the fine_tune_checkpoint field in the train config as
# well as the label_map_path and input_path fields in the train_input_reader and
# eval_input_reader. Search for "PATH_TO_BE_CONFIGURED" to find the fields that
# should be configured.

model {
  ssd {
    num_classes: 1  # 需要修改(这里对应你的数据在标注时总分类数量)
    box_coder {
      faster_rcnn_box_coder {
        y_scale: 10.0
        x_scale: 10.0
        height_scale: 5.0
        width_scale: 5.0
      }
    }
    matcher {
      argmax_matcher {
        matched_threshold: 0.5
        unmatched_threshold: 0.5
        ignore_thresholds: false
        negatives_lower_than_unmatched: true
        force_match_for_each_row: true
      }
    }
    similarity_calculator {
      iou_similarity {
      }
    }
    anchor_generator {
      ssd_anchor_generator {
        num_layers: 6
        min_scale: 0.2
        max_scale: 0.95
        aspect_ratios: 1.0
        aspect_ratios: 2.0
        aspect_ratios: 0.5
        aspect_ratios: 3.0
        aspect_ratios: 0.3333
      }
    }
    image_resizer {
      fixed_shape_resizer {
        height: 300
        width: 300
      }
    }
    box_predictor {
      convolutional_box_predictor {
        min_depth: 0
        max_depth: 0
        num_layers_before_predictor: 0
        use_dropout: false
        dropout_keep_probability: 0.8
        kernel_size: 1
        box_code_size: 4
        apply_sigmoid_to_scores: false
        conv_hyperparams {
          activation: RELU_6,
          regularizer {
            l2_regularizer {
              weight: 0.00004
            }
          }
          initializer {
            truncated_normal_initializer {
              stddev: 0.03
              mean: 0.0
            }
          }
          batch_norm {
            train: true,
            scale: true,
            center: true,
            decay: 0.9997,
            epsilon: 0.001,
          }
        }
      }
    }
    feature_extractor {
      type: 'ssd_mobilenet_v2'
      min_depth: 16
      depth_multiplier: 1.0
      conv_hyperparams {
        activation: RELU_6,
        regularizer {
          l2_regularizer {
            weight: 0.00004
          }
        }
        initializer {
          truncated_normal_initializer {
            stddev: 0.03
            mean: 0.0
          }
        }
        batch_norm {
          train: true,
          scale: true,
          center: true,
          decay: 0.9997,
          epsilon: 0.001,
        }
      }
    }
    loss {
      classification_loss {
        weighted_sigmoid {
        }
      }
      localization_loss {
        weighted_smooth_l1 {
        }
      }
      hard_example_miner {
        num_hard_examples: 3000
        iou_threshold: 0.99
        loss_type: CLASSIFICATION
        max_negatives_per_positive: 3
        min_negatives_per_image: 3
      }
      classification_weight: 1.0
      localization_weight: 1.0
    }
    normalize_loss_by_num_matches: true
    post_processing {
      batch_non_max_suppression {
        score_threshold: 1e-8
        iou_threshold: 0.6
        max_detections_per_class: 100
        max_total_detections: 100
      }
      score_converter: SIGMOID
    }
  }
}

train_config: {
  batch_size: 10  # 这里可以修改或不用修改（表示训练时每次处理的图片数量）
  optimizer {
    rms_prop_optimizer: {
      learning_rate: {
        exponential_decay_learning_rate {
          initial_learning_rate: 0.004
          decay_steps: 800720
          decay_factor: 0.95
        }
      }
      momentum_optimizer_value: 0.9
      decay: 0.9
      epsilon: 1.0
    }
  }
  fine_tune_checkpoint: "d:/ai/tftest/ssd_mobilenet_v2_coco_2018_03_29/model.ckpt" # 这里需要修改
  fine_tune_checkpoint_type:  "detection"
  # Note: The below line limits the training process to 200K steps, which we
  # empirically found to be sufficient enough to train the pets dataset. This
  # effectively bypasses the learning rate schedule (the learning rate will
  # never decay). Remove the below line to train indefinitely.
  num_steps: 5000  # 这里可以修改或不用修改（表示训练的总阶段数）
  data_augmentation_options {
    random_horizontal_flip {
    }
  }
  data_augmentation_options {
    ssd_random_crop {
    }
  }
}

train_input_reader: {
  tf_record_input_reader {
    input_path: "d:/ai/tftest/train.record" # 这里需要修改
  }
  label_map_path: "d:/ai/tftest/picrecognize.pbtxt"
}

eval_config: {
  num_examples: 40 # 这里需要修改（对应验证样本的总数量）
  # Note: The below line limits the evaluation process to 10 evaluations.
  # Remove the below line to evaluate indefinitely.
  max_evals: 10
}

eval_input_reader: {
  tf_record_input_reader {
    input_path: "d:/ai/tftest/test.record" # 这里需要修改
  }
  label_map_path: "d:/ai/tftest/picrecognize.pbtxt" # 这里需要修改
  shuffle: false
  num_readers: 1
}
```
注：以上内容需要修改的地方已做好注释

在d:\ai\tftest目录下创建training目录：
在cmd中执行：
```
python d:\ai\tftest\models-2.3.0\research\object_detection\model_main.py --model_dir=d:\ai\tftest\training --pipeline_config_path=d:\ai\tftest\ssd_mobilenet_v2_coco.config
```
执行以上命令即开始训练数据，如果没有报错则就等待模型训练完成即可

在等待训练完模型后，在d:\ai\tftest\training 目录下输入tensorboard --logdir ./ 可以启动浏览器可以访问的服务查看模型训练的整体结果
在服务启动之后，在浏览器里输入http://localhost:6006 即可查看展示的训练结果


### 三、输出模型
1、首先在d:\ai\tftest目录新建finish目录
2、再进入到d:\ai\tftest\models-2.3.0\research\object_detection目录之后，在cmd中输入以下内容并执行：
```
python export_inference_graph.py --pipeline_config_path=d:\ai\tftest\ssd_mobilenet_v2_coco.config --trained_checkpoint_prefix=d:\ai\tftest\training\model.ckpt-xxx --output_directory=d:\ai\tftest\finish
```
说明一下：上面脚本里面有个xxx，这里是指代模型训练完成后要导出哪个最优索引生成模型文件，可以d:\ai\tftest\training目录下查找checkpoint文件使用记事本打开，查看第一行进行核实确认

执行完上面的脚本后finish目录下就是输出的模型结果相关文件，模型文件以*.pb后缀结尾


### 四、使用模型目标检测（采用OpenCV Android方式检测模型）
1、首先要下载opencv android包，这部分操作自行查找资料，opencv官网：https://opencv.org 
2、在androidstudio中创建新工程，并引入opencv依赖库
3、生成tensorflow模型在opencv中加载时需要的*.pbtxt文件
 + 在cmd中进入到opencv安装的目录，然后进入到opencv/samples/dnn目录
 + 在当前目录执行以下脚本：
 ```
 python tf_text_graph_ssd.py --input d:\ai\tftest\finish\frozen_inference_graph.pb --output d:\ai\tftest\finish\output.pbtxt --config d:\ai\tftest\finish\pipeline.config
 ```
 说明一下：input参数是指tensorflow训练完成输出的新模型文件；output参数是指*.pbtxt生成的文件全路径名称；config参数是指tensorflow训练完成输出的配置文件；
 执行完上面的脚本后会在d:\ai\tftest\finish 目录生成一个output.pbtxt
 
 4、加载tf模型
 ```android
 Net tensorflow = Dnn.readNetFromTensorflow("这里填写*.pd文件的全路径", "这里填写*.pbtxt文件的全路径");
 ```
 说明一下：在android当中模型文件要放在sdcard目录下进行加载才能成功

5、使用模型
```android
Mat frame = new Mat();
Imgproc.cvtColor(src, frame, Imgproc.COLOR_BGRA2BGR);
Mat blob = Dnn.blobFromImage(frame, 1.0 / 127.5, new Size(300, 300), new Scalar(127.5, 127.5), false, false);
List<Mat> outputBlobs= new ArrayList<>();
tensorflow.setInput(blob);
tensorflow.forward(outputBlobs, net.getUnconnectedOutLayersNames());
...（其它代码略）
```

至此实现了从tensorflow训练自己的模型，并采用android加载模型使用新模型的全流程操作


