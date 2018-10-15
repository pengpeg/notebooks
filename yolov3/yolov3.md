# YOLO:Real-Time Object Detection
YOLO(You Only Lool Once) 是一个高性能实时目标检测系统。在ＣＯＣＯ的测试集上得到５７．９ｍAP的结果，同时使用Titan X时检测速度达到３０FPS。
## 与其他检测算法对比
YOLOv3非常快而且精确。IOU阈值为0.5时的mAP标准，YOLOv3和Focal Loss精度差不多，但是前者比后者的检测速度快了４倍。不需重新训练，只需选择不同大小的模型就可以在精度和速度间调整。
## 在COCO数据集上的测试结果
|    Model    |    Train    |    Test    |    mAP    |    FLOPS    |    FPS    |
|:-------:|:------------- | ----------:|:----:| ---:| ---:|
|    SSD300    |    COCO trainval    |    test-dev    |    41.2    |    -    |    46    |
|    SSD500    |    COCO trainval    |    test-dev    |    46.5    |    -    |    19    |
|    YOLOv2 608x608     |    COCO trainval    |    test-dev    |    48.1    |    62.94Bn    |    40    |
|    Tiny YOLO    |    COCO trainval    |    test-dev    |    23.7    |    5.41Bn    |    244    |
|    SSD321    |    COCO trainval    |    test-dev    |    45.4    |    -    |    16    |
|    DSSD321    |    COCO trainval    |    test-dev    |    46.1    |    -    |    12    |
|    R-FCN    |    COCO trainval    |    test-dev    |    51.9    |    -    |    12    |
|    SSD513    |    COCO trainval    |    test-dev    |    50.4    |    -    |    8    |
|    DSSD513    |    COCO trainval    |    test-dev    |    53.3    |    -    |    6    |
|    FPN FRCN    |    COCO trainval    |    test-dev    |    59.1    |    -    |    6    |
|    Retinanet-50-500    |    COCO trainval    |    test-dev    |    50.9    |    -    |    14    |
|    Retinanet-101-500    |    COCO trainval    |    test-dev    |    53.1    |    -    |    11    |
|    Retinanet-101-800    |    COCO trainval    |    test-dev    |    57.5    |    -    |    5    |
|    YOLOv3-320     |    COCO trainval    |    test-dev    |    51.5    |    38.97Bn    |    45    |
|    YOLOv3-416     |    COCO trainval    |    test-dev    |    55.3    |    65.86Bn    |    35    |
|    YOLOv3-608     |    COCO trainval    |    test-dev    |    57.9    |    140.69Bn    |    20    |
|    YOLOv3-tiny     |    COCO trainval    |    test-dev    |    33.1    |    5.56Bn    |    220    |
|    YOLOv3-spp     |    COCO trainval    |    test-dev    |    60.6    |    141.45Bn    |    20    |

## 怎样工作
早期检测系统使用分类器或定位器实施检测。通过在图片多尺度多位置应用模型。高得分区域被认为检测。
我们使用一个完全不同的方法。在整张图片上使用一个神经网络。网络将图片划分为多个区域，为每个区域预测边界框和概率。这些边界框由预测的概率进行加权。
我们的模型比基于分类的系统有几个优势。在测试时使用整个图片的信息使得它的预测由全局上下文推断而出。

