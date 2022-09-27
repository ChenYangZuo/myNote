# 在华为云上部署WordPress II

修改时间：2021.04.04 11:38

使用环境：华为云ECS 1vCPUs|1GB|40G|1Mbps|CentOS 8.0  64bit with ARM

## 申请SSL

### 购买证书

进入华为云控制台，选择云证书管理服务，点击右上角**购买证书**

在购买页面，选择DV(Basic)-DigiCert，价格为0元，点击购买

### 申请证书

返回云证书管理服务，在已购买免费证书的操作列点击**申请证书**

在申请页面选择系统生成CSR，并输入域名

点击下一步，输入正确个人信息，提交申请

### 下载证书

若信息正确无误，证书将在15分钟后下发，发放后下载证书至本地

## 设置安全组规则

开启443端口访问

## 上传证书

本地解压SSL证书，进入Nginx文件夹，找到.crt文件与.key文件

在`/usr/local/nginx/`目录下新建cert文件夹，并将.crt文件与.key文件上传至此目录

## 修改Nginx配置文件

执行`vim /usr/local/nginx/conf/nginx.conf`

新增如下代码至原server上方：

```
server {  
             listen              443 ssl;  
             server_name         localhost;
             ssl_certificate     ../cert/cert.pem;  
             ssl_certificate_key ../cert/cert.key;
             ssl_session_cache   shared:SSL:1m;  
             ssl_session_timeout 5m;  
             ssl_ciphers         HIGH:!aNULL:!MD5;         
             ssl_prefer_server_ciphers  on;  
             location / {  
                        root     html; 
                        index    index.html index.htm;  
                         }  
}
```

并修改80端口的配置，实现http跳转https

```javascript
server {
    listen 80;
    server_name localhost;
    rewrite ^(.*) https://$server_name$1 permanent;
}
```

注意替换localhost为域名

执行`service nginx reload`加载配置

## 修改WordPress配置

进入WordPress后台，在设置-常规中修改连接的http为https

**关联文档：**
上一篇：[[WordPress·[1]在华为云上部署WordPress I]]
下一篇：[[WordPress·[3]在树莓派上部署WordPress]]