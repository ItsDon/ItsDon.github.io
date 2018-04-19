---
title:  Redex  ---  Android 字节码优化器 
author: Don
date: 2018-4-19 10:52:20
tags:
    - Performance Optimization
categories:
    - Tech
---

### Redex简介

 Redex是一款Facebook公司开发的Android字节码(dex)优化器.它提供了一个读写和分析.dex文件的框架，以及一系列基于该框架优化字节码的优化通道。利用Redex优化过的apk可以比之前的包体积更小，运行速度更快。
 
 Redex已在[github](https://github.com/facebook/redex)上开源，开发语言是C++.

 ### Redex实践（Mac系统）
 
 #### 安装Xcode命令行工具

 打开shell,输入一下命令

 ```xcode-select
    xcode-select --install

 ```
 安装成功后，输入`xcode-select -v`进行验证

 <!-- more -->

 #### 使用`homebrew`安装需要的依赖包
 homebrew是一个mac系统上的包管理工具，输入以下命令：
 ```
    brew install autoconf automake libtool python3
    brew install boost jsoncpp

 ```

 #### Download,Build and Install

 从github下载Redex
 ```
  mkdir redex
  git clone https://github.com/facebook/redex.git .

 ```
 利用`autoconf`和`make`编译Redex
 ```
  # if you're using gcc,please use gcc-4.9
  autoreconf -ivf && ./configure && make -j4
  sudo make install

 ```

#### Usage

一条语句搞定
```
    redex path/to/your.apk -o path/to/output.apk

```

Badly, An error occured.Just like below
```
 $ redex ~/Desktop/apk/pippa/dev/FlyBike-jap-release.apk -o outputt.apk 
Traceback (most recent call last):
  File "/tmp/redex.SAHD58/redex.py", line 687, in <module>
    run_redex(args)
  File "/tmp/redex.SAHD58/redex.py", line 633, in run_redex
    args.keyalias, args.keypass, args.ignore_zipalign, args.page_align_libs)
  File "/tmp/redex.SAHD58/redex.py", line 292, in create_output_apk
    zipalign(unaligned_apk_path, output_apk_path, ignore_zipalign, page_align)
  File "/tmp/redex.SAHD58/redex.py", line 248, in zipalign
    subprocess.check_call(zipalign + args)
  File "/usr/local/Cellar/python/3.6.5/Frameworks/Python.framework/Versions/3.6/lib/python3.6/subprocess.py", line 286, in check_call
    retcode = call(*popenargs, **kwargs)
  File "/usr/local/Cellar/python/3.6.5/Frameworks/Python.framework/Versions/3.6/lib/python3.6/subprocess.py", line 267, in call
    with Popen(*popenargs, **kwargs) as p:
  File "/usr/local/Cellar/python/3.6.5/Frameworks/Python.framework/Versions/3.6/lib/python3.6/subprocess.py", line 709, in __init__
    restore_signals, start_new_session)
  File "/usr/local/Cellar/python/3.6.5/Frameworks/Python.framework/Versions/3.6/lib/python3.6/subprocess.py", line 1344, in _execute_child
    raise child_exception_type(errno_num, err_msg, err_filename)
FileNotFoundError: [Errno 2] No such file or directory: 'zipalign': 'zipalign'

```


