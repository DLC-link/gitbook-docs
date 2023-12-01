# Liquidation with NFT

DLC.Link has designed a token standard using ERC-721 (NFT) or ERC-1155 (Semi-fungible token) tokens which represent Bitcoin locked in the DLC escrow. This type of token is often referred to as a **Proof of Reserve** or **Proof of Collateral** token.

This **Proof of Collateral** token implements the "DLC-open" and "DLC-close" functions in the token's mint and burn functions, respectively. Unlike wrapped Bitcoin (wBTC) or its variants, the token itself doesn't hold value. Instead, it provides cryptographic proof that funds are locked on Bitcoin chain. As such, the token is much **safer** than a traditional wrapped Bitcoin.

The token can be redeemed via a "peg-out" service provided by a Liquidator. Here's an example:



### NFT Peg-In and Peg-Out

#### **Setting Up a Loan with PoC Token:**

* Borrowing users initiate the loan process via a DeFi app.
* They're directed to a bridging application, which can be a third-party liquidator working with a Router.
* The details of the DLC are set up, identifying the Bitcoin collateral.
* The Bitcoin is locked into the DLC, and Proof of Collateral tokens are minted.
* The PoC tokens are then used as collateral in the DeFi lending app to borrow stablecoin.
* The stablecoin loan is issued to the user's wallet.

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

**Proof of Collateral Token:**

* The PoC token embodies "DLC-open" and "DLC-close" functions, providing cryptographic proof that funds are locked on the Bitcoin chain.
* Unlike traditional wrapped Bitcoin, it doesn't hold value but verifies the collateral's existence.
* It can be redeemed through a liquidator (in this case, third-party liquidators).



**Liquidation Flow with Proof of Reserve Token:**

* Should the user default, the NFT may be transferred or seized.
* To redeem the Bitcoin collateral, third-party liquidators approved by the protocol handle the process, with a Router serving as the counterparty.
* Liquidators send payment to cover the loan, receiving the PoC tokens from the DeFi protocol.
* The PoC tokens are burned, informing the DLC.Link smart contract.
* The DLC is closed with the associated liquidation outcome, releasing the BTC to the liquidator.
* The liquidator concludes the process, paying out according to the pre-negotiated terms.

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

This approach allows for enhanced flexibility, security, and potential decentralization in the liquidation process. The use of a Router and third-party liquidators adds an extra layer of functionality and control, facilitating seamless integration with existing DeFi platforms.
