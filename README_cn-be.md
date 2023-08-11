## BoAT-Engine

***BoAT-Engine*** 是 ***BOAT EDGE***产品中负责区块链相关应用的组件，是基于***BoAT Infra Arch*** 基础架构设计的跨平台区块链访问应用实例，是***Composable BoAT Core***层的组成部分。

***BoAT Infra Arch*** 基础架构，是一个跨平台应用框架，通过抽象不同平台的应用API，为BoAT应用提供统一的跨平台应用抽象接口，BoAT应用程序可以通过调用这些抽象接口快速实现在不同平台之间移植。

***BoAT Infra Arch**的详细介绍请参考：https://github.com/phengao/hello-world/blob/master/docs/BoAT_Overall_Design_cn.md

BoAT-Engine是在BoAT Infra Arch基础架构下基于BoAT Infra Arch基础架构BoAT-SupportLayer库现实的区块链应用实例，是具有多种区块链上链、链上数据查询、合约交互等应用功能的静态库。

应用程序通过引用BoAT-Engine静态库调用其API接口实现区块链操作功能。

BaAT Engine，提供chainmaker、ethereum、fiscobcos、hlfabric、platon、platone、venachain  多种区块链的区块链网络配置管理network、区块链协议访问protocol，钱包wallet访问等功能。
应用程序通过 BoAT-Engine提供的接口，参考demo示例程序能迅速结合应用需求完成区块链应用实现。

***BoAT-Engine***结合区块链应用的特性，将区块链应用统一划分为三个组成部分，分别是：区块链钱包、区块链协议、区块链网络。
区块链协议：
实现不同区块链协议的组包解包功能。

区块链网络
实现不同区块链网络信息管理和维护功能。

区块链钱包：
通过区块链协议对象和区块链网路对象创建区块链钱包。
实现钱包创建、钱包获取、钱包删除等钱包账号管理。
实现不同区块链的交易访问功能。包含区块链连接建立、区块链功能调用、合约访问等功能。

## 目录说明
### BoAT-Engine
```
<BoAT-Engine>
|
+---demo                  | Demo application
|   \---demo_chainmaker   |     network api of chainmaker
|   \---demo_cita         |     network api of cita
|   \---demo_ethereum     |     network api of ethereum
|   \---demo_fiscobcos    |     network api of fiscobcos
|   \---demo_hlfabric     |     network api of hlfabric
|   \---demo_hwbcs        |     network api of hwbcs
|   \---demo_platon       |     network api of platon
|   \---demo_platone      |     network api of platone
|   \---demo_quorum       |     network api of quorum
|   \---demo_venachain    |     network api of venachain
+---include               | Header files for application to include
+---network               | Configuration and management of blockchain networks
|   \---chainmaker        |     network api of chainmaker
|   \---cita              |     network api of cita
|   \---ethereum          |     network api of ethereum
|   \---fiscobcos         |     network api of fiscobcos
|   \---hlfabric          |     network api of hlfabric
|   \---hwbcs             |     network api of hwbcs
|   \---platon            |     network api of platon
|   \---platone           |     network api of platone
|   \---quorum            |     network api of quorum
|   \---venachain         |     network api of venachain
+---protocol              | Blockchain client protocol implementation
|   \---boatchainmaker_v1 |     protocol api of chainmaker
|   \---boatchainmaker_v2 |     protocol api of chainmaker
|   \---boatcita          |     protocol api of cita
|   \---boatethereum      |     protocol api of ethereum
|   \---boatfiscobcos     |     protocol api of fiscobcos
|   \---boathlfabric      |     protocol api of hlfabric
|   \---boathwbcs         |     protocol api of hwbcs
|   \---boatplaton        |     protocol api of platon
|   \---boatplatone       |     protocol api of platone
|   \---boatquorum        |     protocol api of quorum
|   \---boatvenachain     |     protocol api of venachain
|   +---common            |     Universal soft algorithms implementation for blockchains
|   |   \---web3intf      |     web3 api implementation
+---tests                 | Test cases
\---tools                 | Tools for generating C interface from contract ABI
\---wallet                | wallet API implementation for each blockchain
```

## BoAT-Engine的编译和使用方法
参考 https://github.com/aitos-io/BoAT-ProjectTemplate/blob/dev/README_cn.md
