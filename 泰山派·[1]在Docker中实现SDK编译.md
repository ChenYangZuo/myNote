# 在Docker中实现泰山派SDK编译

## 1 安装Docker环境

> 本段选自[Docker官网](https://docs.docker.com/engine/install/ubuntu/)

### 1.1 卸载原有Docker

运行如下命令卸载已有Docker：

```shell
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

### 1.2 添加APT源

运行如下命令添加Docker官方源：

```shell
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

### 1.3 安装Docker

运行如下命令安装Docker

```shell
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```



## 2 创建编译环境

### 2.1 创建镜像

进入工作目录，输入`mkdir docker_lckfb_tspi_ubuntu_sdk`创建目录，输入`cd docker_lckfb_tspi_ubuntu_sdk`进入目录

使用vim创建脚本`vim dockerfile`

输入如下内容：

```dockerfile
# 设置基础镜像为Ubuntu 18.04
FROM ubuntu:18.04

# 设置作者信息
MAINTAINER zuochenyang "zuo-cy2008@163.com"

# 设置环境变量，用于非交互式安装
ENV DEBIAN_FRONTEND=noninteractive

# 将源列表中的 http://.*ubuntu.com 替换为 http://repo.huaweicloud.com
RUN sed -i 's@http://.*ubuntu.com@http://repo.huaweicloud.com@g' /etc/apt/sources.list

# 更新包列表
RUN apt update

# 安装基本的编译工具和依赖
RUN apt install -y build-essential crossbuild-essential-arm64 \
        bash-completion vim sudo locales time rsync bc python

RUN apt install -y git ssh make gcc libssl-dev liblz4-tool expect \
		g++ patchelf chrpath gawk texinfo chrpath diffstat binfmt-support \
		qemu-user-static live-build bison flex fakeroot cmake gcc-multilib \
		g++-multilib unzip device-tree-compiler ncurses-dev whiptail

# 安装其他依赖包，这里编译android11sdk需要的环境
RUN apt install -y repo git ssh libssl-dev liblz4-tool lib32stdc++6 \
        expect patchelf chrpath gawk texinfo diffstat binfmt-support \
        qemu-user-static live-build bison flex fakeroot cmake \
        unzip device-tree-compiler python-pip ncurses-dev python-pyelftools \
        subversion asciidoc w3m dblatex graphviz python-matplotlib cpio \
        libparse-yapp-perl default-jre patchutils swig expect-dev u-boot-tools 

RUN apt install -y bear

# 再次更新包列表并安装任何未安装的依赖
RUN apt update && apt install -y -f

# 生成本地化语言支持
RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8

# 创建开发板用户
RUN useradd -c 'lckfb user' -m -d /home/lckfb -s /bin/bash lckfb

# sudo免密登录
RUN sed -i -e '/\%sudo/ c \%sudo ALL=(ALL) NOPASSWD: ALL' /etc/sudoers
RUN usermod -a -G sudo lckfb

USER lckfb
#设置docker工作目录为/home/lckfb
WORKDIR /home/lckfb

#容器使用这个内核方法
#
```

在当前目录下运行`sudo docker build -t lckfb_ubuntu_sdk_cmp .`创建名为`lckfb_ubuntu_sdk_cmp`的镜像

### 2.2 创建容器

输入如下命令创建容器：

```shell
sudo docker run --privileged --mount type=bind,source=/media/star/Data1/zuochenyang/lckfb_tspi_ubuntu_sdk,target=/home/lckfb/lckfb_tspi_ubuntu_sdk --name="lckfb_ubuntu_sdk" -h lckfb -it lckfb_ubuntu_sdk_cmp
```

其中，`lckfb_tspi_ubuntu_sdk`为解压的SDK文件夹

### 2.3 使用容器

在使用过程中，可以使用`exit`命令退出容器

再次使用时，使用`sudo docker start lckfb_ubuntu_sdk `启动容器，并使用`sudo docker attach lckfb_ubuntu_sdk`附加到正在运行的名为lckfb_ubuntu_sdk的容器的终端上，以便与容器进行交互



## 3 编译SDK

### 3.1 板级配置

输入`./build.sh lunch`，在弹出的选项中输入`3`以选择泰山派的板级配置

### 3.2 编译buildroot

通过`export RK_ROOTFS_SYSTEM=buildroot`选择buildrot操作系统

使用`./build.sh all`编译所有模块的代码。在第一次运行编译时，会弹出电源选项，其中，仅VCCIO4与VCCIO6选择1.8V，其余均选择3.3V。

接下来开始漫长等待

编译成功后使用`./mkfirmware.sh`打包固件