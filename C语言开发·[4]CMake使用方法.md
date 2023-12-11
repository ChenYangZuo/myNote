# CMake使用教程

## 使用CMake的原因

### 使用GCC

命令冗长，容易出错，可维护性差。

### 使用make与Makefile

Makefile编写工作量大，解决依赖关系容易出错，Makefile通常依赖于当前的编译平台。

### 使用CMake

CMake恰好能解决上述问题，其允许开发者指定整个工程的编译流程，在根据编译平台，自动生成本地化的Makefile和工程文件，最后用户只需make编译即可，所以可以把CMake看成一款自动生成Makefile的工具。

## CMakeLists基本语法

### 注释

1. 使用`#`实现行注释
2. 使用`#[[]]`实现块注释

### 最小CMake版本

```cmake
cmake_minimum_required(VERSION 3.0)
```

上述代码指定执行该CMakeLists.txt的CMake最低版本为3.0。

### 定义工程

```cmake
project(T12_V1.2 C CXX ASM)
```

上述代码指定该工程名为`T12_V1.2`，工程支持C语言、C++与汇编。

### 定义变量

```cmake
set(CMAKE_C_COMPILER arm-none-eabi-gcc)
set(CMAKE_CXX_COMPILER arm-none-eabi-g++)
set(CMAKE_ASM_COMPILER  arm-none-eabi-gcc)
set(CMAKE_AR arm-none-eabi-ar)
set(CMAKE_OBJCOPY arm-none-eabi-objcopy)
set(CMAKE_OBJDUMP arm-none-eabi-objdump)
set(SIZE arm-none-eabi-size)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_C_STANDARD 11)
```

上述代码定义了一系列变量，其中`set`命令的原型为`set(VAR VALUE)`。在上述代码中，通过定义相关宏变量，首先指定当前工程所使用的ToolChain，其次指定当前工程使用C11/C++17标准。

### 定义头文件目录

```cmake
include_directories(Core/Drivers/U8G2 Core/Drivers/NST1001 Core/Drivers/EC11 Core/Drivers/24Cxx Core/Drivers/PID)
```

上述代码指定了头文件查找目录，不同目录之间可以用空格或分号分开。

### 定义源文件

```cmake
file(GLOB_RECURSE SOURCES "Core/*.*" "Middlewares/*.*" "Drivers/*.*")
```

上述代码通过`GLOB_RECURSE`递归搜索后边的指定目录，将搜索到的文件存入`SOURCE`变量。

### 定义生成可执行文件

```cmake
add_executable(app ${SOURCES})
```

上述代码将搜索到的源文件全部编译，最终生成名为app的可执行文件。

