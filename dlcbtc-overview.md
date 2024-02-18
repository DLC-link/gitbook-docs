---
description: Non-custodial wrapped Bitcoin for DeFi on Ethereum
---

# dlcBTC Overview

**Introduction to dlcBTC**

dlcBTC is a non-custodial representation of Bitcoin on Ethereum, enabling Bitcoin holders to participate in DeFi protocols while retaining full ownership of their assets. It employs Discreet Log Contracts (DLCs) to lock Bitcoin in a multisig UTXO, with one key held by the user and the other distributed across a decentralized network.

**How dlcBTC Works**

1. **Locking BTC**: Users lock their BTC into a DLC using DLC.Link's bridge. This process mints dlcBTC tokens equivalent to the amount of BTC locked.
2. **DLC Mechanism**: A DLC acts as a lockbox via a contract on the Bitcoin blockchain, detailing a pre-signed agreement between the user and the protocol.
3. **Key Distribution**: Users hold one key to the multisig UTXO, and the second is distributed among the attestor nodes. The DLC can only be liquidated back to the user, safeguarding against theft or loss.
4. **Attestor Layer**: A network of seven trusted node operators monitor blockchain events, announce DLC creations, and validate outcomes. They support the bridge by ensuring reliable cross-chain communication without holding users' keys.
5. **Using dlcBTC**: Minted dlcBTC tokens can be employed as collateral within various DeFi platforms, such as Curve and AAVE.

**Comparison to wBTC and Other Bridged Assets**

dlcBTC is distinct from wBTC and other bridged assets like tBTC and BTC.B by eliminating the need for intermediaries or custodians, instead locking Bitcoin on-chain with user sovereignty as a core principle.&#x20;

dlcBTC is secured by the full hashrate of the Bitcoin network and doesn't require users to send their BTC to third-party deposit addresses.

Compared to wBTC specifically, dlcBTC offers three advantages:

1. **Self-wrapped:** dlcBTC is minted by having depositors (dlcBTC merchants) self-wrap, locking BTC with themselves in a DLC. Self-wrapping means that the DLC can only pay out to the original depositor, such that the BTC cannot be stolen in a hack or confiscated by government actions.
2. **Fully automated:** wBTC takes 3-12 hours to mint or burn due to manual steps in BitGo's custody process. dlcBTC is fully automated and can mint and burn in 3-6 BTC block confirmations
3. **Flexible fees:** Since DLC.Link is not a custodian, dlcBTC has less overhead and can offer more competitive mint and burn fees.

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

dlcBTC v1 is set for release in Q1 2024. Ahead of the launch, a testnet will be available, offering early access and opportunities for user feedback.&#x20;

For updates on our launch, [follow us on Twitter](https://twitter.com/dlc\_link) or [join our Discord](https://discord.gg/pA4rVKfNAA)!

\
