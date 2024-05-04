# 在Windows平台使用XMake构建C++工程

## 安装工具链

### MinGW工具链



### XMake工具链

在Windows平台可采用PowerShell安装：

```powershell
Invoke-Expression (Invoke-Webrequest 'https://xmake.io/psget.text' -UseBasicParsing).Content
```

注意不要使用管理员权限运行

安装完成后输入`xmake --version`验证安装是否成功



## 配置VS Code环境

在VSCode中安装如下扩展插件：

1. C/C++：用于支持C++编程
2. clangd：C++语言服务器
3. XMake：XMake插件

### clangd

安装clangd后禁用默认的语言服务器并下载clangd二进制文件

clangd的代码补全基于一个叫做`compile_commands.json`的文件，我们需要修改其查找地址

在clangd的拓展设置中，添加额外参数：

```
--compile-commands-dir=${workspaceFolder}/build
```

在工程根目录下，创建`.clang-format`文件，配置clangd的参数：

```
ColumnLimit: 100
AccessModifierOffset: -4
```

其中`ColumnLimit`为强制换行的最大长度，`AccessModifierOffset`为public等关键字的缩进偏移量

### XMake

在XMake插件的拓展设置中，找到`Compile Commands Directory`设置项，将其设置为：`${workspaceFolder}/build`；找到`Run Mode`选项，设置为`buildRun`，确保点击运行时会构建最新的源码



## 创建工程

在VS Code中打开文件夹，在命令面板中输入`XMake`，选择`XMake:CreateProject`，选择语言为`c++`，工程类型为`console`

在窗口下方，选择toolchain为`mingw`

在命令面板中输入`XMake`，选择`XMake:UpdateIntellisense`生成`compile_commands.json`文件