标题|简介
| --- | --- |
使用IPFS储存NFT数据的最佳实践|非同质化代币（NFT）可以代表各种数字资产。学习使用IPFS储存NFT数据的最佳实践

# 使用IPFS储存NFT数据的最佳实践  
当需要储存或定位NFT（非同质化代币）数据时，IPFS自然成了不错的选择。这篇教程主要是讲解如何把NFT的数据保存在IPFS上，让NFT创作者和持有者有更好的体验。  

由于NFT在创建后是无法轻易更改的，我们需要考虑如何储存NFT的数据，提供寻址，并且随着时间的推移依然持久。因此，我们将详细介绍如何准备NFT的元数据。我们也将会探讨不同链接IPFS内容的方式，并且了解我们在什么场景下应该使用哪个。最后，我们也会探讨为什么制定计划来确保数据持久化对于追求良好的用户体验是非常重要的。只要遵循以下的建议，就可以确保你的NFT数据拥有一个长久且健康的未来。  

这个教程是专门给开发NFT平台或者其他工具的开发者，并且专注于解释如何格式化你的数据，并且进行链接，以达到最完美的长期效果。如果你想了解更多关于智能合约之间的互动或者如何铸造代币，可以读一读这篇如何利用IPFS铸造NFT（在以太坊测试网上的完整过程）。

如果你想更深入了解开发NFT的最佳实践或NFT整体开发过程，可以前往[NFT School](https://nftschool.dev/)学习更多概念科普和教程。  

## 不同的类型的链接和应用场景

我们拥有不同的方法来引用IPFS上的数据，不同的方法都有其最佳的应用场景。  

### CID（内容标识） 

CIDs能够独特标识一个内容。CID可以以紧凑的二进制形式在网络上存储和发送，但在前端时，它们会以随机的字符串来显示。举个例子：  

```
bafybeigdyrzt5sfp7udm7hu76uh7y26nf3efuylqabf3oclgtqy55fbzdi
```  

IPFS上使用着两种不同版本的CID。上方的例子是版本1的CID（或CIDv1），相比旧的“版本0”格式更具有一些优势，尤其是使用IPFS网关在搜索引擎上查看IPFS内容的时候。因此，在寻址定位NFT数据时，最好是使用以base32编码的版本1 CID。   

请在运行`ipfs add`的命令时添加`--cid-version=1`标志，以便在使用IPFS命令行时启用CIDv1：  

```shell
ipfs add --cid-version=1 ~/no-time-to-explain.jpeg
added bafkreigg4a4z7o5m5pwzcfyphodsbbdp5sdiu5bwibdw5wvq5t24qswula no-time-to-explain.jpeg
```  

如果使用Javascript, 你可以选择使用`ipfs.add`的方式： 

```javascript
const cid = await ipfs.add({ content }, {
  cidVersion: 1,
  hashAlg: 'sha2-256'
})
```  

如果你的内容本身就拥有版本0（v0）的CID，就不需要为了拥有新版本的CID格式而再添加到IPFS上！你可以使用IPFS的命令行或是在[cid.ipfs.io](https://cid.ipfs.io)，把你的v0 CID转换成v1。如果你不知道你的CID是什么版本的，其实非常容易辨别。v0的CID拥有46个字符，并且以`Qm`作为开头。





