# 在Ubuntu上部署QT6开发环境

修改时间：2022.06.28 14:15

使用环境：Ubuntu22.04

## 下载QT在线安装包

在[清华QT镜像站](https://mirrors.tuna.tsinghua.edu.cn/qt/official_releases/online_installers/)下载.run格式的安装包

## 对安装包提权

终端输入`sudo chmod 777 ./qt-unified-linux-x64-4.4.1-online.run`对安装包进行提权。

## 使用镜像服务器安装

打开终端输入`./qt-unified-linux-x64-4.4.1-online.run --mirror https://mirrors.tuna.tsinghua.edu.cn/qt`使用清华源安装

## 选择开发套件

在安装时选择Desktop套件

## 安装必要依赖库

打开终端输入`sudo apt install libgl-dev`安装OpenGL依赖

**关联文档：**
上一篇：[[Linux·[2]Linux常用软件安装]]
下一篇：[[Linux·[5]利用frp进行SSH公网映射]]