# 转换darknet的yolov2模型到intel神经计算棒

## 由darknet模型转换到tensorflow模型
1.安装tensorflow（安装cpu版，够用就行，intel开发库要求tf版本大于1.2.0，更新的库可能需要更高版本）;
2.照intel文档说明安装darkflow；
3.安装darkflow需要安装cyphon;
4.使用`python3 setup.py build_ext --inplace`安装到darkflow目录，方便转模型时找到路径;
5.指令`flow --model cfg/yolo.cfg --load bin/yolo.weights --savepb --imgdir sample_img/`转换模型、测试模型、保存结果模型;
6.之上以git上darkflow工程的说明为准;

## 由TF模型转到intel神经计算棒使用的模型
1.根据yolov2.cfg修改yolo_v1_v2.json
2.转换指令
```
python3 ./mo_tf.py
--input_model yolov2.pb          \
--batch 1                                       \
--tensorflow_use_custom_operations_config ./extensions/front/tf/yolo_v1_v2.json
--scale 255 \
--data_type FP16
```
3.yolov2的输入为rgb通道，原始值除以255，保持长宽比放到所有值为0.5的输入中心

## 转换好的模型的使用示例
1.修改intel的yolov3的使用示例代码
2.从模型的xml文件得到最后的region层输出为[1, 5*(80+4+1)*fea_w*fea_h]，所以输出layout为NC，而不是NCHW
3.输出值与darknet代码中网络输出是一样的，然后就是参照darknet对输出进行的处理方法进行处理即可
4.因此修改原始示例代码中的处理，包括：x,y,z,w的计算，根据图像和网络输入尺寸进行矫正、nms修改等
5.详情放在代码里面说明

## 程序报错
１.自定义层找不到
详看[解决办法](https://software.intel.com/zh-cn/forums/computer-vision/topic/805210) 
即编译所有示例代码，会将inference_engine中的自定义层编译得到`/home/chen/inference_engine_samples/intel64/Release/lib/libcpu_extension.so`。然后
```
string path = "/home/chen/inference_engine_samples/intel64/Release/lib/libcpu_extension.so";
IExtensionPtr extension_ptr = make_so_pointer<IExtension>(path.c_str());
plugin.AddExtension(extension_ptr);
```