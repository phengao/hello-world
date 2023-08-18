# 基于BoAT-SupportLayer实现STM32存证DEMO适配说明


## 一、前言

本文档说明如何在 ***KEIL*** 开发环境中使用 ***BoAT-SupportLayer*** 中签名相关代码编译静态库及生成存证应用DEMO。

***KEIL*** 版本: ÌVision V5.36.0.0

STM32开发板: NUCLEO-H563ZI

STM32芯片型号: ES32H563ZIT6

## 二、代码下载和配置

### 1. 下载 STM32Cube_FW_H5_V1.1.0  
下载地址：https://www.st.com/en/embedded-software/stm32cubeh5.html  
约定`<STM32Cube>`是 STM32Cube_FW_H5_V1.1.0 的根目录。  
完成下载后目录结构如下：  
```
<STM32Cube>
|
+-- _htmresc/  
+-- Documentations/
+-- config.py/
+-- Drivers/
+-- Middlewares/
+-- Projects/
+-- Utilities/
`-- package.xml
`-- Package_license.html
`-- Package_license.md
`-- Release_Notes.html
```
### 2. 下载 BoAT-SupportLayer 

通过`git clone`命令，在`<STM32Cube>`目录下下载`BoAT-SupportLayer`仓库，命令参考如下:

```
git clone -b dev git@github.com:aitos-io/BoAT-SupportLayer.git
```
或
```
git clone -b dev https://github.com/aitos-io/BoAT-SupportLayer.git
```
完成下载后目录结构如下：  
```
<STM32Cube>
|
+-- _htmresc/  
+-- BoAT-SupportLayer/
+-- Documentations/
+-- config.py/
+-- Drivers/
+-- Middlewares/
+-- Projects/
+-- Utilities/
`-- package.xml
`-- Package_license.html
`-- Package_license.md
`-- Release_Notes.html
```

### 3. 构建 KEIL 工程

STM32 MCU 应用通过构建 KEIL 工程进行开发。  

#### 1. 构建 Keil 工程的方法

1. 可通过MX构建应用程序KEIL工程
MX是st公司构建STM32Cube应用程序的工具，可参考MX用户手册，构建基于NUCLEO-H563ZI开发板的应用程序工程。  
下载地址：https://www.st.com/zh/development-tools/stm32cubemx.html  
	
2. 可通过复制<STM32Cube>中的例程构建应用程序KEIL工程。
本例采用复制<STM32Cube>中的例程的方式构建存证DEMO的 keil 工程。  
本例将构建两个应用工程，分别用来实现boatlib静态库创建和存证demo实现。  

#### 2. 构建 BoAT-SupportLayer 静态库工程

BoAT-SupportLayer 静态库工程，其作用是将 BoAT-SupportLayer 相关代码编译成 KEIL支持的 .lib  静态库，供应用程序使用，应用程序在其工程中只需要添加静态库和头文件，不用增加具体的各个文件，可减少构建应用工程工作量。  

1. 创建 BoAT-SupportLayer 静态库KEIL工程 boatlib  
本例选择 STM32Cube_FW_H5_V1.1.0 中 NUCLEO-H563ZI 的 Nx_Iperf 示例工程作为基础，其路径为：  
`<STM32Cube>\Projects\NUCLEO-H563ZI\Applications\NetXDuo\Nx_Iperf`  
将其复制为 boatlib ， 路径为：  
`<STM32Cube>\Projects\NUCLEO-H563ZI\Applications\NetXDuo\boatlib`  

2. 删除工程中原有的 .c 文件，并添加 BoAT-SupportLayer 中所使用的 .c 和 .h 文件  
注意保留 BoAT-SupportLayer 中所引用的 第三方库头文件。 threadx、netxduo、rng等  

3. 将 BoAT-SupportLayer 中使用到的 .c 文件添加到 boatlib 静态工程中  

