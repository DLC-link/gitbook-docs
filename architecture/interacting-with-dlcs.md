# Interacting with Bitcoin Contracts in the Hiro Wallet

## Overview
This documentation will show dApp developers how to access Bitcoin Contrcts in their applications, via the Hiro wallet.

Enabling the interfacing of Bitcoin Contracts via Hiro wallet requires three steps:

  1. Integrate your smart contract with the *DLC Manager* smart contract
  2. Run a Router Wallet
  3. Configure your web app to support Bitcoin Contracts, which has two parts:
     1. Configure your web app to interact with the Router wallet
     2. And pop up the Hiro wallet's Bitcoin Contract interface

See the below sections for more details on each step.

*Note* Bitcoin Contracts are powered by the underlying technology known as DLCs (Discreet Log Contracts) and sometimes the names are used interchangibly. Learn more about DLCs here (http://www.dlc.link)

## Step 1. Solidity / Clarity Smart Contract Integration

To enable the use of DLCs, which let users transact with native bitcoin directly, it is essential to acquaint yourself with either DLC.Link's Clarity or
Solidity contract. These are referred to as the DLC Manager contracts. Calling into the DLC Manager contract happens directly from your smart contract. See the following instructions for more details:

- Solidity: https://github.com/DLC-link/dlc-solidity/tree/attestor-manager ----- Link to the docs page
- Clarity: https://github.com/DLC-link/dlc-clarity/tree/attestor-manager ----- Link to the docs page

### Registering Your Smart Contract
As the DLC.Link product is currently in the beta-testing phase, you must first register your contract with the DLC.Link development team in our Discord server.
Once you deploy your contract, please share it's address with the DLC.Link developers to have it whitelisted. Please contact DLC.Link on our Discord server.

## Router Wallet

<!-- what is a router wallet? What is a counter-party? -->

To enable DLC creation, there needs to be a counter-party bitcoin wallet in the Bitcoin Contract. The Router Wallet plays a crucial role as it communicates with both the dlc-manager and the attestors.
 <!-- It offers an API that allows you to create and manage transactions.  //What does this mean? what kinds of transactions? it's the first time you used the term transactions, people won't know what it is.-->
 When you have your own Router Wallet set up, other users can fetch offers from your wallet and accept them.
 <!-- other users? you mean the protocol's web-app? -->

Once you have set up your Router Wallet, please share its address with the DLC.Link developers, as this also needs to be whitelisted during beta-testing. By doing so it will be granted permission for it to interact with our DLC Manager contract.

## Fetching Bitcoin Contract Offer from the Router Wallet
OK, now it's time to configure your web app!

DLC offer? accept?

<!-- what is a vault? -->
After a Vault has been created on Ethereum, the UUID can be used to fetch a Bitcoin Contract Offer from the Router Wallet.

To receive a Bitcoin Contract Offer from the Router Wallet, you should utilize a function like the example below:

```ts
export const fetchBitcoinContractOfferFromCounterpartyWallet = async () => {
  // vaultContract is the vault object from either the Solidity or Clarity smart contract.

  const counterpartyWalletURL: string = 'https://counterparty-wallet.com/offer'; // The Router Wallet Offer API endpoint.

  const uuid: string; // The UUID of the vault contract.
  const acceptCollateral: Number; // The amount of collateral the user is willing to deposit.
  const offerCollateral: Number; // The amount of collateral the counterparty is willing to deposit.
  const totalOutcomes: Number; // The total number of outcomes of the DLC.
  const attestorList: string; // An array of attestor API endpoints (string[]), which has been converted into a JSON string.
```

All the necessary fields come from the vault, except the totalOutcomes which can be set here.

```ts
  try {
    const response = await fetch(counterpartyWalletURL, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        uuid,
        acceptCollateral,
        offerCollateral,
        totalOutcomes,
        attestorList,
      }),
    });
    const responseStream = await response.json();
    if (!response.ok) {
      console.error(response);
      return;
    }
    return responseStream;
  } catch (error) {
    console.error(error);
  }
};
```

The function above will return a Bitcoin Contract Offer in string format, which you can then utilize to accept the offer in the next step.

## Wallet API

To accept the received offer and engage with a Hiro wallet, you need to communicate with the Hiro wallet's API in the following manner:

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
<!-- This part seems very important, but is not clear at all. What are the two above types, the CounterPartyWalletDetails and the BitcoinContractRequestParams, where are they used? What are the 'params' below?-->
```ts
const urlParams: BitcoinContractRequestParams = { ...params };

const result = await window.btc.request('acceptBitcoinContractOffer', urlParams);
```

This function will pop up a window that will allow the user to review and accept the offer, and then sign and broadcast the contract.

<img src="https://drive.google.com/uc?id=1cjWdzsAzOmOI4Aw_xylKRvj_9kLOr44r"
     alt="Hiro Wallet Popup Window"
     style="display: block; margin-right: auto; margin-left: auto; width: 35%;" />
