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

大多的NFT都需要结构性的元数据来描述其基本属性。虽然我们在编写元数据时，拥有不同种类的编码和数据格式的选择，但是最标准的格式是把元数据储存成JSON，而编码为UTF-8字节字符串。  

以下例子是JSON格式的NFT元数据：  

```json
{
  "name": "No time to explain!",
  "description": "I said there was no time to explain, and I stand by that.",
  "image": "ipfs://bafybeict2kq6gt4ikgulypt7h7nwj4hmfi2kevrqvnx2osibfulyy5x3hu/no-time-to-explain.jpeg"
}
```  

我们拥有很多不同的方式来构造我们的NFT元数据，也有许多细节是取决于NFT平台的特别使用案例。上述的例子是使用了ERC-721标准中定义的格式。  

一般来说，直接采用或者加以扩展现有标准的格式，像是ERC-721和ERC-1155是个不错的想法，因为你的NFT都能够在标准的钱包或者其他工具（像是区块浏览器）上被显示出来。  

如果你想要链接你的图像，影片，或者是其他的媒体，那就使用IPFS URI。这比起使用HTTP网关URL来得更好，因为它并不是和一个指定的网关运营商绑定。但是如果你为了方便性和互操作性而想要选择使用网关URL，那么你可以直到在应用层（application's presentation layer）才进行生成。   

::: 提示   
在元数据中使用IPFS URI链接到图像和其他媒体有助于保持NFT数据的完整性！IPFS链接在创建后不能被篡改或更新并且指向不同的数据。   

即使至今你还没使用IPFS来保存你的数据，但你可以为你的数据生成一个IPFS URI并且放入其元数据中。这允许其他人在从别的源头下载后，验证其数据的完整性。如果你（或者其他人）把数据上载到IPFS网络上，其URI就能直接发挥功用了。  
:::  

因为你需要知道你想要在元数据中引用的图像（或是其他媒体）的CID，最简单的方式就是在把媒体资产上载到IPFS后，创建其元数据。  

### 利用IPFS目录以保留文件名  

当你想要把数据添加到IPFS时，你可以把文件包含在IPFS目录里来保留一个“人类能读懂”的文件名。  

在Javascript里，你可以在呼叫`ipfs.add`时选择`wrapWithDirectory`的选项：  

```js
const cid = await ipfs.add(
  { path: 'metadata.json', content: aJsonString }, 
  { wrapWithDirectory: true }
)
```   

当你添加包含在目录中的文件时，`ipfs.add` 返回目录对象（object）的CID。接着，你可以在CID后添加一个 `/` 字符，再添加你的文件名。这样，你就已经构建了一个完整的IPFS URI，用于前往你的文件。例如：`ipfs://bafybeibnsoufr2renqzsh347nrx54wcubt5lgkeivez63xvivplfwhtpym/metadata.json`。 

### 持久性和可用性  

当你把数据存储在IPFS上时，用户可以从任何具有副本存档的IPFS节点获取它。这可以进一步提高数据传输的效率并大大减少任何单个服务器上的负载。当每个用户获取数据时，他们会保留一份本地副本，当稍后有其它的用户请求同样的数据时，就能直接从这些节点获取。但有一点需要注意的是，这些数据只是暂时性的被保留下来，除非用户决定把数据“锁定”（pin）。当一个CID被锁定时，代表了我们告诉IPFS这个数据非常之重要，并且不应该被移除（当硬盘的容量接近极限）。  

当你开发了一个平台，并且以IPFS作为数据储存（storage），就应该把这些数据锁定在一些强大且拥有高度可用性的IPFS节点（没有大量停机时间和能在良好性能的情况下运行的）。想了解更多，可以读一读关于服务器基础架构的技术文档，了解IPFS集群（ipfs cluster）如何帮助你管理自己的IPFS节点，这些节点可以自行协调，锁定你的平台数据，并且能有效将其提供给你的平台用户。

或者，你可以选择使用远程锁定服务。像是[Pinata](https://pinata.cloud)和[Eternum](https://www.eternum.io/)就能为你的IPFS数据提供多余，高可用性的存储，无需任何 _供应商锁定_ 。由于IPFS的内容是通过CID来进行内容锁定的而不是位置，因此随着平台的增长，你可以在这些远程锁定服务之间更换或迁移到你的私人基础设施。  

你也可以使用由[Protocol Labs](https://protocol.ai)所推出的[nft.storage](https://nft.storage)，把数据储存到IPFS（由去中心化的储存网络Filecoin所提供的长期持久性）。为了促进 NFT生态系统的发展并保护NFT这全新的文物数字工地，[nft.storage](https://nft.storage) 为公共NFT数据提供了免费存储和带宽。赶快在 [https://nft.storage](https://nft.storage) 注册一个免费帐户！ 

想要学习更多关于持久性和锁定，包括了远程锁定服务的运作方式，可以阅读关于持久性，永久性和锁定的概述篇章。  

如果你想要寻找关于使用远程锁定服务来处理NFT数据的应用程序例子，可以阅读这篇关于使用IPFS铸造NFT的教程。






