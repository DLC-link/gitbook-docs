# Ordinals in DLCs

## Ordinal Support

DLC.Link正在实现对锁定的ordinal作为DLC的抵押品的支持。

Notes:

* An ordinal is a unique numeric reference to a single satoshi, usually one that has had some content permanently inscribed onto it. However, that satoshi is often part of a larger UTXO. DLCs lock UTXOs, so please keep that context in mind.序数是对单个聪的唯一数字引用，通常是一个永久铭刻在其上的内容。然而，这个聪通常是一个更大的UTXO的一部分。DLCs 锁定UTXOs，请记住这点。
* 在所有情况下，锁定和随后解锁DLC的交易费用由双方分摊。

### Supported types of DLCs支持的种类

#### DLC with only the ordinal  只有ordinal

这种情况的用例是: 当DLC的提供方将包含序号的UTXO锁定到DLC中，而对手方(接受者)不锁定任何抵押品。

在这种情况下，结果基本上是二进制的，包含UTXO的序数要么传递给提供方，要么传递给接受方。

#### Ordinal +其它的抵押品

这种情况支持的用例是: 当DLC的发行方将包含ordinal的UTXO锁定在DLC中，并且将任何数量的附加抵押品锁定在DLC中。

1. In this case, the outcome of the UTXO containing the ordinal will go to either party A or B, without resizing (as described earlier) 在这种情况下，包含序数的UTXO的结果将发送给A方或B方，而不需要调整大小(如前所述)。
2. The remaining collateral locked by either, or both, parties can be split between the parties based on the standard DLC enumerated or numerical outcome guidelines.

### How it works工作原理

为了确保ordinal不会因为费用而丢失，包含序号的DLC交易是按照以下方式创建的：

* ordinal输入始终设置为资金交易中的第一个输入。
* 资金输出始终设置为资金交易中的第一个输出。
* 获得ordinal的一方将始终在 CET 输出中的第一个位置。

#### Locking an ordinal in the DLC 在DLC中锁定ordinal

\
要锁定一个ordinal，必须使用`OrdDescriptor`。该描述符包含有关包含在DLC中的序号的信息（在区块链中的位置和包含它的交易），以及DLC所基于的事件的信息。

事件的结果可以是enumerated or numerical，与常规合约类似。

#### Enumerated events

Ordinal DLCs based on enumerated events include, in addition to the regular enumerated event information, an array of booleans indicating for each possible outcome whether the ordinal should be given to the _offer_ party (note that this means that this array _must_ have the same number of elements as there are possible outcomes).基于enumerated events包括一组布尔值，用于指示对于每个可能的结果，序数是否应该给予报价方（请注意，这意味着该数组必须具有与可能结果数量相同的元素数量基于枚举事件的序数DLC还包括一组布尔值，用于指示对于每个可能的结果，序数是否应该给予报价方）。

#### Numerical events

Ordinal DLCs based on numerical events include, in addition to the regular numerical event information, an array or intervals. These intervals indicate the ranges of outcomes for which the ordinal should be given to the offer party.基于数值事件的序数DLC还包括一个区间数组。这些区间指示序数应该给予报价方的结果范围。

### Limitations

#### Changing postage

目前无法更改序数的邮资。

这意味着如果一个序数包含在一个1BTC的UTXO中，整个1BTC将被包含在DLC中，并且在UTXO中的sats不会改变的情况下交给序数的获胜者

因此，在将序数包含在DLC中之前，应由钱包预先更改序数的邮资。

此外，序数的邮资将与获胜者的支付合并

这意味着如果一方获得了一个邮资为1BTC的ordinal，并且还有1BTC的支付，包含该序数的CET输出的价值将为2BTC。
