# 技术架构

### 理论基础

比特币已经成为一种全球现象和安全的价值储存手段，不受政府干预和制度风险的影响。尽管比特币具有优势，但其安全措施限制了其可编程性，缺乏用于强大金融应用程序的智能合约接口。

以太坊和其他区块链以其强大的智能合约平台填补了这一空白，推动了巨大的增长，但也导致了投机和感知风险。在比特币社区，去中心化金融(DeFi)可以被视为一项高风险投资，与波动性和公众认知问题有关。

我们认为，比特币是理想的结算层，可以在其他平台上的分散金融系统中使用。使用比特币作为一种价值存储，可以保护跨各种区块链的复杂应用程序，从而降低风险，并符合去中心化的愿景。

[请在这里阅读更多关于我们的理念:https://www.dlc.link/blog/introducing-the-bitcoin-plus-mentality](https://www.dlc.link/blog/introducing-the-bitcoin-plus-mentality)​

## DLCs 如何实现这一愿景

DLCs为金融交易创造了一个安全和分散的框架。通过预先定义锁定比特币抵押品的可能结果，并在智能合约的区块链上运行业务逻辑，DLC显著减少了错误或恶意行为的可能性。

这种结构还确保了比特币支付和钱包的细节是保密的，符合隐私和不信任的原则。此外，以基于共识的抽象方式使用比特币证明增加了另一层安全性，利用分散安全的力量，同时保持系统的完整性和信任。

## 支持多链

目前我们支持以太坊和[Stacks](https://www.stacks.co/)区块链。其他EVM链将很快跟进，随后将添加Solana, Polkadot, Cosmos等链

### 启动新的区块链

启动一个新的区块链主要需要启动一个DLC-Link管理智能合约，并配置证明人来监听事件。添加新的EVM兼容链比较简单，但其他链可能需要几周的时间，然后是安全评估和测试阶段。由于我们的工具在原生比特币和智能链之间架起桥梁，并利用比特币钱包和分散的比特币证明，它可以与几乎任何处理金融交易的区块链集成。这种灵活性意味着对底层区块链的要求很低。

## DLC的组件

<figure><img src="../.gitbook/assets/DLC.Link_TechnicalFlow_latest.png" alt=""><figcaption></figcaption></figure>

### 一个标准的DLC流程

在这里，我们将描述用户与DLC系统的交互是如何呈现的，并遵循上图。

**设置DLC**

从图表的左上角开始，沿着蓝色的“开放DLC”线。

1. 用户与dApp的智能合约进行交互，例如在dApp中创建新的贷款或存款保险库。通常，用户会指出他们想要锁定多少比特币抵押品。
2. dApp智能合约调用DLC.Link智能合约上的open-dlc函数。
3. The DLC.Link 验证者拾取事件，并发布DLC公告。
4. 一旦交易完成，dApp合约可以乐观地假设该公告存在于证明方。然后，它可以向用户显示一个链接，将比特币锁定到DLC中。
5. 用户点击一个标签类似于“锁定比特币”的按钮。这将触发支持DLC的钱包显示DLC的详细信息，并在批准后广播比特币交易以将BTC锁定到DLC中。
6.
   1. &#x20;在后台，DLC流程有几个步骤。第一步是网站从dApp的比特币钱包请求DLC Offer消息，并将其传递给用户的比特币钱包。然后，钱包自动通过DLC流的接受和签名步骤。
   2. 最后，用户查看详细信息，并选择接受。只有这样，比特币的交易才会广播并将比特币锁定到DLC中。
   3. CETs代表了DLC中比特币抵押品的可能结果集，类似于部分签名的比特币交易，加密存储在安全存储系统中。它们将在关闭流程中再次使用。
7. 当比特币网络达到6次确认时，一条消息被发送到dApp的智能合约，表明比特币抵押品被安全锁定在DLC中，保险库或贷款现在应该是活跃的。这条指示DLC被资助的消息是dApp的比特币钱包应用程序的责任，这是DLC. link的自定义应用程序。这是因为不错误地发放贷款符合dApp的利益。

DLC现在已经建立，比特币被锁定，抵押品在第二个区块链上使用(ETH或者其他区块链)。

**关闭DLC**

从图的左上角开始，沿着紫色的“开放DLC”线。

1. 在智能区块链上发生事件，例如偿还贷款或以其他方式触发保险库的关闭流。这将调用DLC.Link智能合约上的close-dlc函数。
2. 这是由认证方接收的，并发布DLC认证签名。
3. 比特币钱包(用户的或dApp的)将定期检查证明，并查看证明是否已发布。该钱包使用该证明来签署获胜的CET，并将其广播到BTC网络，然后关闭DLC并移出BTC。

既然我们已经描述了整个DLC流程，我们可以单独讨论每个技术部分。

### DLC 证明人

DLC.Link 架构中最重要的部分是DLC认证层。在某些方面类似于跨链桥上的“验证器网络”，DLC证明分两个步骤工作:

1. 当用户表示他们想要锁定比特币时，DLC证明人会创建一个DLC公告。此事件由比特币钱包使用。
2. 之后，DLC证明者在以太坊或stack(或其他链)上监听有关DLC结果的相应事件，并签署相应的证明。

通过认证，参与者的比特币钱包能够签名原始结果集中的一个且仅一个CETs。因此，比特币UTXO被“移动”，或根据该CET分配到新的钱包。

比特币认证由运行DLC认证软件的独立节点组成。它们由独立的第三方节点运营商管理和运行，每个运营商可能只运行一个节点。它们共同构成了DLC.Link的比特币认证层。

除了为DLC结果创建公告和证明之外，证明程序还履行侦听功能，响应相应DLC.Link智能合约上的创建和支付事件。由于在证明程序中没有业务逻辑，而只是一个监听和签名过程，因此确保了谨慎性和安全性，并且通过验证链上比特币证明程序结果的反向检查实体，我们始终可以确保证明程序表现良好。 他的验证保证了证明人行为的准确性，并保证在过程中的任何一点都不会被操纵。 这是通过DLC.Link运行状况指示板可见的(即将推出)。

比特币证明者由各种独立的节点运营商运营，以分散的方式证明支付结果。它们的功能是抽象的，这意味着证明人不知道具体的结果或参与者，从而增强了安全性。多个证人之间的共识用于签名，利用分散的安全性。行为不端的证人会受到严厉的惩罚，保持完整诚信。

在这里阅读更多关于比特币见证者的信息:

[https://www.dlc.link/blog/what-is-a-bitcoin-attestor​](https://www.dlc.link/blog/what-is-a-bitcoin-attestor%E2%80%8B)

### 比特币钱包

A key component of the DLC.Link architecture is our inclusion in various BTC supporting wallets. DLCs are designed so that the smart contracts and Bitcoin Attestors know as little about the details of the bitcoin payments and wallets as possible. This is a great security feature, and means the wallets themselves need to do a good deal of DLC signing.

Two types of wallets are anticipated as being needed, one for end users, and one for application services that enterprises would run in an automated fashion.&#x20;

For either case, we have developed and built-upon DLC libraries that will provide this functionality.

#### JS Library for Browser Extensions and Mobile Wallets

For end-user wallets, that are often written in JS or ReactNative, we have a JS library that can easily be dropped into existing web/mobile wallets. We have also developed our own DLC-Enabled BTC wallet for testing purposes.

The DLC.Link signing library is publically available here: [https://www.npmjs.com/package/@dlc-link/dlc-tools](https://www.npmjs.com/package/@dlc-link/dlc-tools)

DLC signing is available in the Leather wallet. [https://leather.io/](https://leather.io/)

You can read more about the JS library and setting it up here:  [bitcoin-wallets.md](../architecture/installation-and-setup/bitcoin-wallets.md "mention")

#### Router Wallet Service

We have released a secure, reliable BTC/DLC signing application for financial institutions and dApps to automate their DLC and Bitcoin interactions. This tool is built in Rust and leverages a well-developed and featureful open-source DLC management project. While this service can sign Bitcoin DLC transactions, it does not have wallet functionality, and does not manage any funds directly.

### Smart Contracts

Smart contract integrations provide enhanced security by allowing both the attestor and the system to verify smart contract outputs directly on-chain. This ensures a transparent and trustworthy process. Our Bitcoin Attestors have been connected to blockchain DApps to leverage this benefit, merging the strength of Bitcoin with cutting-edge platforms for application development. The integration process is made even more attractive by its simplicity, requiring only the implementation of the "Open DLC" and "Close DLC" functions. These functions usually consist of fewer than 30 lines of code, making it an accessible solution that combines robust security with efficiency.
