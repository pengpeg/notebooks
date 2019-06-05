# Mxnet公布的训练好的网络进行预测时需要进行的数据初始化操作

## gluoncv中图像分类网络
以官方代码为例：
1.读取图片
```
img = mx.image.imread(filename)
```
默认情况：返回一个（H x W x C）的通道顺序为RGB的NDArray对象。
2.数据初始化
```
transformed_img = gluoncv.data.transforms.presets.imagenet.transform_eval(img)
```
第一步：保持图片比例不变，resize图片短边为256；
第二步：中心裁剪出224x224大小的图片；
第三步：将HxWxC且值范围[0-255]的数组转为CxHxW且值范围[0-1)的float32数组，即所有值除以255；
第四步：normalize，即给出3个通道的mean和std（m1,m2,m3,s1,s2,s3）（默认值mean=(0.485, 0.456, 0.406), std=(0.229, 0.224, 0.225)），`output[i] = (input[i] - mi) / si`
第五步：扩充维度，成为1xCxHxW