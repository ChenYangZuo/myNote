# GitHub使用教程

修改时间：2021.04.17 22:02

## 迈出Git的第一步

在项目工程文件夹中右键选择**Git Bash Here**,在弹出的窗口键入`git init`

***这项操作会在此文件夹中建立版本库，.git目录默认隐藏不可见***

使用`git add *.*`添加想加入版本控制的文件

添加文件后使用`git commit -m  "xxxxx"`提交一次记录

## 推送到远程仓库

在GitHub上新建一个空仓库

使用`git remote add origin xxxxx`添加远程仓库

使用`git push -u origin main`将本地文件推送至远程仓库，第一次推送可加入`-u`参数使二者关联

## 从远程仓库拉取

### 克隆仓库

使用`git clone xxxxx`来获得远程仓库的完整拷贝

### 更新仓库

使用`git pull origin master:xxx`或`git pull`将远程仓库拉取并与本地分支合并

## SSL连接

Git支持多种协议，但通过ssh支持的原生git协议速度最快、最安全

使用`ssh-keygen -t rsa -C "your_email@youremail.com"`在本地创建一个密钥，文件存放在账户主目录下的.ssh文件夹中

复制**id_rsa.pub**中的内容并在GitHub上新建一个SSH Key时填入

使用`git remote set-url origin git@github.com:someaccount/someproject.git`更改与远程仓库的访问方式

这样就可以使用SSL连接，而不必每次输入账户与密码

## 删库跑路第一步

使用`git rm *.*`删除不想要的文件

删除文件后使用`git commit -m  "xxxxx"`提交一次记录

还可以使用`git push origin master`同时删除远程仓库上的文件

## 切换分支

使用`git branch test`新建一个test分支

使用`git checkout test`切换至新建的分支

也可以使用`git checkout -b test`来一步新建并切换分支

使用`git branch`来查看所有分支的情况

## 多人开发

### 情形1：

A用户建立了仓库，提交test.txt文件至本地及远程；B用户拉取了远程的仓库。此时所有仓库保持一致

此时A用户修改test.txt文件，并将其推送至远程仓库；B用户在没有拉取远程仓库变更时修改了本地test.txt，此时B用户使用`git push origin main`试图将代码推送至远程，git会提示冲突无法处理，并将冲突内容写入test.txt文件中

这时可以直接打开文件，修改至最终所需要的效果，保存并再次commit，之后继续进行push即可成功推送

### 情形2：

A用户建立了仓库，提交test.txt文件至本地及远程；B用户拉取了远程的仓库。此时所有仓库保持一致

此时A用户修改test.txt文件，并将其推送至远程仓库；B用户在没有拉取远程仓库变更时修改了本地test.txt，此时B用户想直接使用远程仓库覆盖本地仓库

可以使用如下命令：

```
git fetch --all
git reset --hard origin/master
git pull
```



## 代理设置

### 设置代理

使用如下代码将代理设置为1080端口

```
git config --global http.proxy socks5://127.0.0.1:10808
git config --global https.proxy socks5://127.0.0.1:10808
```

### 取消代理

使用如下代码取消代理设置

```
git config --global --unset http.proxy
git config --global --unset https.proxy
```

