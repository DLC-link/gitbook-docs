# Architecture Overview

## Philosophy

Bitcoin has become a global phenomenon and a secure store of value, free from government interference and institutional risks. Despite its advantages, Bitcoin's security measures limit its programmability, lacking a smart contract interface for robust financial applications.

Ethereum and other chains have filled this gap with their robust smart contract platforms, driving tremendous growth but also leading to speculation and perceived risks. In the Bitcoin community, Decentralized Finance (DeFi) can be seen as a high-risk venture, associated with volatility and public perception issues.

We believe that Bitcoin is ideal as a settlement layer, to be used within decentralized financial systems on other platforms. Using Bitcoin as a store of value that secures sophisticated applications across various blockchains reduces risks and aligns with a decentralized vision.

Please read more about our philosophy here: [https://www.dlc.link/blog/introducing-the-bitcoin-plus-mentality](https://www.dlc.link/blog/introducing-the-bitcoin-plus-mentality)

## How DLCs Achieve This Vision

Discreet Log Contracts (DLCs) create a secure and decentralized framework for financial transactions. By predefining possible outcomes for locked Bitcoin collateral, and running the business logic on a blockchain in smart contracts, DLCs significantly minimize the potential for errors or malicious acts.&#x20;

This structure also ensures that the details of Bitcoin payments and wallets are kept secret, aligning with the principles of privacy and trustlessness. Furthermore, the use of Bitcoin Attestors in a consensus-based, abstract manner adds another layer of security, leveraging the power of decentralized security while maintaining integrity and trust in the system.

## Supported Blockchains

Currently we're supporting Ethereum or a corresponding L2. Other EVM chains will follow soon, and then Solana, Polkadot, Cosmos and other chains will be added later.

## Components of a DLC

<figure><img src="../.gitbook/assets/DLC.Link_TechnicalFlow_latest.png" alt=""><figcaption></figcaption></figure>

### A Standard DLC Flow

Here we will describe how a user interaction with the DLC system looks, following along with the picture above.

**Setting up the DLC**

Starting in the top left of the diagram, and following the blue "open DLC" line.

1. The user interacts with a dApp's smart contract, for example to create a new loan or deposit vault in the dApp. Usually a user will indicate how much Bitcoin collateral they want to lock.
2. The dApp smart contract calls the _open-dlc_ function on the DLC.Link smart contract on the EVM chain.
3. The DLC.Link Attestors pick up the event, and create a DLC Event in their datastore**.**
4. Once the transaction is complete, the user can build the BTC locking transaction, and the corresponding pre-signed payout transaction (PSBT), and send those details to the Attestor Network.
5. When the Attestor Network comes to a consensus that this "locking transaction" conforms to the exact specifications needed, and that there is enough confirmations for security, the Attestors do a threshold signing to the EVM smart contract, marking the locking process as complete.
6. In the case of the DLC Bitcoin Bridge, this in-turn mints the associated wrapped tokens (dlcBTC). In other future cases, there can be other actions taken after the BTC bridging confirmation is validated.

The DLC is now set up, the Bitcoin is locked, and the collateral is in-use on the second blockchain.

**Closing the DLC**

Starting in the top left of the diagram, and following the purple "open DLC" line.

1. An event occurs on the smart blockchain, such as burning the wrapped token, which triggers the close flow of the vault. This calls the _close-dlc_ function on the DLC.Link smart contract.
2. That is picked up by the Attestors. When a consensus agree that the token burn has occured, they will perform a threshold signing to close the DLC, and return the locked BTC back to the original owner. NOTE: returning the BTC to the original owner is the only possible outcome supported currently, thus providing an unprecidented level of security for a Bitcoin bridge.
3. When the return of the locked BTC collateral is confirmed on-chain, the Attestors build consensus to write back to the EVM contract, marking the whole flow as complete.

Now that we've described the whole DLC flow, we can discuss each piece of the technology individually.

### DLC Attestors

The most important part of the DLC.Link architecture is the DLC Attestation Layer. Similar in some ways to "validator networks" on bridges, the DLC Attestors work in two steps:

1. The DLC Attestors create a DLC **announcement** when the user indicates that they want to lock bitcoin. This event is used by the bitcoin wallets.&#x20;
2. Later, the DLC Attestors listen for a corresponding event on Ethereum or Stacks (or other chain) regarding the outcome of the DLC, and sign a corresponding **attestation.**&#x20;

With the **attestation,** the participant's bitcoin wallet is able to sign one and only one of the CETs in the original outcome set. Thus the Bitcoin UTXO is "moved," or assigned to the new wallet based on that CET.

The Bitcoin Attestors consist of independent nodes running DLC Attestor software. They are managed and run by independent third-party node operators, who may run exactly one node each. Together they make up the Bitcoin Attestation Layer for DLC.Link.

In addition to creating announcements and attestations for DLC outcomes, the Attestor fulfills a listening function, responding to the **create** and **payout** events on the corresponding **DLC.Link Manager smart contract**. With no business logic in the Attestor, but rather just a **listen-and-sign** process, discreetness and security are ensured, and through a back-checking entity that verifies the results of the Bitcoin Attestors on-chain we can always ensure that an Attestor is behaving well. This verification guarantees the accuracy of the Attestors' actions and provides assurance against manipulation at any point in the process. It is visable thorugh the **DLC.Link Health Dashboard** (coming soon).

Bitcoin Attestors, operated by various independent node operators, work to attest to the payout outcome in a decentralized manner. Their functionality is abstract, meaning Attestors don't know the specific outcome or participants, enhancing security. Consensus among multiple Attestors is used for signing, leveraging decentralized security. Misbehaving Attestors can be penalized through slashing, maintaining integrity.

Read more about Bitcoin Attestors here: [https://www.dlc.link/blog/what-is-a-bitcoin-attestor](https://www.dlc.link/blog/what-is-a-bitcoin-attestor)

### Bitcoin Wallets

A key component of the DLC.Link architecture is our inclusion in various BTC supporting wallets. DLCs are designed so that the smart contracts and Bitcoin Attestors know as little about the details of the bitcoin payments and wallets as possible. This is a great security feature, and means the wallets themselves need to do a good deal of DLC signing.

Two types of wallets are anticipated as being needed, one for end users, and one for application services that enterprises would run in an automated fashion.&#x20;

For either case, we have developed and built-upon DLC libraries that will provide this functionality.

#### JS Library for Browser Extensions and Mobile Wallets

For end-user wallets, that are often written in JS or ReactNative, we have a JS library that can easily be dropped into existing web/mobile wallets. We have also developed our own DLC-Enabled BTC wallet for testing purposes.

The DLC.Link signing library is publically available here: [https://www.npmjs.com/package/@dlc-link/dlc-tools](https://www.npmjs.com/package/@dlc-link/dlc-tools)

DLC signing is available in the Leather wallet. [https://leather.io/](https://leather.io/)

You can read more about the JS library and setting it up here:  [bitcoin-wallets.md](installation-and-setup/bitcoin-wallets.md "mention")

#### Router Wallet Service

We have released a secure, reliable BTC/DLC signing application for financial institutions and dApps to automate their DLC and Bitcoin interactions. This tool is built in Rust and leverages a well-developed and featureful open-source DLC management project. While this service can sign Bitcoin DLC transactions, it does not have wallet functionality, and does not manage any funds directly.

### Smart Contracts

Smart contract integrations provide enhanced security by allowing both the attestor and the system to verify smart contract outputs directly on-chain. This ensures a transparent and trustworthy process. Our Bitcoin Attestors have been connected to blockchain DApps to leverage this benefit, merging the strength of Bitcoin with cutting-edge platforms for application development. The integration process is made even more attractive by its simplicity, requiring only the implementation of the "Open DLC" and "Close DLC" functions. These functions usually consist of fewer than 30 lines of code, making it an accessible solution that combines robust security with efficiency.

