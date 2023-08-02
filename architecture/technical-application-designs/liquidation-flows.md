# Liquidation Flows

In order for a lender to eliminate their own need to manage native Bitcoin payments, the system can be designed with what we're calling a **Liquidator**. The liquidator is the counterparty in the DLC. In the case of a default, Bitcoin is seized and sent to the liquidator, who repays the lender in exchange for a transaction fee.

### Simple Liquidation Flow

If the user repays the loan, collateral is returned to the borrower and the liquidator is not utilized.\
\
In the case of a default, the Bitcoin moves to the liquidator. Working with an exchange or OTC trading desk, the liquidator swaps the Bitcoin for wBTC, USDC or another asset. The liquidator sends the funds to the lender, less a processing fee.

<figure><img src="../../.gitbook/assets/DLC.Link_SimpleLiquidationFlow (3).png" alt=""><figcaption></figcaption></figure>

### Isolated Liquidity Pool

#### What is an Isolated Liquidity Pool?

The term Isolated Liquidity Pool refers to a smart contract that allows for multiple independent participants to support a **Liquidator** during a liquidation event. The pool of liquidators is referred to as _isolated_ because it's totally unaffiliated with the original loan and instead participates via smart contracts.

See below for an example setup of a liquidator supported Bitcoin-collateral loan with an isolated liquidity pool.

<figure><img src="../../.gitbook/assets/DLC.Link_IPLLiquidationFlow.png" alt=""><figcaption></figcaption></figure>
