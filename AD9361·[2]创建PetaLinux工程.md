# 创建PetaLinux工程

系统环境：Ubuntu 18.04.6

## 操作步骤

### 导入环境变量

```shell
source /opt/petaLinux/settings.sh
```

### 创建PetaLinux工程

```shell
cd /opt/petaLinux_projects
petalinux-create -t project --template zynq -n E310-AD9361
```

将VIVADO工程文件复制到E310-AD9361文件夹中

使用`petalinux-config`命令将包含xsa文件的文件夹导入

```shell
petalinux-config --get-hw-description=./AD9361_ZYNQ7020_E310/zed
```

### 配置PetaLinux工程