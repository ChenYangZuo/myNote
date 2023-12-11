# 在Ubuntu上使用罗技鼠标驱动

修改时间：2023年04月24日19:07:21

使用环境：Ubuntu22.04

## 安装依赖

```shell
sudo apt install cmake libevdev-dev libudev-dev libconfig++-dev
```

## 拉取第三方驱动源码并编译

```shell
git clone https://github.com/PixlOne/logiops.git
mkdir build
cd build
cmake ..
make
sudo make install
```

## 修改配置项

打开配置文件：

```shell
vim /etc/logid.cfg
```

将配置项写入配置文件中：

```
devices: (
{
    name: "M720 Triathlon Multi-Device Mouse";
    buttons: (
        {
            cid: 0x56;
            action =
            {
                type: "Keypress";
                keys: ["KEY_LEFTALT","KEY_LEFT"];
            };
        },
        {
            cid: 0x53;
            action =
            {
                type: "Keypress";
                keys: ["KEY_LEFTALT","KEY_RIGHT"];
            };
        },
        {
            cid: 0xd0;
            action =
            {
                type: "Keypress";
                keys: ["KEY_LEFTMETA", "KEY_D"];
            };
        }
    );
    hiresscroll:
    {
        hires: true;
        invert: false;
        target: false;
    };
}
);
```

## 配置系统服务

```shell
sudo systemctl enable --now logid
```

**关联文档：**
上一篇：[[Linux·[6]在Ubuntu上部署XDMA环境]]
下一篇：[[Linux·[8]在Ubuntu上使用screen]]