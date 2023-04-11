# Tech Stack and Architecture Overview

## Philosophy - Secure, Resilient, Trustless

Bitcoin’s spectacular growth has turned it into a global phenomenon. Its decentralized model, predictable monetary policy and self-sovereign principles have made it an effective store of value. Today, Bitcoin is probably the best savings account in the world. When kept in self-custody in a user’s own wallet, it is not subject to adverse government actions, hacks or bankruptcies that plague other banks and financial institutions. Bitcoin however does not have a smart contract interface, and it is practically impossible to build robust financial applications with Bitcoin script. The tight security and hack-resistance of Bitcoin severely limit its programmability.&#x20;

Meanwhile, Ethereum and other chains have rapidly moved to fill this gap. Ethereum features a robust smart contract platform that can be used to create sophisticated financial applications. Its rise in popularity, driven by a central founding team and fueled by venture capital funding, has led to both tremendous growth and rampant speculation. In the eyes of Bitcoiners, Decentralized Finance (DeFi) is essentially a high-risk casino. Similar to the allure of crowdfunding, individuals have the ability to speculate and gamble on over 20,000 available tokens. This trading activity often fills Bitcoiners with dread, especially when smart contract hacks and custodian failures lead to greater volatility and hurts public perception.

Instead of attempting to rebuild financial applications and currencies on top of Bitcoin, it makes more logical sense to use Bitcoin as a stable reserve layer within future decentralized financial systems that are being built on other platforms. It's become clear that entrusting centralized entities to safeguard massive reserve assets is overly risky, so it makes sense to utilize Bitcoin as a store of value that secures the sophisticated applications running on various blockchains on top.

Please read more about our philosophy here: [https://www.dlc.link/blog/introducing-the-bitcoin-plus-mentality](https://www.dlc.link/blog/introducing-the-bitcoin-plus-mentality)

## How Bitcoin Attestors and DLCs Achieve This

We believe in this future, and are designing our infrastructure and Bitcoin Attestors to support this goal while following the designs and vision of blockchains and decentralized applications.

Through DLCs, we predefine the possible outcomes for the Bitcoin before the user / protocol agrees to use it as collateral. With this, and by running the application logic on a full-features blockchain which supports smart contracts, we drastically reduce the surface-area where errors or malicious intent can occur. DLCs are designed so that the smart contracts and Bitcoin Attestors know as little about the details of the bitcoin payments and wallets as possible.&#x20;

### Securing Bitcoin Attestors and DLCs

The Bitcoin Attestors are operated decentrally by various distinct node operators. When creating a DLC, the participants are leveraging the Bitcoin Attestors to attest to the payout outcome indicated by the blockchain smart contract.&#x20;

For security through abstraction, the attestors don't know what the outcome they're signing means, nor do they know who the participants are. Furthermore, multiple attestors are used, and the consensus among that subset is used for the signing, leveraging the power of decentralized security even further. Finally, if an attestor is seen to be acting badly (intentionally or not) they can be punished through a common _slashing_ operation.&#x20;

Read more about Bitcoin Attestors here: [https://www.dlc.link/blog/what-is-a-bitcoin-oracle](https://www.dlc.link/blog/what-is-a-bitcoin-oracle)

## Supported Blockchains

Currently we're supporting EVM compatible change such as **Ethereum** (Optimism, Avalanche and others coming soon) as well as the [Stacks](https://www.stacks.co/) blockchain. Next on our roadmap is Cosmos.

### Onboarding new Blockchains

Onboarding a new blockchain requires writing our DLC management contracts and associated example contracts, to the new framework. This generally takes only a few weeks to implement, followed by a period of time for security assessment and testing.

Because our tool's purpose is to bridge to native Bitcoin, nearly any blockchain that does financial transactions of any kind is a strong candidate for integration. We don't rely on any specific technologies of the underlying blockchain, as most of the heavy lifting is done by the Bitcoin wallets and our decentralized set of Bitcoin Attestors. This means there are very few requirements on the blockchain, and we plan to be able to develop our tools into many of them.&#x20;

## Components of a DLC

### Wallets

A key component of the DLC.Link architecture is our inclusion in various BTC supporting wallets. DLCs are designed so that the smart contracts and Bitcoin Attestors know as little about the details of the bitcoin payments and wallets as possible. This is a great security feature, and means the wallets themselves need to do a good deal of DLC signing.

Two types of wallets are anticipated as being needed, one for end users, and one for application services that enterprises would run in an automated fashion.&#x20;

For either case, we have developed and built-upon DLC libraries that will provide this functionality.

#### JS Library for Browser Extensions and Mobile Wallets

For end-user wallets, that are often written in JS or ReactNative, we have a JS library that can easily be dropped into existing web/mobile wallets. We have also developed our own DLC-Enabled BTC wallet for testing purposes.

#### Backend Wallet Service

We are developing a secure, reliable wallet service for enterprises and financial institutions to automate their DLC and Bitcoin interactions. This tool is built in Rust and leverages a well developed and featureful open-source DLC management project.

### Attestor

The Bitcoin Attestors make up a disjoint set of independent nodes running DLC attestor software. They are used to sign and attest to DLC outcomes. See the [above section](tech-stack.md#securing-bitcoin-oracles-and-dlcs) for more details.

### Smart Contracts

Smart contracts on decentralized blockchains provide the best, most secure and reliable way to manage secure applications, such as payment protocol. They are open source, decentralized, censorship resistant, and immutable.

This is why we connected our Bitcoin Attestors, to blockchain dapps to provide the best digital currency with the best platforms for application development.

### Listening Agent

In order to communicate between our Bitcoin Attestors and the smart contracts using DLCs, the DLC _listener_ is used. This tool passes the create and payout steps of the contracts between the smart contracts and the Bitcoin Attestors.&#x20;

There is essentially no logic in the listener, and in order to ensure safety, we will verify the results of the Bitcoin Attestors on chain, guaranteeing that the Attestors signed the correct values and there was no manipulation anywhere along the path.
