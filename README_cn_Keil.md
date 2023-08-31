# 基于Azure RTOS编译BoATLite静态库适配说明


## 一、前言

本文档说明如何使用Keil工具链在 `Azure RTOS` 嵌入式系统中使用 `BoAT Infra Arch` 基础架构编译BoATLite静态库的过程。

本文的采用的编译操作系统是 windows 操作系统。

本文使用的工具软件包括：

**Keil**： 编译STM32应用的IDE，版本号为 `¦ÌVision V5.36.0.0`。

**STM32Cube**：STM32软件套装，版本为：STM32Cube_FW_H5_V1.1.0，Azure RTOS包含在Cube软件包中。

**Cygwin**：提供BoAT 静态库编译所需的操作命令。  

**git**：下载BoAT源码仓库的工具

本文中使用到 `BoAT Infra Arch` 基础架构的静态库包括：

**BoAT-SupportLayer** ： 为基于 BoAT Infra Arch 基础架构的应用程序和特定功能中间库提供平台操作系统调用接口抽象和驱动抽象应用接口，在本文中仅使用签名相关功能API接口。


## 二、构建编译环境

### 安装`Keil`:  
下载并安装Keil安装包，执行安装程序，按照提示完成Keil安装。
在本文中Keil安装在"D:\Keil_v5"目录中。  
安装成功后Keil的安装目录中包含了armclang编译应用程序，路径为"D:\Keil_v5\ARM\ARMCLANG"，使用<Keil root>代指该目录，后续在配置文件中会使用这一路径。  

### 下载STM32Cube软件包：  
下载地址：  
完成下载后，将STM32Cube软件包，解压缩到工作目录，本文中工作目录为"D:\work\STM32Cube_FW_H5_V1.1.0"，使用<Cube root>代指该目录

### 安装cygwin
具体安装步骤参考用户手册。[]()  
注意要将cygwin的工作目录配置到系统路径中。


## 三、BoAT3.1代码克隆和配置

可在任意自选目录中克隆BoAT3.1代码，本文选择在目录"D:\work"，使用<BoAT root>代指该目录.

### BoAT3.1源码编译目录构建

1、通过`git clone`命令，在`<BoAT root>`目录下 clone BoAT项目模板`BoAT-ProjectTemplate`仓库到本地，命令参考如下:

  ```
  git clone -b dev git@github.com:aitos-io/BoAT-ProjectTemplate.git
  ```
  或
  ```
  git clone -b dev https://github.com/aitos-io/BoAT-ProjectTemplate.git
  ```
  BoAT项目模板`BoAT-ProjectTemplate`仓库负责构建BoAT3.1项目的编译目录，详细说明参考[]()
  clone 完成后,在<BoAT root>目录下会创建`BoAT-ProjectTemplate`目录。
  
2、进入 `<BoAT root>/BoAT-ProjectTemplate/` 目录，检查 `BoATLibs.conf` 文件

确认 BoATLibs.conf 文件的内容为：
```
BoAT-SupportLayer
```
保存并退出。

3、在 `<BoAT root>/BoAT-ProjectTemplate/` 目录下执行配置脚本
```
python3 config.py
```
configy.py脚本是BoAT3.1配置交互脚本，通过输出提示信息，并接受输入选项完成BoAT3.1相关软件功能的配置。

具体配置过程如下：
```  
We will clone the BoAT-SupportLayer repository, which may take several minutes

Input the branch name or null:
``` 
输入‘dev’回车，选择BoAT-SupporLayer仓库dev分支。
``` 
Input the branch name or null:dev
branch name is [ -b dev]

git clone -b dev git@github.com:aitos-io/BoAT-SupportLayer.git

Cloning into 'BoAT-SupportLayer'...
remote: Enumerating objects: 2930, done.
remote: Counting objects: 100% (704/704), done.
remote: Compressing objects: 100% (327/327), done.
remote: Total 2930 (delta 441), reused 589 (delta 362), pack-reused 2226
Receiving objects: 100% (2930/2930), 3.40 MiB | 21.00 KiB/s, done.
Resolving deltas: 100% (1826/1826), done.
git cmd succ


overwrite the Makefile?(Y/n):
```
输入回车，选择重写Makefile
```
Yes

Select the platform list as below:
[1] linux-default             : Default linux platform
[2] Fibocom-L610              : Fibocom's LTE Cat.1 module
[3] Azure-RTOS
```
输入‘3’回车，选择“Azure-RTOS”平台
```
3
 
platform is : Azure-RTOS

include BoAT-SupportLayer.conf


./BoAT-SupportLayer/demo/ False
Configuration completed
```
配置完成。

完成上述操作后部分目录和文件结构如下：
```
<BoAT root>
|
`-- BoAT-ProjectTemplate
      |-- BoAT-SupportLayer
      |-- BoATLibs.conf
      |-- config.py
      |-- Makfile
      |-- README.md
      |-- README_en.md
