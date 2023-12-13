---
description: ERC-721 NFT Representing Locked Bitcoin Collateral
---

# NFT Token Functions

### **Abstract**

Here we describe the design and implementation of the DLC.Link Bitcoin-backed ERC-721 NFT, which serves as a "proof-of-collateral" for BTC locked in a DLC. The NFT is minted and sent to the user's wallet upon locking their BTC in the DLC, and its metadata is stored on-chain and on decentralized storage systems such as IPFS. The NFT enables Bitcoin to be used securely and easily in various transactions, including trading, borrowing, and investing. When the NFT is burned, the BTC is released back to the owner's wallet or paid to the burner in a token native to the chain it was burned on.

### **Introduction**

The DLC.Link ERC-721 NFT is a new product designed to facilitate the safe and easy leveraging of bitcoin on other chains. This technical document describes the features and implementation of the NFT, including its design, minting process, metadata storage, and burning process.

### **Design**

The ERC-721 NFT is designed to represent a "proof-of-collateral" for BTC locked in a DLC. The NFT image lists the amount of BTC locked in it, while its metadata is stored on-chain and on decentralized storage systems such as IPFS. The NFT is fully transferable, and it can be used on any platform that supports the ERC-721 standard.

### **Minting**

Upon locking their BTC in a DLC, the NFT is minted and sent to the user's wallet. The NFT serves as a secure and transparent record of the collateral locked in the DLC. This enables users to keep track of their collateral and ensure its security at all times.

### **Metadata Storage**

The NFT's metadata is stored on-chain and on decentralized storage systems such as IPFS. This provides a high level of security for the collateral locked in the DLC, as the BTC can only be accessed by the owner of the NFT. The NFT's metadata also includes information about the amount of BTC locked in the NFT, making it easy for users to monitor their collateral.

### **Burning**

When the user decides to burn their NFT, the BTC locked in the DLC is released back to their bitcoin wallet. This provides a quick and easy way for users to access their collateral when they need it. If another account burns the NFT, the underlying sats move to the counterparty ("Bob") wallet. The party that burned the NFT can then redeem its value with Bob (a "broker").

Below you can see a demo of how NFTs are minted by locking native BTC, and how they're used on a platform such as https://arcade.xyz

{% embed url="https://www.youtube.com/watch?v=MsjzQwaQVjo" %}
