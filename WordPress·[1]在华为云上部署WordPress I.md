# 在华为云上部署WordPress I

修改时间：2021.03.17 13:28

使用环境：华为云ECS 1vCPUs|1GB|40G|1Mbps|CentOS 8.0  64bit with ARM

## 设置安全组规则

设置入方向，协议HTTP(80)，80端口，源地址0.0.0.0/0

## 部署LNMP

使用一键安装包，执行`wget http://soft.vpser.net/lnmp/lnmp1.7.tar.gz -cO lnmp1.7.tar.gz && tar zxf lnmp1.7.tar.gz && cd lnmp1.7 && ./install.sh lnmp`开始自动安装，大约需要15分钟

安装过程中选择MySQL5.5，开启MySQL InnoDB，选择PHP7.3，不安装内存优化

### 配置MySQL

执行`sudo mysql -u root -p`，直接回车进入，第一次无需输入密码

执行如下命令，注意每条命令后的分号

输入`CREATE DATABASE wordpress;`创建名为wordpress的数据库

输入`GRANT ALL ON wordpress.* TO wordpressuser@localhost IDENTIFIED BY 'BLOck@123';`为数据库创建用户并为用户分配数据库的完全访问权限，其中wordpressuser为用户名，BLOck@123为密码

输入`exit;`退出MySQL

### 配置PHP

执行如下命令启动PHP服务并设置开机自启动

```
systemctl start php-fpm

systemctl enable php-fpm
```

### 配置Nginx

执行`vim /usr/local/nginx/conf/nginx.conf`编辑Nginx配置文件，按i进入编辑模式

在server下添加下列代码

```
location / {
        root   /usr/local/nginx/html;
        index index.php index.html index.htm;
    }
```

```
    location ~ \.php$ {
        root           /usr/local/nginx/html;
        fastcgi_pass   127.0.0.1:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME /usr/local/nginx/html$fastcgi_script_name;
        include        fastcgi_params;
    }
```

并将server下方的root改为`/usr/local/nginx/html`，确保所有root一致

执行`service nginx reload`加载配置

## 安装WordPress

执行`wget https://cn.wordpress.org/latest-zh_CN.tar.gz`下载最新版WordPress

执行`tar -zxvf latest-zh_CN.tar.gz`解压文件，当前目录下生成wordpress文件夹

执行`rm -rf /usr/local/nginx/html`删除html文件夹

执行`mv wordpress /usr/local/nginx/html`将wordpress文件夹复制为html文件夹

执行`chmod -R 777 /usr/local/nginx/html`将文件夹提权

浏览器输入`http://华为云公网IP`即可进入WordPress配置页面，点击现在就开始

数据库名为wordpress，用户名与密码在上一步设置过，其余默认，点击提交

输入站点标题，用户名与电子邮箱，记录下密码，点击安装WordPress

安装完成后浏览器输入`http://华为云公网IP`即可进入博客

## 解决FTP问题

执行`nano /usr/local/nginx/html/wp-config.php`

在以下内容：

```
if ( !defined('ABSPATH') )

define('ABSPATH', dirname(__FILE__) . '/');
```

后添加：

```
define('WP_TEMP_DIR', ABSPATH.'wp-content/tmp');

define("FS_METHOD", "direct");

define("FS_CHMOD_DIR", 0777);

define("FS_CHMOD_FILE", 0777);
```

执行`mkdir /usr/local/nginx/html/wp-content/tmp`

执行`chmod -R 777 /usr/local/nginx/html/wp-content/tmp`