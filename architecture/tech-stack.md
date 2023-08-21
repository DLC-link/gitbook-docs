# Architecture Overview

## Philosophy

Bitcoin has become a global phenomenon and a secure store of value, free from government interference and institutional risks. Despite its advantages, Bitcoin's security measures limit its programmability, lacking a smart contract interface for robust financial applications.

Ethereum and other chains have filled this gap with their robust smart contract platforms, driving tremendous growth but also leading to speculation and perceived risks. In the Bitcoin community, Decentralized Finance (DeFi) can be seen as a high-risk venture, associated with volatility and public perception issues.

We believe that Bitcoin is ideal as a settlement layer, to be used within decentralized financial systems on other platforms. Using Bitcoin as a store of value that secures sophisticated applications across various blockchains reduces risks and aligns with a decentralized vision.

Please read more about our philosophy here: [https://www.dlc.link/blog/introducing-the-bitcoin-plus-mentality](https://www.dlc.link/blog/introducing-the-bitcoin-plus-mentality)

## How DLCs Achieve This Vision

Discreet Log Contracts (DLCs) create a secure and decentralized framework for financial transactions. By predefining possible outcomes for Bitcoin before its use as collateral and running the application logic on a blockchain that supports smart contracts, DLCs significantly minimize the potential for errors or malicious acts. This structure ensures that the details of Bitcoin payments and wallets are kept minimal, aligning with the principles of decentralization. Furthermore, the use of Bitcoin Attestors in a consensus-based, abstract manner adds another layer of security, leveraging the power of decentralized security while maintaining integrity and trust in the system.

### Securing Bitcoin Attestors and DLCs

Bitcoin Attestors, operated by various independent node operators, work to attest to the payout outcome in a decentralized manner. Their functionality is abstract, meaning Attestors don't know the specific outcome or participants, enhancing security. Consensus among multiple Attestors is used for signing, leveraging decentralized security. Misbehaving Attestors can be penalized through slashing, maintaining integrity.

Read more about Bitcoin Attestors here: [https://www.dlc.link/blog/what-is-a-bitcoin-attestor](https://www.dlc.link/blog/what-is-a-bitcoin-attestor)

## Supported Blockchains

Currently we're supporting EVM compatible change such as **Ethereum** as well as the [Stacks](https://www.stacks.co/) blockchain. Solana, Polkadot, Cosmos and other chains will be added soon.

### Onboarding new Blockchains

Onboarding a new blockchain with our DLC management contracts takes just a few weeks to implement, followed by a security assessment and testing phase. Since our tool bridges to native Bitcoin and leverages Bitcoin wallets and decentralized Bitcoin Attestors, it can integrate with nearly any blockchain that handles financial transactions. This flexibility means that there are minimal requirements on the underlying blockchain, allowing for broad development potential.

## Components of a DLC

###

<figure><img src="../.gitbook/assets/DLC.Link_TechnicalFlow_latest.png" alt=""><figcaption></figcaption></figure>

### Wallets

A key component of the DLC.Link architecture is our inclusion in various BTC supporting wallets. DLCs are designed so that the smart contracts and Bitcoin Attestors know as little about the details of the bitcoin payments and wallets as possible. This is a great security feature, and means the wallets themselves need to do a good deal of DLC signing.

Two types of wallets are anticipated as being needed, one for end users, and one for application services that enterprises would run in an automated fashion.&#x20;

For either case, we have developed and built-upon DLC libraries that will provide this functionality.

#### JS Library for Browser Extensions and Mobile Wallets

For end-user wallets, that are often written in JS or ReactNative, we have a JS library that can easily be dropped into existing web/mobile wallets. We have also developed our own DLC-Enabled BTC wallet for testing purposes.

#### Router Wallet Service

We are developing a secure, reliable wallet service for enterprises and financial institutions to automate their DLC and Bitcoin interactions. This tool is built in Rust and leverages a well-developed and featureful open-source DLC management project.

### Attestors

The Bitcoin Attestors consist of independent nodes running DLC Attestor software, used to sign and attest to DLC outcomes. In addition to signing, the Attestor fulfills a listening function, communicating with the create and payout steps with smart contracts. With no application logic in the listener, safety is ensured through a back-checking entity that verifies the results of the Bitcoin Attestors on-chain. This verification guarantees the accuracy of the Attestors' signed values and provides assurance against manipulation at any point in the process.

### Smart Contracts

Smart contract integrations provide enhanced security by allowing both the attestor and the system to verify smart contract outputs directly on-chain. This ensures a transparent and trustworthy process. Our Bitcoin Attestors have been connected to blockchain DApps to leverage this benefit, merging the strength of Bitcoin with cutting-edge platforms for application development. The integration process is made even more attractive by its simplicity, requiring only the implementation of the "Open DLC" and "Close DLC" functions. These functions usually consist of fewer than 30 lines of code, making it an accessible solution that combines robust security with efficiency.

