# 在Ubuntu上使用screen

修改时间：2023年12月11日09:46:44

使用环境：Ubuntu18.04

## 前言

在使用SSH登录服务器进行操作时，如果终端意外断开，则会丢失上下文信息，因此使用screen托管上下文，防止重要内容丢失

## 安装依赖

```shell
sudo apt install screen
```

## 使用方法

### 创建screen终端

```
screen -S name
```

创建名字为`name`的终端

### 查看终端

```
screen -ls
```

列表显示所有已创建的screen终端

### 操作

在screen中可按一般终端进行操作

使用`Ctrl + A` + `D`退出screen终端

### 修改配置项以支持滚轮

使用`vim ~/.screenrc`打开screen的配置文件

输入`termcapinfo xterm* ti@:te@`并`:wq`完成配置

### 恢复终端

```
screen -r name
```

恢复名为name的终端

### 踢出终端

当使用screen终端时意外断开后，该终端会保持Attached状态， 无法进入，此时使用

```
screen -d name
```

将终端状态改为Detached状态



**关联文档：**
上一篇：[[Linux·[7]在Ubuntu上使用罗技鼠标驱动]]
下一篇：无