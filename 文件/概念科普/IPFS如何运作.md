标题|简介
| --- | --- |
IPFS如何运作|了解IPFS（星际文件系统）的运作方式和为何它是未来互联网时代必不可缺的

# IPFS如何运作

:::提示   
想要观看关于IPFS是如何运作和处理文件的视频回顾吗？可以点击查看2019年度IPFS营！[核心课程：IPFS如何处理文件](https://www.youtube.com/watch?v=Z5zNPwMDYGg)  
::: 

IPFS是一个点对点（peer-to-peer: p2p）储存网络。内容是从世界各地不同的节点上获取的，所以这有可能造成数据传输上的延误，不管是获取或者储存。因此，IPFS知道如何利用内容寻址技术来寻找你要的内容，而不是用数据储存的位置来进行定位寻找。  

我们需要了解IPFS三个基础的核心原理：
1. 利用内容寻址技术进行独特标识
2. 利用有向无环图（directed acyclic graphs: DAGs）链接内容
3. 利用分布式散列表（distributed hash tables: DHTs）探索内容

这些核心原理都是环环相扣的，运行着整个IPFS生态。首先，我们来了解什么是内容寻址技术和独特标识。

## 内容寻址技术

IPFS利用了内容寻址技术来辨别并且找到我们要的内容，而不是通过内容储存的位置来进行寻找。我们在日常生活中也有利用内容来寻找一样东西。举例，当我们在图书馆寻找一本书的时候，我们把书名告诉图书馆管理员，要求他帮我们找到那本书。这就是内容寻址的概念，你告诉管理员你要的是什么。如果今天你是用位置来寻找这本书，你应该要告诉管理员：我要的那本书坐落在第二层，第一个行规则，往下数第三格，从左数第四本。但是当有人移动过那本书，你就找不到了！

这个问题出现在了现金的互联网和电脑！目前，我们都是用位置来找寻内容的，例如：

- `https://en.wikipedia.org/wiki/Aardvark`
- `/Users/Alice/Documents/term_paper.doc`
- `C:\Users\Joe\My Documents\project_sprint_presentation.ppt`

相比之下，储存在IPFS上的内容都有自己的内容标识（content identifier）或者称之为CID，这就是哈希（hash）。所有的哈希都是基于内容产出的（虽然长度比真正的内容短），也是独一无二的。如果哈希对于你来说是一个全新的概念，可以看看我们关于密码学哈希运算的介绍。

许多分布式系统都会利用内容寻址技术（哈希形式），来定位寻找内容。但它不仅仅能进行内容识别，也能把它们都链接在一起-从支持代码的提交到加密货币运行的区块链，都使用了这个技术。然而，这些系统的特定底层数据结构并不一定要求可互操作或彼此兼容的。

这时候就轮到IPLD（Interplanetary Linked Data）派上用场了，我们称之为星际链接数据。IPLD在哈希链接的数据结构上来回的进行转换，达到分布式系统上数据的一致性。IPLD也提供了用于组合可插入模块的代码库（提供了各种IPLD节点的解析器），用于解析多个链接节点之间的路径，选择器和查询。因此，你可以在无需理会底层协议的情况下，探索数据。IPLD也提供了一种在内容可寻址数据结构之间进行转换的方法。例如，如果你是使用Git的，它可以遵循所有的链接，而如果你是使用以太坊的，它也可以遵循所有的链接！  

IPFS遵循特定的数据结构偏好和约定。IPFS协议使用这些约定和IPLD，以便从原始内容获得一个IPFS寻址。而这个IPFS寻址是用来在IPFS网络中，辨别和定位内容。  

下个篇章，我们会来探索DAG数据结构是如何把内容之间的链接镶嵌到内容寻址里头。




