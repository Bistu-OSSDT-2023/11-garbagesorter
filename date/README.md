# Refuse Classification

## 1.项目说明

Refuse Classification是用于垃圾分类的一款程序。垃圾分类是现在社会会上的热门话题之一，为了使人们更快速更简单地实现垃圾分类，减少实施中错误出现的频率，我们设计开发了这一款程序。可以先通过程序爬虫获得想要的照片并进行分类，再运用resent50模型对我们投入的大量垃圾照片进行训练，最后便可以实现给他传入照片，可以快速地分辨出是什么类型的垃圾的功能。使我们更好地实现垃圾分类并且减少分类的出错率，从而减少重复的工作量。

## 2.安装运行要求

### (1)运行前提条件

下载好我们提供的模型，安装好pycharm和用anacanda配置好虚拟环境，并配置好requirements.txt中所需要的包

### (2)项目运行步骤

1. 将你电脑中resnet50d_2epochs_accuracy0.99115_weights.pth这个文件的路径复制到如下地方

![](https://markdown.liuchengtu.com/work/uploads/upload_0803edfaa9326c1d0db86a44bd09f1ae.png)



 2.单击右键选择“run window”，便可以运行了

 ![](https://markdown.liuchengtu.com/work/uploads/upload_e285d89b6c311312c22be5ee1ae94be8.png)

![](https://markdown.liuchengtu.com/work/uploads/upload_5a5037f5e5e3cf19c169305c4e17931a.png)


## 3.开发要求

### 1.环境配置

#### （1）Anaconda 和 Pycahrm安装

配置虚拟环境需要通过anaconda来完成，anaconda的下载地址为：https://docs.conda.io/en/latest/miniconda.html

![安装示例](https://img-blog.csdnimg.cn/img_convert/491b9e4ef50e79197270eecfb2a40d34.png)

Pycharm的下载地址：[Other Versions - PyCharm (jetbrains.com)](https://www.jetbrains.com/pycharm/download/other.html)

![image-20221206173934245](https://vehicle4cm.oss-cn-beijing.aliyuncs.com/imgs/image-20221206173934245.png)

#### (2)代码环境配置

程序安装完毕之后打开windows的命令行（cmd），输入conda env list，出现下列信息则表示conda已完成安装

![示例](https://img-blog.csdnimg.cn/img_convert/949b1ddaa51e12f81bfb603542a27d93.png)

在命令行中输入"conda create -n cls-42 python==3.8.5"创建虚拟环境,安装结束之后输入"conda activate cls-42
"激活虚拟环境，出现小括号表示环境激活成功.

代码环境配置步骤较多，下面只列出关键命令，方便大家复制粘贴。

```bash
conda create -n cls-42 python==3.8.5
conda activate cls-42
conda install pytorch==1.10.0 torchvision==0.11.0 torchaudio==0.10.0 cudatoolkit=11.3
cd 自己本地的代码目录 （或者在本地代码目录的上方打开cmd）
pip install -r requirements.txt
```

### 2.数据集

#### （1）数据集的搜集

数据集我们有两种方式获取，一种可以通过我们文件中的代码爬虫爬取建立自建的数据集，另外一种是使用公开的数据集，在我们这次项目中使用的是阿里天池中的数据集

阿里天池官网：https://tianchi.aliyun.com/

我们的文档里也提供了一个爬虫的程序，可以帮助大家从百度图片中爬取自己需要的图片，程序的名称是 `data_get.py`，使用起来非常方便，大家直接运行程序之后，输入自己想要爬取的图片即可


输入完成之后，爬取之后的图片将会自动保存在data目录下。


#### （2）数据集清洗

我们这里首先需要对数据集进行清洗，清洗的目的有两个：一是去除爬取图片中的坏图，二是将原先中文名称的图片修改为英文。

该程序是 `data_clean.py`文件，使用的时候需要传入两个参数，一个是原始爬取的数据目录，一个是该类别的英文名称。

![](https://markdown.liuchengtu.com/work/uploads/upload_e58d6fb468440785d6f820d12050e477.png)


处理之后的好图将会保存在后缀为_cleaned目录的文件下

#### （3）数据集划分

在正式训练之前，你还需要准备好训练集、验证集和测试集，一般是需要将所有的数据按照6:2:2的比例进行划分。这里也为大家准备了 `data_split.py`的脚本帮助大家做数据集的划分。划分之前，请先按照类别将对应的图片放在对应的子目录下，如下图所示：

![](https://markdown.liuchengtu.com/work/uploads/upload_3479d1d49d6090b6596273f17e02e11d.png)


然后使用 `data_split.py`脚本即可，这里你只需要修改原始数据集的路径，直接运行就可以，之后将会产生一个后缀为split的目录，里面是按照6：2：2比例划分之后的数据集。

### 3.模型训练

文档中已经存在resnet50的模型，右键直接运行 `train.py`就可以开始训练模型，代码首先会输出模型的基本信息（模型有几个卷积层、池化层、全连接层构成）和运行的记录，如下图所示：

![image-20221128092952811](https://cmfighting.oss-cn-shenzhen.aliyuncs.com/iiimgs/image-20221128092952811.png)

运行结束之后，训练好的模型将会保存在一开始你指定的目录下

### 4.模型测试

`test.py`是模型测试的主程序，在进行模型测试之前，你需要对标注了todo文字所在行的内容进行修改，并且你的测试一定是在训练之后进行的。

修改完毕之后，右键执行 `test.py`就可以对你指定的模型在测试集上进行测试，测试结束，将会输出模型在数据集上的F1、Recall和ACC等指标，并且会生成由每类分别的准确率构成的热力图保存在record目录下

![](https://markdown.liuchengtu.com/work/uploads/upload_cdfb9e6537ff8a54de4ecdfcfb68d43f.png)

### 5.模型预测

模型预测的代码是 `predict.py`文件，该文件中包含了两个函数，分别是文件夹预测和单张图片预测的函数，在实际运行的时候，你需要在main函数中指定具体要执行的是哪个函数。执行之前，需要设置一些基本参数，需要设置的参数用todo进行了标记，按照标记进行修改就可以。

### 6.图形化界面构建

图形化界面的构建通过pyqt5来编写，主要功能还是上传图片和对模型进行推理。

启动界面之前需要设置几个关键的参数，
![image-20221201113948361](https://cmfighting.oss-cn-shenzhen.aliyuncs.com/iiimgs/image-20221201113948361.png)

之后，直接启动window.py即可运行。


## 4.代码结构说明
images——//目录主要是放置一些图片，包括测试的图片和ui界面使用的图片
test imgs——//便于输出测试结果
utils——//测试的一些文件
date_clean.py——//实际图片保存和读取的过程中存在英文，所以这里通过这两种方式来应对中文读取的情况
date_get.py——//爬取图片
date_split.py——//将数据集划分为训练集、验证集、测试集
onnxruntime_demo.py——//预测图片
predict.py——//用来推理数据集
torchutils.py——//这个库主要用于定义如何进行数据增强
test.py——//测试文件，主要用于测试模型在验证集上的准确率
train.py——//训练模型的代码
window.py——//界面设计代码

### 5.项目联系人
刘彦希 周钰红 徐佳琪 薛艺

