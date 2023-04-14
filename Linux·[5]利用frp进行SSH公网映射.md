# 利用frp进行SSH公网映射

使用环境：Ubuntu 18.04 LTS

## 前言

由于NAT路由器的存在，广域网无法直接访问局域网的计算机，因此需要利用广域网中拥有公网IP的服务器进行内网穿透。

frp 是一个专注于内网穿透的高性能的反向代理应用，支持 TCP、UDP、HTTP、HTTPS 等多种协议。可以将内网服务以安全、便捷的方式通过具有公网 IP 节点的中转暴露到公网。其工作原理如下：

1. 服务端运行，监听一个主端口，等待客户端的连接；
2. 客户端连接到服务端的主端口，同时告诉服务端要监听的端口和转发类型；
3. 服务端fork新的进程监听客户端指定的端口；
4. 外网用户连接到客户端指定的端口，服务端通过和客户端的连接将数据转发到客户端；
5. 客户端进程再将数据转发到本地服务，从而实现内网对外暴露服务的能力。

## 部署frps

### 配置文件

在具有公网 IP 的机器上部署 frps，修改 frps.ini 文件，这里使用了最简化的配置，设置了 frp 服务器用户接收客户端连接的端口：

```ini
[common]
bind_port = 7000
```

### 部署服务

输入`vim /etc/systemd/system/frps.service`创建并编辑文件，写入内容

```
[Unit]
# 服务名称，可自定义
Description = frp server
After = network.target syslog.target
Wants = network.target

[Service]
Type = simple
ExecStart = /path/to/frps -c /path/to/frps.ini

[Install]
WantedBy = multi-user.target
```

使用 `systemd` 命令，管理 frps

```
# 启动frp
systemctl start frps
# 停止frp
systemctl stop frps
# 重启frp
systemctl restart frps
# 查看frp状态
systemctl status frps
# 开机自启
systemctl enable frps
```



## 部署frpc

### 配置文件

在需要被访问的内网机器上（SSH 服务通常监听在 22 端口）部署 frpc，修改 frpc.ini 文件，假设 frps 所在服务器的公网 IP 为 x.x.x.x：

```ini
[common]
tls_enable = true
server_addr = x.x.x.x
server_port = 7000

[ssh]
type = tcp
local_ip = 127.0.0.1
local_port = 22
remote_port = 6000
```

`local_ip` 和 `local_port` 配置为本地需要暴露到公网的服务地址和端口。`remote_port` 表示在 frp 服务端监听的端口，访问此端口的流量将会被转发到本地服务对应的端口。

### 部署服务

输入`vim /etc/systemd/system/frpc.service`创建并编辑文件，写入内容

```
[Unit]
# 服务名称，可自定义
Description = frp client
After = network.target syslog.target
Wants = network.target

[Service]
Type = simple
ExecStart = /path/to/frpc -c /path/to/frpc.ini

[Install]
WantedBy = multi-user.target
```

使用 `systemd` 命令，管理 frpc

```
# 启动frp
systemctl start frpc
# 停止frp
systemctl stop frpc
# 重启frp
systemctl restart frpc
# 查看frp状态
systemctl status frpc
# 开机自启
systemctl enable frpc
```

**关联文档：**
上一篇：[[Linux·[4]在Ubuntu上部署QT6开发环境]]
下一篇：[[Linux·[6]在Ubuntu上部署XDMA环境]]