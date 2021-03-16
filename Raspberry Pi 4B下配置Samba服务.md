# Raspberry Pi 4B下配置Samba服务

修改时间：2021.03.16 10.49

使用环境：Raspbian

## 安装Samba

```
sudo apt-get update
apt-get install samba samba-common-bin
```

## 配置Samba

终端输入`sudo nano /etc/samba/smb.conf`打开配置文件

在末尾添加下列内容，注意修改smb_name与disk_name

```
security = user
[smb_name]
path = /media/pi/disk_name
valid users = @users
force group = users
create mask = 0660
directory mask = 0771
read only = no
```

使用命令`smbpasswd -a pi`将用户加入Samba中

## 设置系统

终端输入`sudo nano /etc/rc.loca`

追加`sudo /etc/init.d/samba restart`使Samba服务开机自启动