打开keil ide，加载 boatlib 工程，在 'Project'->'Manage'->'Project Items' 中增加 BoAT-SupportLayer 相关 .c 文件  
首先添加一个 group ，在这个 group 中添加以下文件：  
```
<STM32Cube>\BoAT-SupportLayer\BoAT-SupportLayer\third-party\crypto\rand.c
<STM32Cube>\BoAT-SupportLayer\BoAT-SupportLayer\platform\azure-rtos\src\osal\BoAT-SupportLayerosalcommon.c
<STM32Cube>\BoAT-SupportLayer\BoAT-SupportLayer\common\utilities\BoAT-SupportLayerutility.c
<STM32Cube>\BoAT-SupportLayer\BoAT-SupportLayer\common\utilities\base64.c
<STM32Cube>\BoAT-SupportLayer\BoAT-SupportLayer\platform\azure-rtos\src\osal\BoAT-SupportLayertask.c
<STM32Cube>\BoAT-SupportLayer\BoAT-SupportLayer\keystore\BoAT-SupportLayer_keystore_intf.c
<STM32Cube>\BoAT-SupportLayer\BoAT-SupportLayer\third-party\crypto\sha3.c
<STM32Cube>\BoAT-SupportLayer\BoAT-SupportLayer\keystore\soft\BoAT-SupportLayer_keystore_soft.c
<STM32Cube>\BoAT-SupportLayer\BoAT-SupportLayer\platform\azure-rtos\src\dal\BoAT-SupportLayerstorage.c
<STM32Cube>\BoAT-SupportLayer\BoAT-SupportLayer\third-party\crypto\aes\aes_modes.c
<STM32Cube>\BoAT-SupportLayer\BoAT-SupportLayer\third-party\crypto\aes\aeskey.c
<STM32Cube>\BoAT-SupportLayer\BoAT-SupportLayer\third-party\crypto\aes\aestab.c
<STM32Cube>\BoAT-SupportLayer\BoAT-SupportLayer\third-party\crypto\aes\aescrypt.c
<STM32Cube>\BoAT-SupportLayer\BoAT-SupportLayer\third-party\crypto\ecdsa.c
<STM32Cube>\BoAT-SupportLayer\BoAT-SupportLayer\third-party\crypto\nist256p1.c
<STM32Cube>\BoAT-SupportLayer\BoAT-SupportLayer\third-party\crypto\secp256k1.c
<STM32Cube>\BoAT-SupportLayer\BoAT-SupportLayer\third-party\crypto\sha2.c
<STM32Cube>\BoAT-SupportLayer\BoAT-SupportLayer\third-party\crypto\bignum.c
<STM32Cube>\BoAT-SupportLayer\BoAT-SupportLayer\third-party\crypto\rfc6979.c
<STM32Cube>\BoAT-SupportLayer\BoAT-SupportLayer\common\storage\persiststore.c
<STM32Cube>\BoAT-SupportLayer\BoAT-SupportLayer\third-party\crypto\hasher.c
<STM32Cube>\BoAT-SupportLayer\BoAT-SupportLayer\third-party\crypto\hmac_drbg.c
<STM32Cube>\BoAT-SupportLayer\BoAT-SupportLayer\third-party\crypto\memzero.c
<STM32Cube>\BoAT-SupportLayer\BoAT-SupportLayer\keypair\BoAT-SupportLayerkeypair.c
<STM32Cube>\BoAT-SupportLayer\BoAT-SupportLayer\third-party\cJSON\cJSON.c
<STM32Cube>\BoAT-SupportLayer\BoAT-SupportLayer\platform\azure-rtos\src\osal\BoAT-SupportLayerqueue.c
<STM32Cube>\BoAT-SupportLayer\BoAT-SupportLayer\platform\azure-rtos\src\dal\BoAT-SupportLayervirtualat.c
```

4. 将 BoAT-SupportLayer 中使用到的 .h 文件添加到 boatlib 静态工程中  

