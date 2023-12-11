# 在Ubuntu上部署XDMA环境

修改时间：2023年4月14日16:38:47

使用环境：Ubuntu20.04.6

## 安装必要依赖

使用`sudo apt install gcc g++ make git`安装必要依赖。

## 安装XDMA驱动

使用`https://github.com/Xilinx/dma_ip_drivers.git`下载Xilinx官方XDMA驱动程序源码。

1. 使用`cd dma_ip_drivers/XDMA/linux-kernel/xdma`进入工程。
2. `sudo make install`编译源码。
3. 使用`cd ../tools`进入目录。
4. 使用`make`编译源码
5. 使用`cd ../tests`进入目录。
6. 使用`sudo ./load_driver.sh`加载驱动。
7. 使用`sudo ./run_test.sh`运行脚本测试驱动是否正常允许。

## 安装Qt环境

具体可参照[[Linux·[4]在Ubuntu上部署QT6开发环境]]

## 对XDMA设备进行提权

```shell
sudo chmod 777 /dev/xdma0_h2c_0
sudo chmod 777 /dev/xdma0_c2h_0
```

注意此处根据Qt工程中使用的设备自行添加提权命令。例如CH03中使用了**xdma0_user**设备，需要用户自行提权。

**关联文档：**
上一篇：[[Linux·[5]利用frp进行SSH公网映射]]
下一篇：[[Linux·[7]在Ubuntu上使用罗技鼠标驱动]]