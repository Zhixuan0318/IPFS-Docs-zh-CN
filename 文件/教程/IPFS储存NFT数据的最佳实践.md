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

如果你的内容本身就拥有版本0（v0）的CID，就不需要为了拥有新版本的CID格式而再添加到IPFS上！你可以使用IPFS的命令行或是在[cid.ipfs.io](https://cid.ipfs.io)，把你的v0 CID转换成v1。如果你不知道你目前的CID是什么版本，其实非常容易辨别。v0的CID拥有46个字符，并且以`Qm`作为开头。  

::: 提示  
你可以在ProtoSchool上学到更多CIDs的相关知识。  
:::  

当你成功把数据添加到IPFS上并且获得你的CID后，就可以开始准备NFT的元数据（metadata）然后在区块链上铸造你的NFT。你需要把你的CIDv1转换成IPFS URI（下方会有教程），以便能够从智能合约或者NFT本身的元数据链接到你的内容。  

### IPFS URI（统一资源标志符）

统一资源标志符（Uniform Resource Identifier: URI），在特定上下文中，指定一个特定的内容。这个上下文是由URI方案（scheme）来决定的（作为前缀附加到URI的前端，然后加上`://`）。IPFS的URI方案就是`ipfs`。

完整的IPFS URI例子：`ipfs://bafybeigdyrzt5sfp7udm7hu76uh7y26nf3efuylqabf3oclgtqy55fbzdi`  

IPFS URI是IPFS链接规范，因为`ipfs`方案清楚明确地表明，CID指向的是IPFS上的内容，而不是其他系统的。我们只需在CID字符串前端加上静态字符串`ipfs://`就能产出一个IPFS URI。  

你也可以把文件名融入到IPFS URI的路径。例如，如果你把NFT元数据放置在一个档案里面并且储存在IPFS，那么你的URI会像这样：`ipfs://bafybeibnsoufr2renqzsh347nrx54wcubt5lgkeivez63xvivplfwhtpym/metadata.json`

我们建议你使用IPFS URI，让你的智能合约和储存在IPFS上的数据互相链接。这包括了用于描述NFT的元数据。

IPFS URI也适合让元数据和照片或是任何储存在IPFS上的数字资产互相链接。阅读下方的元数据篇章以获得更多相关资讯。  

### HTTP Gateway URL（HTTP网关统一资源定位符） 

HTTP网关为无法本地解析IPFS URI的旧版用户代理提供了互操作性。  

举个例子：`https://dweb.link/ipfs/bafybeigdyrzt5sfp7udm7hu76uh7y26nf3efuylqabf3oclgtqy55fbzdi` 

支持IPFS的用户代理（不管是通过IPFS Companion的浏览器扩展，又或者是平台本身就支持，像是Brave）都能识别这些网关链接，并且使用本机IPFS协议解析内容。其他用户代理将跟随通往网关的链接，通过IPFS加载内容并使用HTTP传送。你可以在我们的IPFS网关概念科普篇章学习更多有关HTTP网关的知识。  

网关链接非常适合拥有互操作性质，但它不能作为你IPFS上的数据的主要或规范链接。虽然只要IPFS上的任何人拥有你想要的数据，IPFS URI就仍然可以访问，但如果网关运营商处于离线状态，那么网关链接可能就会失败。  

如果使用了网关链接，那么开发者就要确保网关的URL规格是正确的。以下的URL结构都是可以被接受的：  

`https://<gateway-host>.tld/ipfs/<cid>/path/to/subresource`

`https://<cidv1b32>.ipfs.<gateway-host>.tld/path/to/subresource`  

如果程序是针对用户而开发的，开发者应该同时使用两个方法链接到IPFS：  

- IPFS URI（统一资源标志符） 
- HTTP Gateway URL（HTTP网关统一资源定位符）  

上述做法无疑将会提供最佳的用户体验，直到更多浏览器支持IPFS URI方案的本机解析。值得一提，这两种网关链接都可以基于CID或IPFS URI轻松生成。  

### Metadata（元数据） 







