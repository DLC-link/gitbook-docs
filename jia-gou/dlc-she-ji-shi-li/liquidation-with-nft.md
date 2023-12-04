# Liquidation with NFT

DLC.Link 设计了一种使用 ERC-721（NFT）或 ERC-1155（半同质化代币）代表锁定在 DLC 托管中的比特币的通证标准。这种类型的代币通常被称为**储备金证明**或**抵押品证明**通证。

此**抵押品证明**通证分别在令牌的mint和burn函数中实现了“DLC-open”和“DLC-close”功能。与封装比特币(wBTC)或其变体不同，代币本身不具有价值。相反，它提供了加密证据，证明资金被锁定在比特币链上。因此，这种代币比传统的封装比特币**安全**得多。

The token can be redeemed via a "peg-out" service provided by a Liquidator. Here's an example:

代币可以通过清算人提供的"peg-out" 服务进行兑换。这里有一个例子:

### NFT Peg-In and Peg-Out

**使用PoC代币设置贷款:**

* 借款用户通过DeFi应用程序启动贷款流程。
* 它们被定向到一个桥接应用程序，它可以是一个与Router一起工作的第三方清算器。
* DLC的细节已经建立，确定了比特币抵押品。
* 比特币被锁定在DLC中，抵押品证明通证被铸造。
* 然后，PoC代币被用作DeFi借贷应用程序中的抵押品，以借入稳定币。
* 稳定币贷款发放到用户的钱包中。

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

**Proof of Collateral Token:抵押品证明通证**

* PoC代币体现了“DLC-open”和“DLC-close”功能，提供了资金被锁定在比特币链上的加密证明。
* 与传统的封装比特币不同，它不持有价值，而是验证抵押品的存在。
* 它可以通过清算人(在这种情况下是第三方清算人)赎回。

**储备金证明的清算流程:**

* 如果用户默认，则可以转移或扣押NFT。
* 为了赎回比特币抵押品，由协议批准的第三方清算机构处理这一过程，Router作为交易对手方。
* 清算人发送付款以支付贷款，从DeFi协议接收PoC令牌。
* PoC令牌被烧掉，通知DLC.Link智能合约。
* DLC与相关的清算结果一起关闭，将BTC释放给清算人。
* 清算人结束清算过程，根据预先商定的条款进行支付。

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

这种方法可以增强清算过程中的灵活性、安全性和潜在的去中心化。Router和第三方清算器的使用增加了额外的功能和控制层，促进了与现有DeFi平台的无缝集成。

\