在'Project'->'Options for Target boatlib'->'C/C++(AC6)'->'Include Paths' 中添加 BoAT-SupportLayer 相关 .h 文件路径
需要添加的头文件路径如下：
```
<STM32Cube>/BoAT-SupportLayer
<STM32Cube>/BoAT-SupportLayer/BoAT-SupportLayer/third-party/crypto
<STM32Cube>/BoAT-SupportLayer/BoAT-SupportLayer/platform/azure-rtos/src/log
<STM32Cube>/BoAT-SupportLayer/BoAT-SupportLayer/platform/azure-rtos/src/osal
<STM32Cube>/BoAT-SupportLayer/BoAT-SupportLayer/include
<STM32Cube>/BoAT-SupportLayer/BoAT-SupportLayer/platform/include
<STM32Cube>/BoAT-SupportLayer/BoAT-SupportLayer/keystore/soft
<STM32Cube>/BoAT-SupportLayer/BoAT-SupportLayer/platform/azure-rtos/src/dal
<STM32Cube>/BoAT-SupportLayer/BoAT-SupportLayer/common/storage
<STM32Cube>/BoAT-SupportLayer/BoAT-SupportLayer/keystore
<STM32Cube>/BoAT-SupportLayer/BoAT-SupportLayer/third-party/crypto/aes
<STM32Cube>/BoAT-SupportLayer/BoAT-SupportLayer/third-party/cJSON
<STM32Cube>/BoAT-SupportLayer/car/include
<STM32Cube>/BoAT-SupportLayer/car/src
<STM32Cube>/BoAT-SupportLayer/car/inc
<STM32Cube>/BoAT-SupportLayer-SupportLayer/BoAT-SupportLayer/common/utilities</IncludePath>
```

5. 将 boatlib 工程输出配置为静态库

在'Project'->'Options for Target boatlib'->'Output' 中选择 'Create Library：'，编译完成后将在输出目录生成 boatlib.lib 静态库文件。


#### 3. 构建 STM32 存证应用程序KEIL工程 boatdemo

1. 选择正确的工程模板复制为基础KEIL工程。  
	本例中选择 STM32Cube_FW_H5_V1.1.0 中 NUCLEO-H563ZI 的 Nx_Iperf 示例工程作为基础，其路径为：  
		<STM32Cube>\Projects\NUCLEO-H563ZI\Applications\NetXDuo\Nx_Iperf  
	将其复制为 boatdemo ， 路径为：  
		<STM32Cube>\Projects\NUCLEO-H563ZI\Applications\NetXDuo\boatdemo  

2. 在boatdemo工程中添加所需的其他功能  

打开 boatdemo 工程中的 xxxx.ico 文件，参考MX操作，将 boatdemo 中使用的 RNG、MQTT 等功能加入到工程中。  

添加 boatlib 静态库和头文件。    
    添加 boatlib 静态库，需要先完成 boatlib 静态库的编译。
    添加头文件方式与'构建 BoAT-SupportLayer 静态库工程'中的方法一致。
	 
3. 取消 microlib 库使用选项  
打开keil ide，加载 boatlib 工程，'Project'->'Options for Target boatlib'->'Tartget' 中，取消'Use MicroLIB'项。
MicroLIB 是一个精简库，由于部分 BoAT-SupportLayer 使用的头文件和接口 API 不支持，因而不能使用，取消后，keil将使用 ARMCLANG 标准库。  
增加 nosemihosting.c 文件重写部分半主机接口函数，函数只提供接口不提供具体功能，在实际应用中不会调用这些接口，而是使用第三方库来实现对应的功能。nosemihosting.c 文件存在的意义是为编译过程中产生的错误实现半主机模式的接口函数。  
  
## 四 示例程序执行
	将 boatdemo 程序下载到nucleo开发版中，下载方法参考 keil 调试说明。
	复位开发板，程序运行后，可通过PC端的串口收发程序发送AT指令实现 STM32 WAKE 存证功能。
	
