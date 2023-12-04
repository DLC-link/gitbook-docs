---
description: 一个通用的支持DLC的应用程序的示例
---

# DLC-Enabled Lender

### 组成部分

从高层次上来说，DLC.Link中最重要的技术系统架构如下:

* 比特币认证人，他们通过为DLC参与者签署结果来充当调解人([了解更多 DLCs](https://docs.dlc.link/applications/technical-application-designs/simple-case))
* DeFi协议使用的智能合约 (e.g. Solidity)
* DLC.Link 的管理协议
* 参与 DLC的钱包

作为一个实现的例子，考虑以太坊上的DeFi贷方，该贷方将稳定币贷款给原生链上比特币抵押方。

### 贷款流程示例

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

1. 建立一个新的抵押贷款, 调动DLC.Link智能合约，在比特币认证层上建立比特币DLC合约。
2. 来自比特币认证者的DLC的详细信息被转发给用户，用户可以使用支持DLC的比特币钱包查看和签名DLC。应用程序还使用DLC.Link协议的钱包服务签署他们一方的DLC协议。
3. 一旦签名，DLC交易被提交到比特币区块链。一旦DLC被确认，应用程序的智能合约就会得到通知。
4. 该应用程序的智能合约现在有比特币抵押品的担保，并将稳定币释放到用户的钱包中。

### Close Loan via Repayment还款结清贷款

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

Above a user wants to repay their stable-coin loan and regain access to their bitcoin collateral.&#x20;

1. The user navigates to the defi applications webpage and chooses to repay the loan.
2. The user initiates the sending of the stablecoin via the application's smart contract.
3. The application's smart contact communicates with the DLC.Link management smart contract to begin the closing of the DLC. The Bitcoin Attestors sign an outcome for the repayment (other possible outcome could have been liquidation).&#x20;
4. A callback to the application's smart contract notifies it that the DLC is ready to be closed, and the defi applications website is updated.
5. The application's protocol wallet puts the closing transaction on the BTC blockchain.
6. &#x20;The user can now see the funds in their BTC wallet, and the loan closing is complete.



上面，用户希望偿还他们的稳定币贷款并重新获得他们的比特币抵押品。

1. 用户导航到DeFi应用程序网页并选择偿还贷款。
2. 用户通过应用程序的智能合约发起稳定币的发送。
3. 应用程序的智能合约与DLC.Link智能合约通信开始关闭DLC。比特币证明人签署了一份还款结果(其他可能的结果可能是清算)。
4. 对应用程序的智能合约的回调函数通知它DLC已准备好关闭，并且DeFi应用程序网站已更新。
5. 应用程序的协议钱包将关闭交易放在BTC区块链上。
6. 用户现在可以看到他们的BTC钱包中的资金，贷款关闭完成。

### Close Loan via Liquidation清算结清贷款

当用户抵押品的价值低于某一阈值时，就会发生清算事件。在这里，清算人确定抵押不足的贷款，并开始偿还贷款资产的过程，并以折扣价“买断”剩余的抵押品，从而保持贷款资产的价值稳定。

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>
