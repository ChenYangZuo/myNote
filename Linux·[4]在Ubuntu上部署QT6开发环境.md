# 在Ubuntu上部署QT6开发环境

修改时间：2022.06.28 14:15

使用环境：Ubuntu22.04

## 下载QT在线安装包

在QT官网下载.run格式的安装包

## 对安装包提权

右键安装包，点击属性，在权限中勾选允许执行文件

## 使用镜像服务器安装

打开终端输入`./qt-unified-linux-x64-4.4.1-online.run --mirror https://mirrors.tuna.tsinghua.edu.cn/qt`使用清华源安装

## 选择开发套件

在安装时选择Desktop套件

## 安装必要依赖库

打开终端输入`sudo apt install libgl-dev`安装OpenGL依赖

