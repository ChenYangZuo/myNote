# ESP32 Linux开发环境部署 原始版本

修改时间：2021.01.30 21:14

使用环境：Deepin 20.1

## 安装依赖环境

### 安装依赖文件

`sudo apt-get install git wget flex bison gperf python3 python3-pip python3-setuptools cmake ninja-build ccache libffi-dev libssl-dev dfu-util libusb-1.0-0`

## 获取ESP-IDF

### 使用Git获取

`mkdir -p ~/esp`
`cd ~/esp`
`git clone --recursive https://github.com/espressif/esp-idf.git`

### 直接下载

前往乐鑫官网下载页面下载压缩包，解压至`~/esp`目录

## 安装前设置

### 将默认Python版本切换为Python3

`sudo update-alternatives --install /usr/bin/python python /usr/bin/python3 10 && alias pip=pip3`

### 设置环境变量1

防止安装脚本运行时报错

`export IDF_PATH='~/esp/esp-idf'`

## 安装ESP-IDF

### 运行安装脚本

`cd ~/esp/esp-idf`
`./install.sh`

### 设置环境变量

在运行脚本前运行此代码，将环境变量导入当前终端

`. $HOME/esp/esp-idf/export.sh`

## 第一个工程

### 新建工程

复制ESP-IDF自带的Example进行测试

`cd ~/esp`
`cp -r $IDF_PATH/examples/get-started/hello_world .`

### 连接设备

使用`ls /dev/tty*`查看当前设备连接情况后，插入设备，再次输入命令，新增设备即为插入设备

### 配置工程

`cd ~/esp/hello_world`
`idf.py set-target esp32`
`idf.py menuconfig`

若报错可指定python3运行idf.py文件

### 编译工程

`idf.py build`

### 烧录二进制文件

`idf.py -p PORT [-b BAUD] flash`

PORT为前一步所得到的插入设备端口

烧录时注意要长按开发板上的复位键

### 查看调试信息

`idf.py -p PORT monitor`

可以观察到开发板的运行状况，使用快捷键 `Ctrl+]`退出 IDF 监视器