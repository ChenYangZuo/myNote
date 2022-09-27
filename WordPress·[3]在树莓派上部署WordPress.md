# 在树莓派上部署WordPress

修改时间：2021.03.17 13:00

使用环境：Raspberry Pi OS with desktop January 11th 2021

## 更换软件源

使用`sudo nano /etc/apt/sources.list`编辑软件源

注释掉原来的内容，加入如下：

```
deb http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ buster main non-free contrib rpi

deb-src http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ buster main non-free contrib rpi
```

使用`sudo nano /etc/apt/sources.list.d/raspi.list`

注释掉原来的内容，加入如下：

```
deb http://mirrors.tuna.tsinghua.edu.cn/raspberrypi/ buster main ui
```

执行`sudo apt-get update`更新

## 部署LNMP

### 部署Nginx

#### 安装Nginx

执行`sudo apt-get install nginx`安装Nginx

#### 让Nginx处理PHP

执行`sudo nano /etc/nginx/sites-available/default`

将内容：

```
location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files $uri $uri/ =404;
        }
```

替换为：

```
location / {
index  index.html index.htm index.php default.html default.htm default.php;
}
 
location ~\.php$ {
fastcgi_pass unix:/run/php/php7.3-fpm.sock;
#fastcgi_pass 127.0.0.1:9000;
fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
include fastcgi_params;
}
```

保存退出后执行`sudo service nginx restart`重启Nginx

### 部署PHP

执行`sudo apt-get install php7.3-fpm php7.3-cli php7.3-curl php7.3-gd php7.3-cgi php7.3-mysql`安装

执行`sudo service php7.3-fpm restart`启动

### 部署MySQL

#### 安装MySQL

执行`sudo apt-get install mariadb-server-10.0`安装MySQL

#### 配置数据库

执行`sudo mysql -u root -p`，直接回车进入，第一次无需输入密码

执行如下命令，注意每条命令后的分号

输入`CREATE DATABASE wordpress;`创建名为wordpress的数据库

输入`GRANT ALL ON wordpress.* TO wordpressuser@localhost IDENTIFIED BY 'BLOck@123';`为数据库创建用户并为用户分配数据库的完全访问权限，其中wordpressuser为用户名，BLOck@123为密码

输入`exit;`退出MySQL

## 安装WordPress

执行`wget https://cn.wordpress.org/latest-zh_CN.tar.gz`下载最新版WordPress

执行`tar -zxvf latest-zh_CN.tar.gz`解压文件，当前目录下生成wordpress文件夹

执行`rm -rf /var/www/html`删除html文件夹

执行`mv wordpress /var/www/html`将wordpress文件夹复制为html文件夹

执行`chmod -R 777 /var/www/html`将文件夹提权

浏览器输入`http://树莓派内网IP`即可进入WordPress配置页面，点击现在就开始

数据库名为wordpress，用户名与密码在上一步设置过，其余默认，点击提交

输入站点标题，用户名与电子邮箱，记录下密码，点击安装WordPress

安装完成后浏览器输入`http://树莓派内网IP`即可进入博客

**关联文档：**
上一篇：[[WordPress·[2]在华为云上部署WordPress II]]
下一篇：无