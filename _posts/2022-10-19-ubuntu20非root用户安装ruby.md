---
layout: post
title: Ubuntu20.04非root用户安装ruby
date: 2022-10-19 19:39 +0800
description:
category: [环境配置, Linux]
tags:
math: false
mermaid: false
comments: true
---

# 安装依赖
## 安装openssl

从[openssl](https://www.openssl.org/source/)官网下载需要的openssl版本，本次安装的是openssl 1.1.1q
```
wget https://www.openssl.org/source/openssl-1.1.1q.tar.gz
```
解压缩
```
tar zxvf openssl-1.1.1q.tar.gz
```
进入解压后的目录并配置文件
```
cd openssl-1.1.1q
./config --prefix=/home/username/openssl --openssldir=/home/username/openssl
```
prefix用于配置安装路径，可以按需要更改
安装
```
make && make install
```
添加 PATH 到~/.bashrc文件
```
export PATH=$HOME/openssl/bin:$PATH
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$HOME/openssl/lib64
export LC_ALL="en_US.UTF-8"
export LDFLAGS="$LDFLAGS -L/home/username/openssl/lib -Wl,-rpath,/home/username/openssl/lib"
```
注意LDFAGS两个路径之间使用的是空格分隔不是冒号
之后更新参数即可
```
sourc ~/.bashrc
```
查看默认openssl是不是自己当前安装的，以及版本
```
which openssl
openssl version
```
## 安装zlib
安装过程类似
从[官网](https://zlib.net/)下载需要的zlib版本
```
wget https://zlib.net/zlib-1.2.13.tar.gz
```
解压后安装
```
tar zxvf zlib-1.2.13.tar.gz
cd zlib-1.2.13
./configure --prefix=/your_path_to/zlib
make && make install
```
在 .bashrc 中增加变量
```
export PATH =$PATH:/your_path_to/zlib
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/your_path_to/zlib/lib/
export LIBRARY_PATH=$LIBRARY_PATH:/your_path_to/zlib/lib/
export C_INCLUDE_PATH=/your_path_to/zlib/include/
export CPLUS_INCLUDE_PATH=/your_path_to/zlib/include/
export PKG_CONFIG_PATH=/your_path_to/zlib/lib/pkgconfig
```
对于 C_INCLUDE_PATH、CPLUS_INCLUDE_PATH、PKG_CONFIG_PATH 变量，如果之前有使用过，最好加入之前的路径
```
export C_INCLUDE_PATH=/your_path_to/zlib/include/:$C_INCLUDE_PATH
export CPLUS_INCLUDE_PATH=/your_path_to/zlib/include/:$CPLUS_INCLUDE_PATH
export PKG_CONFIG_PATH=/your_path_to/zlib/lib/pkgconfig:$PKG_CONFIG_PATH
```
但如果之前没有就不要加入了，否则容易出现空路径，导致编译器将当前所在目录也加入 PATH 中，出现不期望的结果，其余的PATH也要注意类似的问题。
之后source 一下 ~/.bashrc 即可
# 安装
安装方法类似，从[官网](http://www.ruby-lang.org/en/downloads/)下载安装包
```
wget https://cache.ruby-lang.org/pub/ruby/3.1/ruby-3.1.2.tar.gz
```
解压并进入文件夹
```
tar zxvf ruby-3.1.2.tar.gz
cd ruby-3.1.2
```
配置文件
```
./configure --prefix=/your_path_to/ruby --with-openssl-dir=/home/username/openssl
```
后面是指定openssl安装的位置
```
make && make install
```
~/.bashrc 中添加路径
```
PATH=$PATH:/your_path_to/ruby/bin
```
之后source ~/.bashrc 即可
查看ruby和gem版本
```
ruby -v
gem -v
```
参考：
1. Installing OpenSSL locally under your username, <https://help.dreamhost.com/hc/en-us/articles/360001435926-Installing-OpenSSL-locally-under-your-username>
2. 惧色, 已解决｜No such file #include zlib.h｜非root权限, <https://zhuanlan.zhihu.com/p/484168155>
3. 毅斯丶, Liunx 离线安装Ruby（非ROOT用户）, <https://blog.csdn.net/qq1340976576/article/details/88426784>
