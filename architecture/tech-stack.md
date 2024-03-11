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

## Steps of a DLC

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

## Components of the DLC.Link Architecture

### DLC Attestors

The most important part of the DLC.Link architecture is the DLC Attestation Layer. Similar in some ways to "validator networks" on bridges, the DLC Attestors work have two main functions:

1. **Bridging BTC to EVM chain**
   1. The DLC.Link Attestors accept the details of a BTC bridging request, which consist of a PSBT and a locking transaction, and check the state of a controlling EVM contract.&#x20;
   2. Once validated and confirmed on BTC chain, the nodes form a consensus and use a threshold ecdsa signature to execute the bridging action on the smart contract. In the case of the DLC.Link bridge, that mints dlcBTC tokens.
2. **Redeeming BTC from EVM chain**
   1. Later, the DLC.Link Attestors listen for a corresponding event on EVM. In the case of the DLC.Link Bridge, this would be the burn of the associated dlcBTC tokens. The Attestors form a consensus and sign a schnorr threshold signature  to unlock the previously locked BTC collateral, and broadcast out the bitcoin TX.
   2. When the redeem tx is confirmed, they again form consensus and use a threshold ecdsa signature to close the DLC flow.

The Bitcoin Attestors consist of independent nodes running DLC Attestor software. They are managed and run by independent third-party node operators, who may run exactly one node each. Together they make up the Bitcoin Attestation Layer for DLC.Link.

With no business logic in the Attestor, but rather just a **listen-and-sign** mechanism, _discreetness_ and security are ensured. Through a reputation system and back-checking entity run by all the nodes, which verifies the results of the Bitcoin Attestors on-chain, we can be sure that an Attestor is behaving well. This verification guarantees the accuracy of the Attestors' actions and provides assurance against manipulation at any point in the process. This is then completely visable through the **DLC.Link Health Dashboard** (coming soon).

Bitcoin Attestors, operated by various independent node operators, work to attest to the payout outcomes in a decentralized manner. Misbehaving Attestors can be penalized through slashing, maintaining integrity.

Read more about Bitcoin Attestors here: [https://www.dlc.link/blog/what-is-a-bitcoin-attestor](https://www.dlc.link/blog/what-is-a-bitcoin-attestor)

### Supported Bitcoin Wallets

DLC.Link technology is supported by any bitcoin wallet which supports Taproot, and can sign PSBTs. We are working to put together a list of wallets confirmed to be compatible (coming soon).&#x20;

### Smart Contracts

At DLC.Link, we believe the power of Bitcoin will be fulfilled when it is trustlessly bridged to a trusted smart contract chain where it can be fully leveraged. We therefore have powered our DLC.Link Attestors by our secure and heavily audited smart contract on an EVM chain. With this design, verifying the bridge is operating correctly (audit) is simple and clear, as the Attestors are simply responsible for doing consensus signing of the requests which come from the EVM contract.

Our contracts are fully open source, and can be found in the Solidity section on this docs page.

