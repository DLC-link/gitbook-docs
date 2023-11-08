---
description: Non-custodial wrapped Bitcoin for DeFi on Ethereum
---

# dlcBTC Overview

**Introduction to dlcBTC**

dlcBTC is a tokenized representation of Bitcoin on Ethereum, allowing Bitcoin holders to engage in DeFi protocols without endangering their assets. dlcBTC utilizes Discreet Log Contracts (DLCs) to lock Bitcoin on-chain, ensuring that users maintain control of their funds through a 2-of-2 multisig UTXO. One key is held by the user, and the other is held in a decentralized manner by the network. This decentralized mechanism ensures that the user's Bitcoin remains in their control.

**How dlcBTC Works**

1. **Locking BTC**: Users lock their BTC into a DLC using DLC.Link's bridge. This process mints dlcBTC tokens equivalent to the amount of BTC locked.
2. **DLC Mechanism**: A DLC acts as a lockbox via a contract on the Bitcoin blockchain, detailing a pre-signed agreement between the user and the protocol.
3. **Key Distribution**: Users hold one key to the multisig UTXO, and the second is distributed among the attestor nodes. The DLC can only be liquidated back to the user, safeguarding against theft or loss.
4. **Attestor Layer**: A network of seven trusted node operators monitor blockchain events, announce DLC creations, and validate outcomes. They support the bridge by ensuring reliable cross-chain communication without holding users' keys.
5. **Using dlcBTC**: Minted dlcBTC tokens can be employed as collateral within various DeFi platforms, such as Curve and AAVE.

**Comparison to wBTC and Other Bridged Assets**

Unlike wBTC, which relies on a single custodian (BitGo), dlcBTC remains securely locked on-chain, negating the need for a middleman. This approach also differs significantly from bridged assets like tBTC and BTC.B, which typically involve trust in third-party bridge validators or other intermediaries.

**Minting Mechanism**

The process of minting dlcBTC involves creating a DLC where the user's BTC is locked. This is done via a two-step verification process:

1. **Initiation**: The user locks BTC into a 2-of-2 multisig address, which triggers the creation of a DLC.
2. **Confirmation**: Upon confirmation of the locked BTC, dlcBTC tokens are minted and delivered to the user's wallet, reflecting the locked amount.

**Features of dlcBTC v1**

dlcBTC v1 will launch with the following features:

* **Whitelisted access**: Only Bitcoin miners, exchanges, and financial institutions pre-approved through a stringent vetting process will have access to dlcBTC's bridge, ensuring a secure and controlled environment for significant transactions.
* **Security**: Depositors lock BTC into the system, with the assurance that all DLC payout addresses direct funds exclusively to the depositor's wallet, making the theft or loss of BTC infeasible.
* **Redemptions**: Initially, dlcBTC will support dlcBTC-BTC trading pairs on chosen centralized exchanges to ensure direct conversion back to BTC. Additionally, trading pairs like dlcBTC-wBTC and dlcBTC-USDT will be available on widely-used decentralized exchanges.
* **Integration**: dlcBTC is designed to integrate smoothly with existing DeFi protocols, providing institutions with a seamless transition into decentralized finance.

**Launch Plans**

We are looking to launch dlcBTC v1 in Q1 2024. Stay tuned for more updates!

\
