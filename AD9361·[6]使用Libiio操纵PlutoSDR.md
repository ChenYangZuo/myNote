# 使用Libiio操纵PlutoSDR

## 前置条件

1. 安装libiio
2. 安装linad9361-iio（不一定用得到）

## 测试代码

```c
#include <iio.h>
#include <stdio.h>

int receive(struct iio_context *ctx) {
    struct iio_device *dev;
    struct iio_channel *rx0_i, *rx0_q;
    struct iio_buffer *rxbuf;

    // 在uri创建的context中获取cf-ad9361-lpc设备
    dev = iio_context_find_device(ctx, "cf-ad9361-lpc");
    // 在设备中获取I、Q的数据通道
    rx0_i = iio_device_find_channel(dev, "voltage0", 0);
    rx0_q = iio_device_find_channel(dev, "voltage1", 0);
    // 使能数据通道
    iio_channel_enable(rx0_i);
    iio_channel_enable(rx0_q);
    // 创建非圆形数据缓冲区（读取一个数据后其余数据需要搬移）
    rxbuf = iio_device_create_buffer(dev, 4096, false);
    if (!rxbuf) {
        perror("Could not create RX buffer");
        shutdown();
    }

    while (true) {
        void *p_dat, *p_end, *t_dat;
        ptrdiff_t p_inc;

        // 从硬件获取数据填满缓冲区
        iio_buffer_refill(rxbuf);

        // Get the step size between two samples of one channel.
        p_inc = iio_buffer_step(rxbuf);
        // Get the address that follows the last sample in a buffer.
        p_end = iio_buffer_end(rxbuf);

        // iio_buffer_first()
        // Find the first sample of a channel in a buffer.

        for (p_dat = iio_buffer_first(rxbuf, rx0_i); p_dat < p_end; p_dat += p_inc, t_dat += p_inc) {
            const int16_t i = ((int16_t*)p_dat)[0]; // Real (I)
            const int16_t q = ((int16_t*)p_dat)[1]; // Imag (Q)

            /* Process here */
            printf("i:%d q:%d\r\n",i,q);
        }
    }

    // 回收缓冲区资源
    iio_buffer_destroy(rxbuf);

}

int main (int argc, char **argv) {
    struct iio_context *ctx;
    struct iio_device *phy;
 
    // 通过uri创建context
    ctx = iio_create_context_from_uri("ip:192.168.2.1");
    // 寻找ad9361-phy设备用于配置信息
    phy = iio_context_find_device(ctx, "ad9361-phy");
 
    // iio_device_find_channel(phy, "altvoltage0", true) -> 找到RX的LO配置通道
    // Key: "frequency"  Value: 2.4G
    // 配置接收通道的中心（本振）频率为2.4GHz
    iio_channel_attr_write_longlong(
        iio_device_find_channel(phy, "altvoltage0", true),
        "frequency",
        2400000000
    ); /* RX LO frequency 2.4GHz */
 
    // iio_device_find_channel(phy, "voltage0", false) -> 找到RX的Phy配置通道
    // Key: "sampling_frequency" Value: 5M
    // 配置接收通道的采样率为5MSPS
    iio_channel_attr_write_longlong(
        iio_device_find_channel(phy, "voltage0", false),
        "sampling_frequency",
        5000000
    ); /* RX baseband rate 5 MSPS */
 
    // 接收数据
    receive(ctx);
 
    // 回收context资源
    iio_context_destroy(ctx);
 
    return 0;
} 

```

打开命令行，输入`gcc -o test_iio test_iio.c -liio`完成编译。

