# 在Ubuntu上安装PetaLinux

系统环境：Ubuntu 18.04.6

## 操作步骤

### 安装依赖

#### 换源

```shell
sudo sed -i "s@http://.*archive.ubuntu.com@https://mirrors.tuna.tsinghua.edu.cn@g" /etc/apt/sources.list
sudo sed -i "s@http://.*security.ubuntu.com@https://mirrors.tuna.tsinghua.edu.cn@g" /etc/apt/sources.list
```

#### 安装依赖

```shell
sudo apt-get install tofrodos iproute2 gawk gcc g++ git make net-tools libncurses5-dev \
tftpd zlib1g:i386 libssl-dev flex bison libselinux1 gnupg wget diffstat chrpath socat \
xterm autoconf libtool tar unzip texinfo zlib1g-dev gcc-multilib build-essential \
libsdl1.2-dev libglib2.0-dev screen pax gzip automake
```

### 建立目录

```shell
sudo chown -R $USER:$USER /opt
mkdir /opt/petaLinux
```

### 安装PetaLinux

#### 文件提权

```shell
sudo chmod 755 ./petalinux-v2021.1-final-installer.run
```

#### 安装PetaLinux

```shell
./petalinux-v2021.1-final-installer.run -d /opt/petaLinux
```

#### 修改Bash

```shell
sudo dpkg-reconfigure dash
```

选择`No`

#### 导入环境变量

```shell
source /opt/petaLinux/settings.sh
```

每次打开终端时都需要执行

