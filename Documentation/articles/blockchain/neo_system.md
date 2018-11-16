<center><h2>Neo区块链系统</h2></center>

资产是Neo系统中的核心。交易、合约、账户和钱包这些的存在都是为了服务于资产的生成、流动和管理。我们把这种关系用下面这张图来描述了一下。

<p align="center"><img src="../../images/blockchain/system.jpg" /><br></p

区块链网络中，一切的事物操作都是通过交易完成，包括转账、调用合约和提取分红的GAS等。而合约又承担了交易中签名验证的任务。合约可以简单理解成比特币的Script脚本的升级。比特币的Script脚本因为不是图灵完备的，能做到的功能有限，完成交易的签名验证是可以的，所以比特币只有UTXO模型，关注的就是交易本身。编写Neo智能合约的语言，比如C#和Python等，都是图灵完备的，可以满足现实世界中的丰富多彩的需要。而现实世界广泛采用的是账户account机制。比特币采用UTXO模型，以太坊采用账户模型。在Neo中，UTXO模型和账户模型同时存在。UTXO模型主要用在全局资产的转账，账户模型主要用于提供智能合约支持。

Neo中的账户实际就是地址。这个地址可以是一个私钥对应的地址，用于UTXO，也可以是智能合约的地址，用于调用执行智能合约。私钥对应的地址实际就是私钥通过一系列加密算法运算最后求得的一个hash值，过程见下图。智能合约的地址是如果算得得呢？也请见下图。

<p align="center"><img src="../../images/blockchain/address.jpg" /><br></p

在Neo钱包中存放了各种资产，包括NEO、GAS和各种NEP5资产。存放的形式其实就是hash地址。比如下图： 

<p align="center"><img src="../../images/blockchain/account-gui.jpg" /><br></p

### **UTXO模型**

与账户模型不同的是，UTXO（Unspent Transaction Output）并不直接记录账户资产，而是通过未花费的`output`计算用户资产。每一笔UTXO类型的资产（如全局资产)，都是`input-output`关联模型，`input`指明了资金来源，`output`指明了所有资产的去向。如下图中，Alice 持有NEO分红得到8个GAS，记录在交易 #101 的第一位output上。当Alice转账给Bob时，新交易的`input`指向资金来源是交易#101的0号位置上的output所代表的资产——8个GAS，并在交易中一笔output指向给Bob 3个GAS，另外一笔output指向Alice 5个GAS(找零)。

<p align="center"><img src="../../images/blockchain/utxo.jpg" /><br></p

> [!WARNING]
> 1. 当有手续费交易时，input.GAS > output.GAS
> 2. 当持有NEO提取GAS分红时 input.GAS < output.GAS
> 3. 当发行资产时，input.Asset < output. Asset

UTXO进行转账时，实际上是对能解锁`Output.scriptHash`的output进行消费，并在新交易的见证人上填充其签名参数。账户地址，实际上就是脚本hash的base58check处理，代表的是一段签名认证脚本，如下图。 [`Op.CheckSig`](../neo_vm.md#checksig) 执行需要公钥和签名两个参数，在地址脚本中，已经包含了公钥参数，故在交易中只需要补充签名参数。

<p align="center"><img src="../../images/blockchain/account_scripthash.jpg" /><br></p

### **账户模型**

在智能合约部分详述。这里需要讨论一下与以太坊的区别。

### **合约Contract**

这里介绍的是具体概念。设计的细节请见smart contract部分。

### **资产Asset**

这里仅描述资产。具体数据结构请见asset部分。

### **NEP5资产**

这里介绍NEP5资产。与智能合约的关系。NEP5资产的具体设计请见nep5asset部分，而如何生成一个NEP5资产见transaction部分举例(例2)。


