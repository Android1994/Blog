---
title:  "ubuntu下利用cue文件对flac等音频文件进行分轨"
author: Subaru Lai
date:   2018-03-19 21:18:24 +0800
layout: post
---
在Ubuntu系统下可以通过一些工具来对flac等格式的音频文件进行分轨，过程如下：

## **一、安装所需的工具**
`$ sudo apt-get install flac shntool libav-tools`

需要对ape文件进行分轨的话，需要编译安装linux版的mac编解码器。

## **二、根据cue索引对flac文件进行分轨**
`$ shntool split -t "%n.%p-%t" -f example.cue -o flac music.flac -d outputdir`

- -d：指定输出目录，默认为当前目录
- -t：指定输出文件的文件名格式，%n是音轨号，%p是艺术家，%t是标题

## **三、将ape格式的文件转换为其他格式**
对ape文件进行分轨似乎会出现一些比较麻烦的错误，可以先将ape格式的文件先转换成其他格式的音频文件，再进行分轨：
```
$ avconv -i CDImage.ape CDImage.flac //ape转flac, 以前用ffmpeg，现在用avconv
$ avconv -i CDImage.wav CDImage.flac //wav转flac
$ avconv -i CDImage.ape CDImage.wav //ape转wav 
```

## **四、其他格式之间的转换**
```
$ flac CDImage.wav CDImage.flac //wav转flac
$ shnconv -i ape -o flac CDImage.ape //ape转flac
$ shnconv -i flac -o ape CDImage.flac //flac转ape
```
