---
layout:     post
title:      "学校ssh服务器使用"
subtitle:   "向Project低头"
date:       2020-04-20 18:30:00
author:     "AH"
header-img: "img/post-bg-2015.jpg"
tags:
    - 杂谈
---

大作业队友传来坏消息，周五要交的report周一实验还没跑完，而她的小本本已经为此死机两回了，吓得我赶紧求实验室学长开了个服务器账号。

拿到账号，配环境是最麻烦的事情，在这篇记录一下整个过程。

## 服务器登录

1. 命令行方式

```
>> ssh -p [port] account@ip
```

​	之后会要求输入密码，然后显示连接成功

2. Pycharm连接远程服务器

   命令行界面对我等用惯了Windows系统的人不太友好，幸好Pycharm专业版有显示远程主机文件夹的功能

   我参考了[这篇博客](https://blog.csdn.net/qq_40986693/article/details/103372301)中配置链接远程服务器和远程python解释器的部分

既舍弃不了命令行的简洁，又舍弃不了图形界面的直观，成年人当然两个都要！（所以本菜鸡就两个一起用了，各取其优点）

## 文件上传下载

百度告诉我应该用scp，可是我就是传不上去，气死了。再次感谢Pycharm的远程文件夹管理，上传文件so easy（真的不是广告）

具体参考[这篇](https://www.jetbrains.com/help/pycharm/uploading-and-downloading-files.html)

## Conda

刚连上主机、又传了数据集的我发现服务器内置Python（甚至2、3都有），喜不自胜，以为这就可以开始做作业了，结果一跑代码才发现没有sklearn——没事，pip呗。然后我就发现自己没有sudo权限无法使用pip！还是太天真！

腆着老脸去问学长怎么办，学长回复说自己装conda。隐隐约约想起来上次另一个学长讲过miniconda比较简洁，大概会好装一点吧！于是一顿搜索安装好了miniconda（详见[这里](https://www.osetc.com/en/how-to-install-miniconda-on-ubuntu-18-04-16-04-linux.html)）

安装一切顺利，我又开始沾沾自喜了，`conda install xx`一顿猛装，装了好几个包（这里顺便吐槽一下sklearn在安装的时候并不叫这名，而是叫scikit-learn），这时候我才想起来，万一conda里Python版本是2咋办！看了一下果然还真是！看[这里](https://docs.conda.io/projects/conda/en/latest/user-guide/getting-started.html)学会conda中Python版本的切换。 

总结一下conda的常见命令：

| 命令                                  | 作用                                                         |
| ------------------------------------- | ------------------------------------------------------------ |
| conda list                            | 查看当前环境中已安装的包                                     |
| conda search scikit-learn             | 看看scikit-learn这个还没安装的包在Anaconda库里有没有（要在联网环境下） |
| conda install scikit-learn            | 安装scikit-learn                                             |
| python --version                      | 查看当前环境下Python的版本                                   |
| conda info --envs                     | 展示conda下所有的环境                                        |
| conda create --name snakes python=3.5 | 新建一个名为snakes的环境，Python版本是3.5                    |
| conda activate snakes                 | 激活snakes环境（默认是base）                                 |

## 其他奇奇怪怪的问题

1. `PermissionError: [Error 13] Permission denied: 'xxx.txt'`

   队友传给我的代码在她本地运行没有什么问题（当然说的是用一小部分数据测试代码的时候），然后在服务器上就出现了这个问题。

   代码里导入数据的部分文件路径我早就改了，只是我没想到后面代码还有写文件的操作，而且在`open('xxx.txt')`的时候，同样要带上绝对路径，这和平时在自己电脑上运行是不一样的。

   后来在`plt.savefig('xxx.png')`的时候碰到了类似的问题，同样改成绝对路径就可以了。

2. 如果还有，再补充吧