# Interacting with Bitcoin Contracts in the Hiro Wallet

## Overview

This documentation will show dApp developers how to access Bitcoin Contracts in their applications, via the Hiro wallet.

Enabling the interfacing of Bitcoin Contracts via Hiro wallet requires three steps:

1. Integrate your smart contract with the `dlc-manager` smart contract
2. Set up and run a Router Wallet
3. Configure your web app to support Bitcoin Contracts, consisting of two parts:
   1. Configure your web app to interact with the Router wallet
   2. Configure your web app to interact with the Hiro Wallet API

See the below sections for more details on each step.

_Note_ Bitcoin Contracts are powered by the underlying technology known as DLCs (Discreet Log Contracts) and sometimes the names are used interchangibly. [Learn more about DLCs here](https://www.dlc.link)

## Step 1 | Solidity / Clarity Smart Contract Integration

To enable the use of DLCs, which let users transact with native Bitcoin directly, it is essential to acquaint yourself with either DLC.Link's Clarity or Solidity contract. These are referred to as the DLC Manager contracts. Calling into the DLC Manager contract happens directly from your smart contract. See the following instructions for more details:

- [Solidity Docs](https://github.com/DLC-link/dlc-solidity/tree/1.0/prerelease)
- [Clarity Docs](https://github.com/DLC-link/dlc-clarity/tree/1.0/prerelease)

### Registering Your Smart Contract

As the DLC.Link product is currently in the beta-testing phase, you must first register your contract with the DLC.Link development team in our Discord server. Once you deploy your contract, please share it's address with the DLC.Link developers to have it whitelisted. Please contact DLC.Link on their [Discord server](https://discord.gg/MYphfaZrtB).

## Step 2 | Router Wallet Communication

Bitcoin Contracts are powered by the underlying technology known as DLCs (Discreet Log Contracts). These contracts involve two parties, an offeror and an acceptor. This is where the Router Wallet comes in. In the current setup of the DLC.Link solution, the user's Hiro wallet acts as the acceptor and the Router Wallet acts as the offeror. But the Router Wallet is more than just a Bitcoin wallet. It communicates with both the `dlc-manager` and the attestors and manages the lifecycle of a Bitcoin Contract, from creation to closure.

When a user wishes to accept a Bitcoin Contract and invokes the specified function, the dApp is expected to retrieve a Bitcoin Contract offer from the Router Wallet. Once the request arrives at the Router Wallet, it will then create a Bitcoin Contract with the given parameters and send it to the user's Hiro wallet. The user can then accept the offer and the Router Wallet will manage the lifecycle of the Bitcoin Contract.

### Router Wallet Setup

On how to set up the Router Wallet, please refer to the following documentation:
[Router Wallet](https://github.com/DLC-link/dlc-stack/tree/1.0/prerelease/wallet)

#### Registering Your Router Wallet

Once you have set up your Router Wallet, please share its address with the DLC.Link developers, as this also needs to be whitelisted during beta-testing. By doing so it will be granted permission for it to interact with our `dlc-manager` contract.

## Step 3 | dApp API Integration with the Router Wallet and Hiro Wallet

### Router Wallet API Integration

To integrate your dApp with the Router Wallet, follow these steps:

1. Request a Bitcoin Contract using the dlc-manager contract. This will provide you with a unique identifier (UUID).
2. Use the UUID and other Bitcoin Contract parameters obtained from the smart contract, or set them directly in your dApp.
3. From the dApp, send these parameters to the Router Wallet using the offer API endpoint.

The required parameters for the offer API endpoint are as follows:

- uuid: The unique identifier of the Bitcoin Contract.
- acceptCollateral: The amount of collateral the acceptor (user's wallet) is willing to deposit.
- offerCollateral: The amount of collateral the offeror (Router Wallet) is willing to deposit.
  _Note:_ In the current setup of the DLC Link solution, this is recommended to be 0.
- totalOutcomes: The total number of outcomes of the Bitcoin Contract.
- attestorList: The list of attestors that will be involved in the Bitcoin Contract.
  _[Router Docs](https://github.com/DLC-link/dlc-stack/tree/1.0/prerelease/wallet) to learn more about any of these parameters._

By providing these parameters to the Router Wallet, you can successfully fetch an offer for the Bitcoin Contract.

To fetch an offer from the Router Wallet, you should use a function similar to the one below:

```ts
export const fetchBitcoinContractOfferFromRouterWallet = async (
  uuid: string,
  acceptCollateral: Number,
  offerCollateral: Number,
  totalOutcomes: Number,
  attestorList: string[]
) => {
  const counterpartyWalletURL: string = 'https://router-wallet.com/offer'; // The Router Wallet Offer API endpoint.

  const attestorListJSON = JSON.stringify(attestorList); // Convert attestorList to JSON string.

  const response = await fetch(counterpartyWalletURL, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      uuid,
      acceptCollateral,
      offerCollateral,
      totalOutcomes,
      attestorList: attestorListJSON,
    }),
  });
  const bitcoinContractOffer = await response.json();
};
```

The function above will return a Bitcoin Contract Offer in string format, which you can then utilize to accept the offer in the next step.

### Hiro Wallet API Integration

To accept the received offer and engage with a Hiro wallet, you need to communicate with the Hiro wallet via the Wallet API.

The parameters for the API are as follows:

```ts
interface CounterPartyWalletDetails {
  counterpartyWalletURL: string; // The URL of the counterparty wallet.
  counterpartyWalletName: string; // The name of the counterparty wallet.
  counterpartyWalletIcon: string; // The icon associated with the counterparty wallet.
}

interface BitcoinContractRequestParams {
  bitcoinContractOffer: string; // The Bitcoin Contract Offer in string format.
  attestorURLs: string; // An array of attestor URLs (string[]) converted to JSON string.
  counterpartyWalletDetails: string; // A CounterPartyWalletDetails object converted to JSON string.
}
```

Ensure that all the necessary fields are JSON-stringified before utilizing them as param, as the API operates using JSON format.

#### Example

```ts
// receivedBitcoinContractOffer is the Bitcoin Contract Offer received from the Router Wallet.
const bitcoinContractOffer: string = JSON.stringify(receivedBitcoinContractOffer);

// attestorURLs is an array of attestor URLs, recieved from the Smart Contract object.
const attestorURLs: string = JSON.stringify([
  'https://attestor1.com',
  'https://attestor2.com',
  'https://attestor3.com',
]);

// counterpartyWalletDetails is the details of the Router Wallet.
const counterpartyWalletDetails: CounterPartyWalletDetails = JSON.stringify({
  counterpartyWalletURL: 'https://counterparty-wallet.com',
  counterpartyWalletName: 'Counterparty Wallet',
  counterpartyWalletIcon: 'https://counterparty-wallet.com/icon.png',
});

const requestParams: BitcoinContractRequestParams = {
  bitcoinContractOffer: bitcoinContractOffer,
  attestorURLs: attestorURLs,
  counterpartyWalletDetails: counterpartyWalletDetails,
};

const sendOfferToHiroWallet = async () => {
  window.btc
    .request('acceptBitcoinContractOffer', urlParams)
    .then((response) => {
      // Success! The user has accepted the offer.
      console.log(response.result.txId); // The broadcasted transaction ID.
      console.log(response.result.contractId); // The contract ID.
    })
    .catch((error) => {
      // Error! The user has declined the offer, us or an error has occurred.
      console.error(error.error.message); // The error message.
    });
};
```

The API will return an object which may optionally contain a contractId and a txId, both of which are strings.

```ts
interface BitcoinContractResponseBody {
  contractId?: string;
  txId?: string;
}
```

When the offer gets accepted, the response will include the broadcasted transaction ID (`txId`) and its corresponding contract ID (`contractId`). On the other hand, if the user turns down the offer, closes the window, operates on an unsupported network, or faces an error while accepting, you'll receive an error object detailing the specific error message.

You can use the `txId` to check the status of the transaction on the blockchain.

This function will pop up a window that will allow the user to review and accept the offer, and then sign and broadcast the contract.

<img src="https://drive.google.com/uc?id=1cjWdzsAzOmOI4Aw_xylKRvj_9kLOr44r"
     alt="Hiro Wallet Popup Window"
     style="display: block; margin-right: auto; margin-left: auto; width: 35%;" />
