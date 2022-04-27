标题|简介
|---|---|
使用IPFS铸造NFT|非同质化代币（NFT）允许用户创造和交易不同价值的数字资产。从零开始学习如何铸造一个NFT并且把它储存在IPFS，并且学习如何使用Pinata的远程锁定服务。  

# 使用IPSFS铸造NFT  

非同质化代币（NFT）允许用户创造和交易不同价值的数字资产。从零开始学习如何铸造一个NFT并且在远程锁定服务，像是Pinata和nft.storage的帮助下把它储存在IPFS。  

由于IPFS并不是一个区块链，我们会在这个教程使用以太坊。但是不仅仅是以太坊，我们也能把本次的教程轻松套用在别的区块链上。  

## NFT简介  

本次教程不会深入讲解NFT或者讨论其重要性和潜能。本次教程仅仅是帮助你学习如何使用IPFS托管你的NFT，并且了解这个过程是如何扩展至区块链开发的其它方面。  

因此，我们会尝试概况最基本的NFT知识，以确保大家是在同一个频道上。如果你想学习更多关于创造NFT的最佳实践和开发过程，可以前往[NFT School](https://nftschool.dev)。  

### NFT是如何组成的   

不论是什么平台，NFT拥有着几个关键特征。  

首先，每个非同质化代币拥有自己独一无二的编号，用于在其他代币之间区别自己。这跟同质化代币有非常鲜明的对比，像是以太 `ETH`，以一个数量存在于户口或者钱包。我们不能区别每个`ETH`。正因为NFT是独一无二的，它们是被独立交易和独立拥有的，而智能合约负责追踪是谁拥有这些NFT。  

另一个NFT的关键特征是它可以于储存在智能合约外的数据做链接。储存并且处理智能合约外的数据我们称之为 _链下_（off-chain）。这是因为凡是储存在链上的数据都需要被处理，验证，和广播到整个区块链网络，导致储存巨大的数据是非常昂贵的。NFT的应用就存在了这个问题，尤其是当NFT代表着一些艺术画作和收藏品，那么保存它们可以花费上百万美金。  

### IPFS如何解决问题  

当创建NFT并将其链接到存在于其他系统上的数字资产时，那么链接数据的方式就非常的关键。有几个原因导致了传统的HTTP链接并不适合应用。  

例如像这样的HTTP地址 `https://cloud-bucket.provider.com/my-nft.jpeg`，只要服务器的主人支付账单，任何人都可以获取 `my-nft.jpeg` 的内容。但是，我们永远无法保证 `my-nft.jpeg` 的内容与创建NFT时的内容是相同的。那是因为，服务器的主人可以随时将 `my-nft.jpeg` 替换为完全不同的图像，从而导致NFT失去了原有所代表的含义。  

这种称为“跑路”的事件也曾经发生过。一名画家创建了自己的NFTs并且卖出后，就把原本的图像进行替换，再把原图卖给其他人。  

IPFS通过内容寻址技术解决了这个问题。当你把数据保存到IPFS，IPFS会基于你的数据内容生成一个内容标识（content identifier: CID），而内容标识是链接到你刚上传到IPFS的数据。由于每个CID只能指向一个内容，并且是独一无二的，因此我们也不必害怕内容被替换或者篡改。  

当你使用CID，即使原著消失了，只要IPFS网络上存在着至少一份你想要的数据，你都能成功获取这份数据。这使得CIDs非常适合用于储存NFT。我们需要做的就只是把CID输入在 `ipfs://` 的统一资源标志符（URI）：`ipfs://bafybeidlkqhddsjrdue7y3dy27pu5d7ydyemcls4z24szlyik3we7vqvam/nft-image.png`。这样，就拥有了一个从区块链至代币数据之间无法被破坏的链接了。  

当然，在某些情况之下，我们会想要再发布了NFT后更新其元数据。这完全办得到！你只需要在智能合约中添加一个机制，那就是在每次更新代币的数据后自动更新URI。这将让你旧的IPFS URI更改为新的URI，同时初始版本的记录仍然在区块链的交易历史中保留下来。这增加了透明度，并让每个人都清楚知道更改了什么内容、何时以及由谁更改。   

## Minty  

为了更方便讲解NFTs和IPFS是如何互相配合和运作的，我们开发了Minty。Minty是一个简单的命令行应用程序，能够自动铸造NFT并利用[Estuary](https://estuary.tech/)，[nft.storage](https://nft.storage)，或[Pinata](https://pinata.cloud)把其数据在IPFS进行锁定。  

NFT平台的运作是较为复杂的。在当今的网络应用程序，我们需要围绕在我们选择的技术栈上做不同的决定，用户界面的规格，API的社交等等。链接区块链的去中心化应用程序（d-apps）还需要和用户钱包，例如[Metamask](https://metamask.io)互动，大大增加了开发的复杂程度。  

但既然Minty的出现是要演示使用IPFS来创建NFT的概念和过程，因此我们不需要专注于现代去中心化应用程序的开发。Minty就只是一个使用Javascript编写的命令行应用程序。  

### 下载Minty  

让我们一起下载Minty，开始动手创建NFTs！首先，你先需要下载NPM，才能下载并且运行Minty。目前Windows系统是不支持的。下载Minty的方法非常简单。你只需要下载其Github的仓库，再下载NPM dependencies，就能开始开启本机的测试环境。　　

1. 复制Minty的仓库，并且进入命名为 `minty` 的目录：　

   ```shell
   git clone https://github.com/yusefnapora/minty
   cd minty
   ```

2. 下载NPM dependencies：

    ```shell
    npm install
    ```

3. 添加 `minty` 命令到 `$PATH`。这并不是必须做的步骤，但能让你在电脑的不同角落都能运行Minty：

    ```shell
    npm link
    ```

4. 运行 `start-local-environment.sh` 脚本，以便能开始运行以太坊的本机测试网和IPFS daemon：

    ```shell
    ./start-local-environment.sh
    > Compiling smart contract
    > Compiling 16 files with 0.7.3
    > ...
    ```

    这个指令会不断运行下去。接下来更多的命令都需要输入到新的终端窗口。　
    
### 部署智能合约

在运行任何其他的 `minty` 命令之前，您需要部署一个智能合约：　　

```shell
minty deploy
> deploying contract for token Julep (JLP) to network "localhost"...
> deployed contract for token Julep (JLP) to 0x5FbDB2315678afecb367f032d93F642f64180aa3 (network: localhost)
> Writing deployment info to minty-deployment.json
```

这将部署到 `hardhat.config.js` 中配置的网络，默认情况下设置为 `localhost` 网络。如果您收到有关无法访问网络的报错，请确保你使用 `./start-local-environment.sh` 启动了本机开发网络。　　

部署此合约时，部署的地址和其他信息将写入`minty-deployment.json`。这个文件必须存在，才能使后续命令正常运行。