## 版本３中新改变了什么
YOLOv3使用一些小手段来提升训练和性能，包括多尺度预测，更好的主干分类网等等。详细查看[论文](https://pjreddie.com/media/files/papers/YOLOv3.pdf) 。

## 使用预训练模型检测
此节引导你如何使用训练好的模型进行目标检测。如果你还未安装Darknet，你应该先做[第一步](https://pjreddie.com/darknet/install/) ，或者不去阅读全文只需执行下面命令：
```
git clone https://github.com/pjreddie/darknet
cd darknet
make
```
在子目录cfg/中已经有YOLO的配置文件。你需要下载训练好的[模型](https://pjreddie.com/media/files/yolov3.weights) 。或者只需执行下面命令：
```
wget https://pjreddie.com/media/files/yolov3.weights
```
然后运行检测器！
```
./darknet detect cfg/yolov3.cfg yolov3.weights data/dog.jpg
```
然后你会看到运行信息和最终检测结果。
Darknet会输出它检测出的对象、置信度、耗时。我们没有联合Opencv编译Darknet，因此不能直接展示检测结果。相反我们将其保存为predictions.png。你可以打开图片看到检测出的对象。因为我们是在CPU上检测，因此每张图片花费了6-12秒。如果使用GPU版本，它会跑的非常快。
在子目录data/中有几张测试图片用于测试。
`detect`命令是一个通用命令的简化版本，其等价于下面的命令：
```
./darknet detector test cfg/coco.data cfg/yolov3.cfg yolov3.weights data/dog.jpg
```
如果你只是在一张图片上运行检测，你不需要知道此命令。但是当你想要做其他事例如运行在网络相机上，此命令可能是有用的。

##　多张图片
将`detect`命令中的图片路径设为空可以检测多张图片。此时当加载完配置文件和模型参数你会看到提示符。
```
./darknet detect cfg/yolov3.cfg yolov3.weights
```
在命令符后输入图片的路径来检测图片。
使用`Ctrl-C`退出检测程序。

## 调整检测阈值
默认情况下，YOLO只展示置信度0.25以上的结果。可以在命令中加入`-thresh <val>`来设置这个阈值。例如：
```
./darknet detect cfg/yolov3.cfg yolov3.weights data/dog.jpg -thresh 0
```

## Tiny YOLOv3
我们有一个为受限环境准备的小模型`yolov3-tiny`。为使用这个模型，我们先下载模型参数：
```
wget https://pjreddie.com/media/files/yolov3-tiny.weights
```
然后使用模型的配置文件和权重
```
./darknet detect cfg/yolov3-tiny.cfg yolov3-tiny.weights data/dog.jpg
```
## 网络相机上的实时检测
如果不能看到检测结果在测试数据上运行YOLO不是很有趣。我们让它运行网络摄像机上而不是一堆图片上。
为了能运行这个Demo，你需要编译[带有CUDA和OpenCV的Darknet](https://pjreddie.com/darknet/install/#cuda) 。然后运行命令：
```
./darknet detector demo cfg/coco.data cfg/yolov3.cfg yolov3.weights
```
YOLO会显示当前FPS并将预测的类别和边界框画在图片上。
你需要一个网络相机连接到电脑使OpenCV能连接上，否则无法工作。如果你连接了多个网络相机，可以在命令中加入参数`-c <num>`选取（默认情况下OpenCV使用网录像机０）。
如果OPenCV能够读取一个视频文件，你也可以在此视频文件上运行检测：
```
./darknet detector demo cfg/coco.data cfg/yolov3.cfg yolov3.weights <video file>
```
上面就是我们在YouTube上的视频的制作方法。
## 在VOC数据集上训练YOLO
你可以使用不同的超参数、数据来训练YOLO。此处给出使用Pascal VOC数据集训练YOLO的示例。
## 获取Pascal VOC数据
为了训练YOLO，你需要从2007到2012的VOC数据。你可以找到[数据连接](https://pjreddie.com/projects/pascal-voc-dataset-mirror/) 。为了获取数据，创建一个目录用于存放数据，然后在创建的目录下执行命令：
```
wget https://pjreddie.com/media/files/VOCtrainval_11-May-2012.tar
wget https://pjreddie.com/media/files/VOCtrainval_06-Nov-2007.tar
wget https://pjreddie.com/media/files/VOCtest_06-Nov-2007.tar
tar xf VOCtrainval_11-May-2012.tar
tar xf VOCtrainval_06-Nov-2007.tar
tar xf VOCtest_06-Nov-2007.tar
```
将会有一个`VOCdevkit/`的子目录，所有的VOC数据都在里面。
## 为VOC数据生成标签
现在我们需要生成Darknet使用的标签文件。Darknet需要每一张图片有一个`.txt`文件，其中每行代表一个对象标签，格式为：
```
<object-class> <x> <y> <width> <height>
```
其中`x,y,width,height`是相对于图片的宽和高的。为了生成这些文件，我们需要运行Darknet子目录`scripts/`中的`voc_label.py`。先下载这个文件。
```
wget https://pjreddie.com/media/files/voc_label.py
python voc_label.py
```
几分钟后，这个脚本会生成所有需要的标签文件。其产生的文件在`VOCdevkit/VOC2007/labels/`和`VOCdevkit/VOC2012/labels/`中。当前你目录中的文件为：
```
ls
2007_test.txt   VOCdevkit
2007_train.txt  voc_label.py
2007_val.txt    VOCtest_06-Nov-2007.tar
2012_train.txt  VOCtrainval_06-Nov-2007.tar
2012_val.txt    VOCtrainval_11-May-2012.tar
```
`2007_train.txt`列出某年某图片集的图片名。Darknet需要一个列出所有训练图片的文本文件。这个例子中我们使用除去2007测试集外的所有图片来训练。执行：
```
cat 2007_train.txt 2007_val.txt 2012_*.txt > train.txt
```
现在我们让所有`2007 trainval,2012 trainval`在一个大列表中了。这就是所有的数据设置。
## 为Pascal数据修改Cfg
现在去Darknet的目录。修改`cfg/voc.data`配置文件指向你自己的数据：
```
classes= 20
train  = <path-to-voc>/train.txt
valid  = <path-to-voc>2007_test.txt
names = data/voc.names
backup = backup
```
你需要使用你自己存放VOC数据的路径来替换`path-to-voc`。
## 下载预训练卷积权重
为了训练，我们使用在Imagenet预训练的卷积权重。我们使用的权重来着模型[darknet53](https://pjreddie.com/darknet/imagenet/#darknet53) 。你也可以只下载[卷基层权重(76MB)](https://pjreddie.com/media/files/darknet53.conv.74) 。
```
wget https://pjreddie.com/media/files/darknet53.conv.74
```
## 训练模型
现在我们可以训练模型了。执行命令：
```
./darknet detector train cfg/voc.data cfg/yolov3-voc.cfg darknet53.conv.74
```
## 在COCO数据集上训练YOLO
下面展示如何使用[COCO](http://cocodataset.org/#overview) 数据训练模型。
## 获取COCO数据
为了训练YOLO，你需要所有COCO的数据和标签。脚本`scripts/get_coco_dataset.sh`将为你做这件事。将这个脚本放到你想要存放COCO数据的地方，运行脚本下载数据：
```
cp scripts/get_coco_dataset.sh data
cd data
bash get_coco_dataset.sh
```
`Now you should have all the data and the labels generated for Darknet.`
## 为COCO修改cfg
先转到arknet的目录。修改'cfg/coco.data'文件指向你的数据：
```
classes= 80
train  = <path-to-coco>/trainvalno5k.txt
valid  = <path-to-coco>/5k.txt
names = data/coco.names
backup = backup
```
使用你的COCO数据路径替换`<path-to-coco>`。
修改模型配置文件，训练代替测试。`cfg/yolo.cfg`应该像下面一样：
```
[net]
# Testing
# batch=1
# subdivisions=1
# Training
batch=64
subdivisions=8
....
```
## 训练模型
现在可以训练了！执行命令：
```
./darknet detector train cfg/coco.data cfg/yolov3.cfg darknet53.conv.74
```
使用多个GPU训练：
```
./darknet detector train cfg/coco.data cfg/yolov3.cfg darknet53.conv.74 -gpus 0,1,2,3
```
如果想要从某个	`checkpoint`重新开始训练：
```
./darknet detector train cfg/coco.data cfg/yolov3.cfg backup/yolov3.backup -gpus 0,1,2,3
```
## Open Images dataset上的YOLOv3
```
wget https://pjreddie.com/media/files/yolov3-openimages.weights

./darknet detector test cfg/openimages.data cfg/yolov3-openimages.cfg yolov3-openimages.weights
```
## 旧版的YOLO在哪里？
如果你想要使用YOLOv2，你仍可以找到它：[https://pjreddie.com/darknet/yolov2/](https://pjreddie.com/darknet/yolov2/) 。
## Cite
如果在你的工作中使用了YOLOv3，请引用我们的论文！
```
@article{yolov3,
  title={YOLOv3: An Incremental Improvement},
  author={Redmon, Joseph and Farhadi, Ali},
  journal = {arXiv},
  year={2018}
}
```