```


## 四、文件修改

### 1、修改BoAT-SupportLayer中的Azure-RTOS平台的external.env

打开`<BoAT root>/BoAT-ProjectTemplate/BoAT-SupportLayer/platform/Azure-RTOS/external.env`文件
  
1. 配置本地实际的交叉编译器，例如：
```
CC := D:/Keil_v5/ARM/ARMCLANG/bin/armclang.exe
AR := D:/Keil_v5/ARM/ARMCLANG/bin/armar.exe
```
2. 增加BoAT3.1所需要引用的头文件路径
```
  EXTERNAL_INC := -I<Cube root>\Projects\NUCLEO-H563ZI\Applications\demo\boatlib\AZURE_RTOS\App \
                -I<Cube root>\Projects\NUCLEO-H563ZI\Applications\demo\boatlib\Core\Inc \
                -I<Cube root>\Middlewares\ST\threadx\common\inc \
                -I<Cube root>\Middlewares\ST\threadx\ports\cortex_m33\ac6\inc \
                -I<Cube root>\Drivers\STM32H5xx_HAL_Driver\Inc \
                -I<Cube root>\Drivers\CMSIS\Device\ST\STM32H5xx\Include \
                -I<Cube root>\Drivers\CMSIS\Include \
                -I<Cube root>\Projects\NUCLEO-H563ZI\Applications\demo\boatlib\FileX\App \
                -I<Cube root>\Middlewares\ST\filex\common\inc \
                -I<Cube root>\Middlewares\ST\filex\ports\generic\inc \
                -I<Cube root>\Projects\NUCLEO-H563ZI\Applications\demo\boatlib\NetXDuo\App \
                -I<Cube root>\Middlewares\ST\netxduo\common\inc \
                -I<Cube root>\Middlewares\ST\netxduo\ports\cortex_m33\ac5\inc \
                -I<Cube root>\Middlewares\ST\netxduo\common\drivers\ethernet \
                -I<Cube root>\Drivers\BSP\STM32H5xx_Nucleo \
                -I<Cube root>\Middlewares\ST\netxduo\addons\dhcp \
                -I<Cube root>\Middlewares\ST\netxduo\addons\mqtt \
                -I<Cube root>\Middlewares\ST\netxduo\addons\sntp \
                -I<Cube root>\Middlewares\ST\netxduo\addons\dns \
```
3. 增加芯片选型宏定义 `-DSTM32H563xx`
```
EXTERNAL_CFLAGS := -DHAVE_ARPA_INET_H -xc -std=c11 --target=arm-arm-none-eabi -mcpu=cortex-m33 -mfpu=fpv5-sp-d16 -mfloat-abi=hard -c \
-fno-rtti -funsigned-char -fshort-enums -fshort-wchar -DSTM32H563xx
```

### 2、修改 Makefile关闭不需要编译的内容

打开`<BoAT root>/BoAT-ProjectTemplate/BoAT-SupportLayer/common/Makefile`文件
屏蔽或删除 http2intf 和 rpc 编译项
```
all:
#	make -C http2intf all
#	make -C rpc all
	make -C storage all
	make -C utilities all


	
clean:
#	make -C http2intf clean
#	make -C rpc clean
	make -C storage clean
	make -C utilities clean
```
保存并退出

打开`<BoAT root>/BoAT-ProjectTemplate/BoAT-SupportLayer/third-party/Makefile`文件
屏蔽或删除 nghttp2 编译项
```
all:
	make -C cJSON all
	make -C protobuf-c/src all
	make -C protos all
	make -C rlp all
#   make -C nghttp2/src all


	
clean:
	make -C cJSON clean
	make -C protobuf-c/src clean
	make -C protos clean
	make -C rlp clean
#   make -C nghttp2/src clean
```
保存并退出

打开`<BoAT root>/BoAT-ProjectTemplate/BoAT-SupportLayer/platform/azure-rtos/src/Makefile`文件
修改
```
	$(AR) r $(LIBNAME) $(PORT_CRYPTO_OBJECTS) $(SRC_OBJECTS)
```
为
```
	$(AR) -r $(LIBNAME) $(PORT_CRYPTO_OBJECTS) $(SRC_OBJECTS)
```
保存并退出

## 四、BoAT静态库的编译和使用

### 编译BoAT静态库.lib文件
   
   
   在windows打开终端并进入`<BoAT root>/BoAT-ProjectTemplate`目录编译BoAT静态库：
   ```
   cd <BoAT root>/BoAT-ProjectTemplate
   make clean
   make all
   ```
   
   编译成功后，在`<BoAT root>/BoAT-ProjectTemplate/lib`下会生成两个静态库：
   ```
   libboatvendor.lib
   ```
   
   将静态库中引用的头文件打包在一个新建目录中，本文中将静态库引用的头文件复制到`<BoAT root>/BoAT-ProjectTemplate/lib/include`目录下，具体头文件根据应用选择复制。
   

### 在Keil项目中增加 BoAT 静态库引用
   
1. 将静态库添加到`Keil`项目中  
打开 Keil 项目，打开`Project->Manage->ProjectItems..`添加`BoAT`静态库引用。  
在`Groups:`栏中增加新的组，本文中添加`BoAT`组，在左侧`Files:`栏中，点击`Add Files...`，将前面编译好的`libboatvendor.lib`添加到`BoAT`组中。点击`Ok`，退出配置窗口。  
   
2. 将`BoAT`头文件添加到`Keil`项目中  
打开 Keil 项目，打开`Project->Options of Project 'xxxxx'...`，在`c/c++ (AC6)`栏`Include Path...`项中添加`BoAT`头文件路径，本文使用的`BoAT`头文件路径为`<BoAT root>/BoAT-ProjectTemplate/lib/include`目录。  
   
完成配置后，可在项目代码中引用BoAT提供的API接口函数。

  

