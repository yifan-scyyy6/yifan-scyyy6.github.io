---
title: 本地复现 TCResNet8 模型的训练算法
date: 2023-10-05 01:00:00
---

### 本地复现 TCResNet8 模型的训练算法

0. 引用

    文章标题：Temporal Convolution for Real-time Keyword Spotting on Mobile Devices

    项目地址：https://github.com/hyperconnect/TC-ResNet

1. 环境搭建

   项目中的Markdown文件已经写的较为清楚。这里再补充一点，因为原项目是在Linux系统下运行的，使用Windows系统的用户，建议先安装 Windows Subsystem for Linux (WSL)，再在Linux下运行本项目。

   否则可能出现如下警告：

   > WSL is not installed, so could not detect WSL profiles.

   WSL安装教程（Microsoft官方）：https://learn.microsoft.com/en-us/windows/wsl/install

2. 调试运行

   调试步骤在Markdown文件内已经写的较为清楚。这里补充一些细节并修正一些错误。

   2.1 To train TCResNet8Model-1.0 model, run:

   > ./scripts/commands/TCResNet8Model-1.0_mfcc_40_3010_0.001_mom_l1.sh

   在执行此指令之前，需要将所用的dataset下载到 `TC-ResNet` 文件夹下。并执行split此dataset的.sh文件。

   > ./speech_commands_dataset/download_and_split.sh

   有可能因为权限不够出现报错：

   > Permission denied

   可以输入如下指令获取权限后再次执行：

   > sudo chmod -R 777 *
   >
   > -R 所执行目录下的所有子目录与文件
   >
   > 777 给予用户最高权限
   >
   > \* 指定执行目录，\*为执行当前路径

   若成功执行本节（2.1）开头的指令，即开始训练模型。

   关于训练模式，在论文3.1节中的Training部分有详细介绍。

   简述：共30k循环，每0.5k次记录一次performance。

   可能出现如下报错：

   > ```python
   > Traceback (most recent call last):
   >   File "evaluate_audio.py", line 87, in <module>
   >   	main(args)
   >   File "evaluate_audio.py", line 28, in main
   >   	is_training,
   >   File "/home/yifan/TC-ResNet/datasets/audio_data_wrapper.py", line 15, in \__init__
   >   	self.setup()
   >   File "/home/yifan/TC-ResNet/datasets/audio_data_wrapper.py", line 126, in setup
   >   	assert self.args.num_classes == self.num_labels
   > AssertionError
   > ```

   此处提示dataset的label数量与模型输出的分类数不一致，由dataset路径错误导致。未能使程序正确找到dataset并获取其中的label信息。

   打开`TCResNet8Model-1.0_mfcc_40_3010_0.001_mom_l1.sh`

   ![1-1](/img/1-1.PNG)

   将 *dataset_path* 修改为split后的dataset所在路径。建议使用绝对路径。

   2.2 To freeze the trained model checkpoint into `.pb` file, run:

   > python freeze.py --checkpoint_path work/v1/TCResNet8Model-1.0/mfcc_40_3010_0.001_mom_l1/TCResNet8Model-XXX --output_name output/softmax --output_type softmax --preprocess_method no_preprocessing --height 49 --width 40 --channels 1 --num_classes 12 TCResNet8Model --width_multiplier 1.0

   此指令有误。直接运行会报错，提示缺少preprocess_method，height，width 和 channels 参数（事实上已传入）。

   修改如下：

   > python freeze.py --num_classes 12 --checkpoint_path work/v1/TCResNet8Model-1.0/mfcc_40_3010_0.001_mom_l1/TCResNet8Model-XXX --output_name softmax  --preprocess_method no_preprocessing --height 49 --width 40 --channels 1 TCResNet8Model --width_multiplier 1.0 
   >
   >
   > XXX 输入产生该份数据的迭代次数（完整运行则为30000）

   2.3 To convert the `.pb` file into `.tflite` file, run:

   > tflite_convert --graph_def_file=work/v1/TCResNet8Model-1.0/mfcc_40_3010_0.001_mom_l1/TCResNet8Model-XXX.pb --input_format=TENSORFLOW_GRAPHDEF --output_format=TFLITE --output_file=work/v1/TCResNet8Model-1.0/mfcc_40_3010_0.001_mom_l1/TCResNet8Model-XXX.tflite --inference_type=FLOAT --inference_input_type=FLOAT --input_arrays=input --output_arrays=output/softmax --allow_custom_ops

   同样改句指令也有误，修改为：

   > tflite_convert --graph_def_file=work/v1/TCResNet8Model-1.0/mfcc_40_3010_0.001_mom_l1/TCResNet8Model-XXX.pb --input_format=TENSORFLOW_GRAPHDEF --output_format=TFLITE --output_file=work/v1/TCResNet8Model-1.0/mfcc_40_3010_0.001_mom_l1/TCResNet8Model-XXX.tflite --inference_type=FLOAT --inference_input_type=FLOAT --input_arrays=input --output_arrays=softmax --allow_custom_ops
   > 
   >
   >
   > XXX 输入产生该份数据的迭代次数（完整运行则为30000）

   至此，得到`.pb` 与 `.tflite`两份文件，TCResNet8 模型训练部分算法复现结束。

3. 参考资料：

   [Generate pb file command is error · Issue #12 · hyperconnect/TC-ResNet (github.com)](https://github.com/hyperconnect/TC-ResNet/issues/12)

   



