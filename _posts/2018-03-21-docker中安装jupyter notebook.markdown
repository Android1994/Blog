---
title:  "docker中安装jupyter notebook"
author: Subaru Lai
date:   2018-03-21 19:04:10 +0800
layout: post
---

这是在容器中安装YOLO时遇到的另一个坑，虽然更YOLO没什么关系，而且最后也没用上，还是记一下吧。

### 0、安装ssh
```
$ sudo apt-get install openssh-server
```
然后编辑它的配置文件 /etc/ssh/sshd_config，注释掉配置文件中的"PermitRootLogin without-password"，再增加一句"PermitRootLogin yes"使root用户可以远程登录。

### 1、安装jupyter
```
$pip install jupyter
```

### 2、#生成jupyter配置文件
```
$ jupyter notebook --generate-config
```
这个命令会生成配置文件，在容器里的目录是/root/.jupyter/jupyter_notebook_config.py

### 3、修改密码
进入python命令行：
```
$ python
>> from notebook.auth import passwd
>> passwd()
```
输入密码后，会创建一个密文密码，比如：'sha1:ce23d945972f:34769685a7ccd3d08c84a18c63968a41f1140274'，把这个密文复制下来


### 4、修改默认配置文件
```
$ vim /root/.jupyter/jupyter_notebook_config.py
```

修改以下内容：
```
c.NotebookApp.ip='*'                          #绑定所有地址
c.NotebookApp.password = u'刚才生成的密文'
c.NotebookApp.open_browser = False            #启动后是否在浏览器中自动打开
c.NotebookApp.port =8888                      #指定一个访问端口，默认8888，注意和映射的docker端口对应
```

### 5、配置完成后，就可以启动notebook并从远程访问了
```
jupyter notebook --allow-root
```