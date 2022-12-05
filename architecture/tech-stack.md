# Tech Stack and Architecture Overview

## Philosophy - Secure, Resilient, Trustless

Bitcoin’s spectacular growth has turned it into a global phenomenon. Its decentralized model, predictable monetary policy and self-sovereign principles have made it an effective store of value. Today, Bitcoin is probably the best savings account in the world. When kept in self-custody in a user’s own wallet, it is not subject to adverse government actions, hacks or bankruptcies that plague other banks and financial institutions. Bitcoin however does not have a smart contract interface, and it is practically impossible to build robust financial applications with Bitcoin script. The tight security and hack-resistance of Bitcoin severely limit its programmability.&#x20;

Meanwhile, Ethereum and other chains have rapidly moved to fill this gap. Ethereum features a robust smart contract platform that can be used to create sophisticated financial applications. Its rise in popularity, driven by a central founding team and fueled by venture capital funding, has led to both tremendous growth and rampant speculation. In the eyes of Bitcoiners, Decentralized Finance (DeFi) is essentially a high-risk casino. Similar to the allure of crowdfunding, individuals have the ability to speculate and gamble on over 20,000 available tokens. This trading activity often fills Bitcoiners with dread, especially when smart contract hacks and custodian failures lead to greater volatility and hurts public perception.

Instead of attempting to rebuild financial applications and currencies on top of Bitcoin, it makes more logical sense to use Bitcoin as a stable reserve layer within future decentralized financial systems that are being built on other platforms. It's become clear that entrusting centralized entities to safeguard massive reserve assets is overly risky, so it makes sense to utilize Bitcoin as a store of value that secures the sophisticated applications running on various blockchains on top.

## How Bitcoin Oracles and DLCs Achieve This

We believe in this future, and are designing our infrastructure and Bitcoin Oracles to support this goal, and following in the designs and theory of blockchains and decentralized programming.

Through DLCs, we can predefine the possible outcomes for the Bitcoin before the user agrees to use it as collateral. And by running the application logic on a full-features blockchain which supports smart contracts, we drastically reduce the possible areas where errors or malicious intent can occur.&#x20;

### Securing Bitcoin Oracles and DLCs

The Bitcoin Oracles are operated decentrally by various distinct node operators. When creating a DLC, the participants are leveraging the Bitcoin oracles to attest to the payout outcome indicated by the blockchain smart contract. The oracles don't know what the outcome they're signing means, nor do they know who the participants are. Furthermore, multiple oracles are used, and the consensus is used for the signing, leveraging the power of decentralized security even further. Finally, if an oracle is seen to be acting badly (intentionally or not) they are punished through a common _slashing_ operation.&#x20;

## Supported Blockchains

Currently we're supporting EVM compatible change such as **Ethereum** (Optimism, Avalanche and others coming soon) as well as the [Stacks](https://www.stacks.co/) blockchain. Next on our roadmap is Cosmos.

### Onboarding new Blockchains

Onboarding a new blockchain requires writing our DLC management contracts and associated example contracts, to the new framework. This generally takes only a few weeks to implement, followed by a period of time for security assessment and testing.

Because our tool's purpose is to bridge to native Bitcoin, nearly any blockchain that does financial transactions of any kind is a strong candidate for integration. We don't rely on any specific technologies of the underlying blockchain, as most of the heavy lifting is done by the Bitcoin wallets and our decentralized set of Bitcoin Oracles. This means there are very few requirements on the blockchain, and we plan to be able to develop our tools into many of them.&#x20;

## Components of a DLC

### Wallets

#### JS Library for Browser Extensions and Mobile Wallets

#### Backend Wallet Service

### Oracle

### Smart Contracts

### Listening Agent



##
