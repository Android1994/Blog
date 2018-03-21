---
title:  "ubuntu下安装OpenCV"
author: Subaru Lai
date:   2018-03-21 18:32:14 +0800
layout: post
---

最近在做的一个项目用到了YOLO进行目标识别，尝试在docker容器里装了两个YOLO的实现，一个是原作者给的基于[darknet](https://pjreddie.com/darknet/yolo/)的实现，还有一个是基于tensorflow的实现--[darkflow](https://github.com/thtrieu/darkflow)。

使用的容器是基于tensorflow/tensorflow:1.4.0-gpu-py3这个镜像，安装YOLO的过程中，碰到的主要问题是OpenCV的编译安装，下面主要记录一下OpenCV的安装过程。

### 一、安装相关依赖
首先是安装OpenCV官网给出的依赖：
```
[compiler] sudo apt-get install build-essential
[required] sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
[optional] sudo apt-get install (python-dev python-numpy) libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev # ()里那两个python可以不用装
```

额外的依赖：
```
$ sudo apt-get install pkg-config
```

### 二、下载OpenCV源码
下载：
```
$ wget https://github.com/opencv/opencv/archive/3.2.0.zip # 从github上直接下载或者clone也可
$ wget https://github.com/opencv/opencv_contrib/archive/3.2.0.zip
```

解压：
```
$ unzip opencv-3.2.0.zip
$ unzip opencv_contrib-3.2.0.zip
```

将opencv_contrib复制到opencv-3.2.0目录下：
```
$cp -r opencv_contrib-3.2.0 opencv-3.2.0
```

### 三、编译
在opencv-3.2.0目录下新建build文件夹，并使用cmake命令进行编译：
```
$ cd opencv-3.4.1
$ mkdir build
$ cd build

$ cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/usr/local -D OPENCV_EXTRA_MODULES_PATH=/notebooks/opencvInstall/opencv-3.2.0/opencv_contrib-3.2.0/modules/ ..  #此处路径需要注意
```

cmake成功后，使用make开始安装:
```
$ make -j8

$ make install
```

### （四、设置链接库共享）
先在文件/etc/ld.so.conf中添加内容： /usr/local/lib（这个跟安装目录有关， {CMAKE_INSTALL_PREFIX}/lib），也可以在/etc/ld.so.conf.d 目录下增加一个conf文件（可以命名为 opencv.conf），同样添加内容： /usr/local/lib

然后执行:
```
$ sudo ldconfig -v
```

### （五、指定opencv头文件位置）
这里使用pkg-config命令来完成。首先在 /etc/profile 中添加内容：
```
export  PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/local/lib/pkgconfig
```

通过pkg-config 命令可以列出关于opencv的配置信息：
```
$ pkg-config --libs opencv
```

>note: 四和五的作用有待确认，我安装的时候没做第四步也没啥问题
