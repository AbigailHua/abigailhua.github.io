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

   之后会要求输入密码，然后显示连接成功。登录的那一瞬间连命令行字体都会变成Linux风，看得我虎躯一震。

   忍不住吐槽一下powershell里蓝底居然还会显示蓝字，真不知道设计师怎么想的...还是滚回去用cmd吧

2. Pycharm连接远程服务器

   命令行界面对我等用惯了Windows系统的人不太友好，幸好Pycharm专业版有显示远程主机文件夹的功能

   我参考了[这篇博客](https://blog.csdn.net/qq_40986693/article/details/103372301)中配置链接远程服务器和远程python解释器的部分

既舍弃不了命令行的简洁，又舍弃不了图形界面的直观，成年人当然两个都要！（所以本菜鸡就两个一起用了，各取其优点）

## 文件上传下载

搜了百度告诉我应该用scp（前几周百度面试官也让我了解一下scp，好巧哦）。一开始在cmd里死活传不上去，后来再探索了一阵子，应该这么操作：

不登录服务器，在cmd输入

```shell
>> scp -P [port] upload_file_name account@ip:/path
```

要注意的是，`-P`是大写的P，且一定要直接跟在scp后面。如果没有cd到upload_file_name所在的文件夹，就要使用绝对路径。path里不需要包含文件名，只要目录即可。另外，我是没有sudo权限的，所以目的文件夹有限制。

下载文件同样在Windows的cmd中进行，输入

```
>> scp -P [port] account@ip:download_file_path "D:/XXX/XXX/XX"
```

（看了一圈，因为linux没有sudo权限不能乱装东西，只有这个方法有用呜呜呜）

在做平时作业的时候，一个一个文件上传属实麻烦，再次感谢Pycharm的远程文件夹管理，上传/下载文件so easy（真的不是广告）

但是改完远程代码一定要上传！一定要上传！一定要上传！平时本地代码Pycharm是实时保存的，真是被惯坏了。

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

## 后台运行

因为之前是在Pycharm里运行远程代码，在远程运行期间就不能断网也不能关电脑。之前觉得没什么，把电脑晾一晚上就好了。但是这次作业耗时太久，一天一夜也没跑完，加上这几天半夜家里正好停电，赶ddl的进度就有点捉襟见肘了。幸好，队友提示我可以去搜搜`nohup`。

缺点是一定要cmd中运行，先上命令格式吧：

```bash
>> nohup python3 example.py>exampleout.log 2>&1 &
```

大致就是，nohup是不间断运行（no hang up的缩写），结尾的`&`是后台运行的意思，中间就是想要后台不间断运行的命令。

再详细解释一下中间的命令，0表示stdin (standard input)，1表示stdout (standard output)，2表示 stderr (standard error) 。2>&1是将标准错误（2）重定向到标准输出（&1），标准输出（&1）再被重定向输入到`exampleout.log`文件中。

提交nohup命令后，屏幕上会有一个进程编号。

既然后台运行，`ctrl+c`就不能中止这个进程，所以需要使用

```bash
>> kill [PID]
```

来杀死进程。这里的PID就是上面显示的进程编号。

那如果前一天关了电脑，忘了保存这个进程编号怎么办呢？我们可以用`ps`命令查看进程（参数`-ux`可只查看当前用户的进程），找到对应的PID就可以了。

## 其他奇奇怪怪的问题

1. `PermissionError: [Error 13] Permission denied: 'xxx.txt'`

   队友传给我的代码在她本地运行没有什么问题（当然说的是用一小部分数据测试代码的时候，因为全部数据跑不下来嘛），然后在服务器上就出现了这个问题。

   代码里导入数据的部分文件路径我早就改了，只是我没想到后面代码还有写文件的操作，而且在`open('xxx.txt')`的时候，同样要带上绝对路径，这和平时在自己电脑上运行是不一样的。

   后来在`plt.savefig('xxx.png')`的时候碰到了类似的问题，同样改成绝对路径就可以了。

2. 再提个和服务器无关的事儿吧，是tex的问题。

   拿队友传来的文件一编译，图片的`ref`找不到指代的图片了。

   平时我个人习惯是`caption`写在`label`前面的，但不知道顺序有讲究，所以一开始并没有想到是这方面问题，还想着是不是像函数声明一样`ref`得放得比`label`前面呀？后来一想不对，我的表格就没这个问题啊。之后才查到是`caption`和`label`的顺序很重要！

   敲黑板：`caption`可以后面加`label`，也可以把`label`写在`caption`里面，但就是不能把`label`写在`caption`前面！

   具体原理看[这里](https://tex.stackexchange.com/questions/431676/problem-with-label-and-references-and-subfig-subcaption)

3. 如果还有，再补充吧