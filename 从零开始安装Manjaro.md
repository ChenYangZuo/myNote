# 从零开始安装Manjaro

修改时间：2021.03.27 11:29

使用环境：VirtualBox+Manjaro 2021.1 全量版

## 安装系统

### 下载镜像

进入https://manjaro.org/downloads/official/kde/下载**Minimal**版的镜像

下载Rufus2.18版本，选择DD模式写入U盘

### 安装系统

关闭BIOS中BOOT Source项，选择U盘引导启动

按引导安装系统，安装时保持断网

## 基础配置

### 更换系统源

命令行输入`sudo pacman-mirrors -i -c China -m rank`，在弹出窗口中选择合适的镜像源

命令行输入`sudo pacman -Syyu`更新系统

### 添加ArchLinuxcn源

命令行输入`sudo nano /etc/pacman.conf`，在最后输入以下内容：

```text
[archlinuxcn]
SigLevel = Optional TrustAll
Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch
```

运行`sudo pacman -Syy`刷新镜像

运行`sudo pacman -S archlinuxcn-keyring`安装密钥

### 安装AUR工具

运行`sudo pacman -Sy yay`安装yay工具

### 配置base-devel工具

运行`yay -Sy gcc`与`yay -Sy fakeroot`

**注意在此及之后使用yay时不要使用sudo权限**

**切勿使用`sudo pacman -S base-devel`命令直接安装base-devel工具**

## 安装腾讯软件

使用`yay -S com.qq.im.deepin`安装QQ

使用`yay -S com.qq.weixin.deepin`安装微信

因使用KDE桌面环境，需要额外`sudo pacman -S xsettingsd`

复制windows全部字体进 **.deepinwine/Deepin-QQ/drive_c/windows/Fonts** 和**.deepinwine/Deepin-WeChat/drive_c/windows/Fonts**目录下

初次运行会自动配置wine

## 安装WPS

使用`yay -S wps-office-cn`安装国内版WPS

使用`yay -S wps-office-mui-zh-cn`安装中文语言包

使用`yay -S ttf-wps-fonts`补充缺失字体

运行`sudo nano /usr/bin/wps`，在第一行下补充如下代码：

```text
export XMODIFIERS="@im=fcitx5"
export QT_IM_MODULE="fcitx5"
gOpt=
# gOptExt=-multiply
gTemplateExt=("wpt" "dot" "dotx")
```

## 配置输入法

依次运行如下代码：

```text
sudo pacman -S fcitx5-im
sudo pacman -S fcitx5-chinese-addons
sudo pacman -S fcitx5-material-color
```

运行`nano ~/.xprofile `添加如下代码：

```text
export GTK_IM_MODULE=fcitx5
export QT_IM_MODULE=fcitx5
export XMODIFIERS="@im=fcitx5"
fcitx5 &
```

运行`nano ~/.xinitrc`在`exec $(get_session)`前添加：

```
export GTK_IM_MODULE=fcitx5
export XMODIFIERS=@im=fcitx5
export QT_IM_MODULE=fcitx5
```

在设置中将**工具**中的fcitx5加入开机启动项

运行`sudo reboot now`重启计算机

打开fcitx配置工具，进行相关配置

## 其他软件

### 网易云音乐

```text
yay -Sy netease-cloud-music
```

### QQ音乐

```text
yay -Sy cocomusic
```

### V2ray

```text
yay -Sy qv2ray
yay -Sy v2ray
```

### Edge

```
yay -S microsoft-edge-dev-bin
```

在地址栏输入`edge://flags`打开实验性选项页面，接着搜索`MSA sign in`，将右侧的选项改为**Enabled**

待后续补充

## 常见问题

**ERROR: Cannot find the strip binary required for object file stripping.**

解决：安装gcc与fakeroot

**安装软件显示：已损坏 (无效或已损坏的软件包 (PGP 签名))**

解决：配置archlinuxcn时SigLevel=Optional TrustAll

**开机后会打开关机之前的所有窗口**

解决：`kcmshell5 kcm_smserver`后勾选**以空白会话启动**