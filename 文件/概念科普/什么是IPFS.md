标题|简介 
--- | --- |
什么是IPFS?|认识IPFS（星际文件系统）和它的运作方式，并了解它在未来的互联网时代的巨大作用和潜力。

# 什么是IPFS？

我们先来看看IPFS的含义：  
**IPFS是一个负责储存和查阅各种文件，网站，应用程序和数据的分布式系统。** 

这又是什么呢？假设今天你在做关于土豚的研究（别问为什么是土豚了，因为它们非常的酷！你知道土豚5分钟能挖深度达3尺的地洞吗？），你的第一步就会先访问维基百科：

```
https://en.wikipedia.org/wiki/Aardvark
```

当你在搜索引擎输入上方的URL并且进行搜索，你的电脑就会把你的请求发送到维基百科的其中一个伺服器（在地球上的某个角落），当找到了关于土豚的页面，就会再传回你的电脑。但这并不是唯一一个获得维基百科上的资料的方法。其实，维基百科拥有一个镜像拷贝（mirror）储存在IPFS，因此你可以选择使用IPFS来进行查阅。如果你是使用IPFS进行查阅，那么你的电脑发出的请求会是：

```
/ipfs/QmXoypizjW3WknFiJnKLwHCnL72vedxjQkDDP1mXWo6uco/wiki/Aardvark.html
```

