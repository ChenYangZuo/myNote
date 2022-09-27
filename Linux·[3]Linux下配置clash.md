# Linux下配置clash

修改时间：2021.01.31 11:47

使用环境：Deepin 20.1 clash1.3.5

## 下载clash二进制文件

进入[https://github.com/Dreamacro/clash/releases](https://github.com/Dreamacro/clash/releases)下载最新版二进制文件，注意系统类型

将二进制文件放在`~/clash/`文件夹中

右键clash二进制文件，允许文件以程序方式启动

## 下载config文件

去机场下载clash所需的config文件

将config文件放在`~/clash/`文件夹中

## 等待下载Country.mmdb文件

clash会自动下载Country.mmdb文件，该步骤会很慢，可以使用手机代理后开放热点进行下载

## 配置clash

浏览器进入[http://clash.razord.top/](http://clash.razord.top/)配置相关节点

配置完成后在系统代理中设置HTTP代理为`http://127.0.0.1:7890`

设置Socks代理为`http://127.0.0.1:7891`

在终端中可使用`export http_proxy="http://127.0.0.1:7890"`

`export https_proxy="http://127.0.0.1:7890"`针对当前终端进行代理

## 手动开启clash

终端输入以下命令可以手动开启clash

```
cd clash
./clash -d .
```

使用这种方法打开clash时，使用全程不能关闭当前终端

## 加载系统服务

进入clash文件夹，将文件复制到相应位置

```
cd clash
cp clash /usr/local/bin
cp config.yaml /etc/clash/
cp Country.mmdb /etc/clash/
```

使用`sudo nano /etc/systemd/system/clash.service`新建服务，输入：

```
[Unit]
Description=Clash Daemon

[Service]
ExecStart=/usr/local/bin/clash -d /etc/clash/

[Install]
WantedBy=multi-user.target
```

使用`systemctl enable clash`设置clash开机自启动

使用`systemctl start clash`开启clash

使用`systemctl restart clash`重启clash

使用`systemctl stop clash`关闭clash

使用`systemctl status clash`查看服务运行状况

使用`systemctl list-units --type=service`查看当前正在运行的服务


**关联文档：**
上一篇：[[Linux·[2]Linux常用软件安装]]
下一篇：[[Linux·[4]在Ubuntu上部署QT6开发环境]]