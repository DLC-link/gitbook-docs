---
description: Non-custodial wrapped Bitcoin for DeFi on Ethereum
---

# dlcBTC Overview

**Introduction to dlcBTC**

dlcBTC is a tokenized representation of Bitcoin on Ethereum, allowing Bitcoin holders to engage in DeFi protocols without relinquishing ownership of their assets. dlcBTC utilizes Discreet Log Contracts (DLCs) to lock Bitcoin on-chain, ensuring that users maintain control of their funds through a 2-of-2 multisig UTXO. One key is held by the user, and the other is held in a decentralized manner by the network.

**How dlcBTC Works**

1. **Locking BTC**: Users initiate a transaction to lock their BTC into a DLC. This action mints dlcBTC tokens equivalent to the amount of BTC locked.
2. **DLC Mechanism**: A DLC is a simple "smart contract" on the Bitcoin blockchain that enforces the rules of the agreement between the user and the protocol.
3. **Key Distribution**: Users hold one key to the multisig UTXO, ensuring they retain control over their assets.
4. **Attestor Layer**: This layer comprises entities that monitor blockchain events, announce new DLCs, and validate outcomes, facilitating cross-chain communication.
5. **Using dlcBTC**: Once dlcBTC tokens are minted, users can use them as collateral in various DeFi platforms.

**Comparison to wBTC and Other Bridged Assets**

Unlike wBTC, which relies on a single custodian (BitGo), dlcBTC remains securely locked on-chain, negating the need for a middleman. This approach also differs significantly from bridged assets like tBTC and BTC.B, which typically involve trust in third-party bridge validators or other intermediaries.

**Minting Mechanism**

The process of minting dlcBTC involves creating a DLC where the user's BTC is locked. This is done via a two-step verification process:

1. **Initiation**: The user locks BTC into a 2-of-2 multisig address, which triggers the creation of a DLC.
2. **Confirmation**: Upon confirmation of the locked BTC, dlcBTC tokens are minted and delivered to the user's wallet, reflecting the locked amount.

**Features of dlcBTC v1**

dlcBTC v1 will launch with the following features:

* **Whitelisted access**: Access to the dlcBTC bridge is exclusive to whitelisted entities such as Bitcoin miners, exchanges, and financial institutions, ensuring a focused and secure environment for large-scale operations.
* **Security**: In v1, depositors will be locking BTC with themselves. All payout addresses from the DLC will point to the user's wallet. Thus, it will be impossible for BTC to be hacked or stolen in any adverse event.
* **Redemptions**: At launch, dlcBTC will partner with several centralized and decentralized exchanges to enable common trading pairs, including dlcBTC-BTC to enable redemption back into BTC.
* **Integration**: dlcBTC is designed to integrate smoothly with existing DeFi protocols, providing institutions with a seamless transition into decentralized finance.

**Launch Plans**

We are looking to launch dlcBTC v1 in Q1 2024. Stay tuned for more updates!

\