:::提示
查阅上方链接最简单方式就是以IPFS的网关 _IPFS Gateway_ 在你的搜索引擎上进行搜索。你只需要在链接前端加上 `https://ipfs.io` 就能浏览到该[页面 →](https://ipfs.io/ipfs/QmXoypizjW3WknFiJnKLwHCnL72vedxjQkDDP1mXWo6uco/wiki/Aardvark.html)
:::

IPFS知道如何利用内容来寻找你要的资讯，而不是储存的位置（往下我们会提到，这就是所谓的内容可寻技术 content-addressing）。利用IPFS储存（IPFS化）的资讯将会由一串字符代表，填写在URL的中心 （`QmXo…`）。此外，我们并不是从维基百科其中一个伺服器中获得想要的页面，而是利用IPFS同时发送请求给全球多台电脑。哪些电脑储存着你想要的资讯就会传回给你，与你分享。从这里我们可以知道，我们可以从任何拥有土豚资讯的电脑获得那个页面，而不仅仅是依赖维基百科。

当你使用IPFS，你不只是从别人手上获得资讯，你的电脑也会帮忙传播这个资讯（存有副本）。假设你邻近的朋友也需要同样的维基百科页面，那么他拥有很大的几率从你这里获得那个资讯或者是其中一个使用IPFS的电脑。  

IPFS不仅仅是用来处理网页资讯，它也能用来处理任何电脑能储存的文件，不管是文档，电邮，甚至是数据库。

## 去中心化特质

我们能从非常多个节点获得我们想要的文件，而不是通过由一个组织或企业来管理的数据库：

- **建立一个“有弹性”的互联网世界。** 即使有一天维基百科遭到骇客的攻击或是开发人员的疏忽导致伺服器出现问题，我们依然能从别的途径获得同样的网页和资讯。
- **网络内容更难被审查并且封锁。** 由于IPFS使用了分布式的文件储存系统，无疑对想要封锁资讯的一方（国家，机构，或任何人）增加了许多难度。我们希望IPFS的出现能够提供更多方案来避免类似情况的发生。
- **高效的数据传输，即使处于偏远地带或离线状态。** 与其从千里之外获得我们想要的资讯或文件，如果我们能从身边附近的节点获得资讯或文件，那么相比之下，后者就快了许多。尤其是当自身处于的本地社群没有很好的互联网链接，但共同形成了互相链接的网络，那么IPFS就凸显出了它宝贵的价值。（通常拥有专业人才的企业都会设立多个数据中心或使用CDNs-内容分发网络 [Content Delivery Networks](https://en.wikipedia.org/wiki/Content_delivery_network)。但IPFS希望每个人都能做到。）

IPFS之所以命名为**星际文件系统**就是希望能建立一套能跨越星球的文件系统，不论多么遥远或是处于离线状态。这是一个非常美好和具有潜力的目标，而我们的团队也不断在努力，在这过程中创造的任何东西都有利于我们身处的家园。

## 内容寻址技术

:::呼吁  
想要知道为什么密码学里的哈希运算（cryptographic hashing）和内容寻址技术（content addressing）拥有强大的关联吗？可以参与ProtoSchool的教学-[Content Addressing on the Decentralized Web](https://proto.school/content-addressing)，建立相关的基础知识。  
:::

让我们回看上面提到的土豚网页链接，是不是觉得有点奇怪：

```
/ipfs/QmXoypizjW3WknFiJnKLwHCnL72vedxjQkDDP1mXWo6uco/wiki/Aardvark.html
```

这些在 `/ipfs/` 后端杂乱的字符串就是我们的内容标识（content indentifier），而这让IPFS能从多个节点找到吻合的资讯。

传统的URL或文件路径像这样。。。

- `https://en.wikipedia.org/wiki/Aardvark`
- `/Users/Alice/Documents/term_paper.doc`
- `C:\Users\Joe\My Documents\project_sprint_presentation.ppt`

。。。标识了文件的 _所在位置_ - 在哪部电脑，和在电脑的哪个硬盘。因此，对于同时储存在多个位置的文件（像是你邻居的电脑和外国朋友的电脑），这种方式是不可行的。

与其用 _位置_ 来锁定一个文件，IPFS利用了 _“这文件里头是什么”_ 或简单说 _文件内容_ 来锁定一个文件。因此，刚刚我们看到的内容标识，其实就是经过哈希运算后的内容。虽然这个内容标识和原本的内容相比变短了，但是它是基于文件内容所产出的，独一无二。这也让你能辨别传回来的文件或资讯是否是你想要的，或是否是正确的，避免了恶意节点传送错误的内容。

:::提升 笔记  
为什么我们都说“内容”而不是“文件”或“网页”呢？因为内容标识可以指向多种数据。例如，一个小文件，一个大文件，甚至是元数据（metadata）。（以防万一你不知道元数据是什么：元数据是“数据的数据”。例如，你查阅一个照片拍摄的地点，时间，和照片大小，这些都是元数据。）所以，IPFS的一个地址是可以指向一个文件，一整个档案，网页或不管什么类型的内容的元数据。如果想要了解更多相关的，可以读一读[IPFS是如何运作的](IPFS如何运作.md)。  
:::  

由于IPFS的文件地址是基于文件内容产出的，所以IPFS的文件链接是不能改变的。举个例子。。。  

- 如果一个网站的内容改变了，基于最新的内容，IPFS会产出一个新的链接，指向这个新的内容。
- 现今的互联网，一家公司会重新整理网站的内容，并且从 `http://mycompany.com/what_we_do` 搬迁到 `http://mycompany.com/services`。在IPFS，旧的链接还是会指向旧的内容，我们无法用旧的链接指向更新后的内容。

当然，人们都想要可以无时无刻更新或者更改内容，但不想要经过每次更新后，都需要再重新更换新的链接。在IPFS的领域里，这是完全可以办得到的，但是需要多一点的解释，也偏离了我们这篇对于IPFS的初步介绍。有兴趣了解更多的可以阅读IPNS（InterPlanetary Name System），MFS（Mutable File System）和DNSLink的概念科普，学习更多关于利用内容寻址技术的分布式系统如何适用于内容更新。  

值得一提的是，在那么多情况底下，IPFS是一个偏向于“参与性质”和“合作性质”的运作系统。举个例子，如果IPFS所有的节点都没有存有你指向的内容，那么抱歉，你无法获得你想要的内容。换个角度，如果IPFS上有任何一个节点始终储存着一份特定内容，那么那份内容就始终无法在IPFS上彻底被删除。这个节点可以是原创，也可以不是。这跟现在的互联网有相似之处，当一个内容被网上疯传并且拷贝和储存了，是非常难被删除的；不同的地方在于，IPFS上，这些内容的拷贝都可以被找出来。

## 参与度

IPFS的背后拥有非常多复杂的技术，但是当回到根本去探讨IPFS存在的意义，那就是改变由一群人和电脑所形成的巨大网络彼此之间的互动。现今的互联网是建立在“所有权”和“使用权”上，也就是用户能拿到想要的内容资讯或文件，但前提是原创或持有者允许。IPFS提倡“拥有”和“参与”。大家“拥有”彼此的文件，并且大家“参与”储存，确保内容存在，随时能够获得。  

这也代表了只有大家积极的参与, 才能充分发挥IPFS的潜能。假设你的电脑是IPFS其中的一个节点，用于分享文件。当你的电脑关闭后，其他人就无法从你这个节点获得他们想要的内容了。但是当你把内容储存在多个不同IPFS的节点，那么这个内容就更容易被想要获取的用户拿到了。这种情况在某种程度上是自动的：当你从IPFS下载文件到你的电脑，你的电脑会在特定时段里成为一个节点，其他人也能从你这获取那份文件。你也可以通过锁定（pinning）让你的内容在IPFS存在更长久一些。锁定是把内容储存在你的电脑，让它存在IPFS上，至到你决定不锁定（unpin）。你可以在这篇[持续性和锁定]了解更多相关知识。

在现今的互联网时代，如果你要确保你的文件永久性的存在于网络上，你需要使用付费性的云端硬盘，例如：Dropbox。而在IPFS的生态里，也有类似的服务，我们称之为锁定服务（pinning service）。但既然IPFS本身就拥有类似的分享机制，你也能跟朋友或者机构（例如：博物馆和图书馆）合作，并且互相分享文件。我们希望IPFS是一个拥有低门槛的工具，结合多元的社群，企业和组织的力量，共同打造一个可靠，强大和公平的（对比现金）分布式网络。




