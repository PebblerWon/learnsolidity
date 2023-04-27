# 区块链分层

你可能已经听过 Layer1 Layer2 这样的术语，这是区块链在不可能三角限制下进行垂直方向上的扩展。

以比特币及以太坊为例，去中心化及安全性是比特币，以太坊的基石，正是这些才保证中立、抗审查、开放性等特性，但这在某种程度上牺牲一些可扩展性来换取而来，比如：比特币网络每秒可处理的交易不足 7 笔，以太坊每秒可处理的交易通常也只有几十笔。而Visa这样的电子支付网络每秒可以处理超过 20,000 笔交易。

开发人员试图以各种方式对区块链网络扩容，一个广泛被采用的方案是把一些计算放到链下进行（即链上叠加一层），链上只进行计算的校验和存储。

以下是一个分层架构图：

![](https://img.learnblockchain.cn/pics/20230209171636.png)

我们来分别介绍一下每一层。

## Layer0 

第 0 层，其实第0层的定义目前行业还没有完全一致的理解。

多数人认为第0层是 **加密数据连接层及其硬件**，对应上图下半部分。
也有一些人把跨链或可以创建链的基础设施为作为第0层，他们的代表有: [LayerZero](https://layerzero.network/)、[Substrate](https://substrate.io/) / [Polkadot](https://polkadot.network/) 及 [Cosmos](https://cosmos.network/)

## Layer1

第 1 层是我们常说的区块链，如比特币、以太坊、BSC 、Solana 等。 这些区块链在自己的区块链上根据共识处理并最终完成交易，

第 1 层区块链网络为开发dApps提供了基础架构，开发者可以在第1层网络上其他协议，比如我们看到MAKER DAO 稳定币协议、加密朋克 NFT 及 Uniswap DEX 协议等。

随着链上应用不断增长，网络“吞吐量”无法满足快速增加的需求，经常导致网络拥堵。增加区块链网络自身处理能力，一个常见的方法是扩大区块大小，以便在单个区块里可以容纳更多的交易，以太坊社区也确实多次提高过区块大小限制，但提高更快意味着更慢网络传播速度，以及更大数据意味着节点需要更大的存储容量，会提高节点参与网络的门槛，使得网络更中心化；另一个是方法是以太坊在尝试的分片（Sharding）扩容方案，将区块链数据分成不同的组（分片），每个分片负责网络活动中的不同交易子集。



## Layer2（二层网络）

Layer2 是针对底层区块链（Layer1）扩容的一种链下解决方案，Layer2 是一个独立的区块链，但使用第一层的安全性保证。

扩容主要思想是将原本 Layer1 的交易放在链下(Layer2)执行，减轻 Layer1 的负担，并且 Layer2 定期与Layer1通信，将Layer2的交易批量提交到 Layer1 。



### 比特币闪电网络


比特币上的 一个主要的 Layer2 扩容方案是**闪电网络（Lightning Network）**，为小额支付场景进行优化。闪电网络的主要是实现是，支付的双方在链下建立一个"通道"，双方可以在这个“通道”多次进行支付交易，在需要结算时，关闭通道即可。当支付的双方没有直接的"通道"可以借助第三方节点进行中转，如下路，A 要向 F 交易时，可借助 节点C 形成"通道"链路。所有节点一起就形成了一个支付网络。

![支付信道网状网络路由](https://img.learnblockchain.cn/pics/20230214173851.png)


与比特币链上交易相比，闪电网络有几个好处：
1. **更低的交易费用，对小额交易非常友好**，由于比特币链上交易需要用户之间相互竞价，比特币上一笔交易手续费通常在几美金，巅峰时期这需要几十美金，对于小额的交易，手续费往往比转移的金额还要多，闪电网络上通道费用是动态的，通常按转移的BTC数量的万分之几收取。
2. 在闪电网络协议下每秒可以发生的**支付数量没有基本限制**，仅受每个节点的容量和速度限制。
3. **更好的隐私**，闪电网络支付的细节不会公开记录在区块链上。闪电网络支付可以通过许多连续的通道进行路由，每个节点运营商都可以通过他们的通道看到支付，但如果不相邻，他们将无法看到这些资金的来源或目的地。



闪电网络也有一些**限制**：
闪电网络要求节点一直保持在线以充当支付通道，比较容易受到黑客攻击和盗窃；
多数用户可能会你来某个关键枢纽节点，这样的枢纽的离线可以迅速带来网络的集体（或完全）崩溃。



### Rollup

以太坊上主要的 Layer2 扩容方案是 Rollup，Rollup 意思是卷起，Rollup的核心思想是把**由Rollup层负责执行交易，然后许多笔交易压缩成一笔交易提交给以太坊**。



Rollup 目前分为两种类型：**Optimistic Rollup（乐观 Rollup）和 ZK Rollup（零知识证明 Rollup）**，他们的主要区别是如何将交易数据发布到第一层， Optimistic Rollup 是**乐观假设**从 Layer2 上执行的交易都是可信的，并批量提交到以太坊上，乐观 Rollup设置有一个挑战期（通常为一周左右），任何人发起挑战来验证交易的真实性，若挑战成功，原有交易被拒绝，并惩罚Layer2出块人。而 ZK Rollup 则是通过生成一个零知识证明来证明所有交易的有效性，相比Optimistic Rollup没有乐观假设，且有更高的数据压缩率，但为通用的计算生成零知识证明是一个难点，开发难度很大。

这是一个技术特性对比图：

![](https://img.learnblockchain.cn/pics/20230215143350.png)



目前乐观 Rollup 有 [Arbitrum](https://offchainlabs.com/) 、 [Optimism](https://optimism.io/) 、Boba network 等项目在运行，乐观 Rollup 可以实现 EVM 等效，已有在以太坊上的智能合约大部分不用做任何修改就可以直接部署到 这些 Rollup 上，



经过 1 年的发展，现在 Layer2 上的交易量已经赶超了以太坊主网上的交易量。

![image-20230215120513798](https://img.learnblockchain.cn/pics/20230215120521.png)

这是一张以太坊与 Arbitrum 、 Optimism 交易量的对比图。



目前 ZK Rollup 有 ZKSync、 StarkNet 等项目在运行，但都不具备 EVM 的等效性。在StarkNet上部署智能合约，开发人员需要学习Cairo 语言，是为STARK可验证程序构建的一种编程语言，ZKSync 目前主网的版本则仅支持转账及兑换操作。包括 [Scroll](https://scroll.io/)  和  [Polygon](https://polygon.technology/solutions/polygon-zkevm/) 在内的多个团队都在为实现 EVM 等效的 zkEVM 而努力，应该马上可以看到他们的主网上线。





### 侧链及其他

另一个和 Layer2 类似的二层扩容方案是侧链， 侧链和以太坊L2解决方案的主要区别是，**Layer2继承以太坊主网络的安全性，而侧链依赖于自己的安全性**。一个流行的侧链是Polygon ，他使用自己的PoS共识，有自己的验证者。但是 Polygon 会定期把交易的状态提交到以太坊。



在出现Rollup之前，状态通道、Plasma 等技术也是广泛讨论的扩容解决方案，目前采用度不高，有兴趣的读者可以自行研究。

下图是以太坊链下扩容技术方案图。





![](https://img.learnblockchain.cn/pics/20230215164740.png)



## Layer 3

Layer3（第 3 层）目前行业还没有一致认可的定义，Vitalik 在他的文章 [什么样的 Layer3 有意义](https://vitalik.ca/general/2022/09/17/layer_3.html) 里提出了对 Layer3 的 3 个愿景：

1. **L2 用于扩容，L3 用于自定义功能，例如隐私。**
2. **L2 用于通用扩容，L3 用于自定义扩容**， 
3. **L2 用于无信任扩展（rollups），L3 用于弱信任扩展（validiums）**。



还有一些人，将 Layer2 上的应用层，称为第 3 层，例如 Uniswap 、AAVE 、MarkerDAO 等。
