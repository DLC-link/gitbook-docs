# Liquidation Flows

In the case of a default, Bitcoin is seized and must be liquidated to repay the lender (most commonly, in exchange for a transaction fee).

Several scenarios exist to handle liquidations. Liquidations can be facilitated via centralized entities, via smart contracts or by a mix of both.

Here are examples of each:

1. **DApp-Handled Liquidations**: The decentralized application (DApp) itself manages both the routing and liquidations. To facilitate this option, the DApp must be equipped with a DLC-enabled Bitcoin wallet, allowing it to function as the counterparty in the transaction.
2. **Third-Party Liquidations with Router**: In this scenario, liquidation is handled by a third-party, such as AAVE-style crowdsourced Liquidators. A component called the **Router** acts as the counterparty to the DLC and follows the DApp's instructions to move the liquidated BTC.
3. **Entity as a Liquidator with Router**: A specific entity serves as the Liquidator, with a Router acting as the counterparty, sending liquidated BTC to the Liquidator, who then repays the lender.
4. **Entity as a Liquidator with Off-Chain Transaction**: An entity called the "Liquidator" directly serves as the counterparty. This Liquidator then sells the BTC in an off-chain transaction to repay the lender.



Our assumption is that most DApps can't or won't want to handle a Bitcoin wallet address. In this scenario, to minimize trust, we generally recommend that the Router be implemented as a smart contract. Let's review a version of this.



### Third-Party Liquidations with Router

In this approach, the liquidation process is outsourced to a third party. This third-party does not directly interact with the underlying Discreet Log Contract (DLC), but instead, they engage through a component called the **Router**.

**How it Works:**

* **Router**: The Router serves as the counterparty to the DLC. It has the essential functionality to move the liquidated Bitcoin (BTC) as per the instructions from the decentralized application (DApp). The Router must be equipped to understand and execute the specific contract terms related to liquidation.
* **Third-Party Liquidators**: These are entities or individuals who can participate in the liquidation process. They may be incentivized through fees, and they interact with the Router through a predefined API.
* **DApp**: The DApp facilitates the liquidation process by establishing the rules and conditions under which liquidation occurs, and it communicates these to the Router. It may also handle other functionalities like monitoring the asset's price, determining liquidation thresholds, and more.

<figure><img src="../../.gitbook/assets/Router Flow.png" alt=""><figcaption></figcaption></figure>
