# Bitcoin Contract Interface

To activate DLC-related features in your Bitcoin wallet application, it's necessary to integrate and install the 'dlc-wasm-wallet' package. This is a fundamental step
towards interacting with Bitcoin Contracts.

## Instantiaton

```ts
import { JsDLCInterface } from 'dlc-wasm-wallet';
```

Before utilizing the DLC interface to create a DLC, you must instantiate it with the following information: the user's private key, address, the Bitcoin network, its API
endpoint, and the URLs for the attestors' endpoints.

```ts
interface JsDLCInterface {
  new (
    userPrivateKey: string, // The user's private key.
    userAddress: string, // The user's address.
    bitcoinNetwork: string, // The Bitcoin network.
    bitcoinNetworkAPI: string, // The Bitcoin network API endpoint.
    attestorURLs: string // An array of attestor API endpoints (string[]), which has been converted into a JSON string.
  ): JsDLCInterface;
}
```

Ensure that all the necessary fields are JSON-stringified before utilizing them as param, as the interface operates using JSON format.

To instantiate the interface, use the function new() as follows:

```ts
const bitcoinContractInterface = JsDLCInterface.new(
  userPrivateKey,
  userAddress,
  bitcoinNetwork,
  bitcoinNetworkAPI,
  attestorURLs
);
```

## Bitcoin Contract Interface Functions

After instantiating the interface, you can utilize the following functions to accept, sign and broadcast Bitcoin Contracts.

### Accept Offer

To accept a Bitcoin Contract Offer, you must utilize the accept_offer() function, which accepts a bitcoin contract offer in JSON format as a param.

```ts
const bitcoinContractJSON = JSON.stringify(bitcoinContractOffer);

const acceptedBitcoinContract = await bitcoinContractInterface.accept_offer(bitcoinContractJSON);
```

### Send Accepted Offer to Router Wallet

Before signing and broadcasting a Bitcoin Contract, you must send the accepted Bitcoin Contract Offer to the counterparty's wallet. To do so, you should ulitize a function
similar to the following:

```ts
async function sendAcceptedBitcoinContractOfferToRouterWallet(
  acceptedBitcoinContractOffer: string,
  counterpartyWalletURL: string
) {
  return fetch(`${counterpartyWalletURL}/offer/accept`, {
    method: 'put',
    body: JSON.stringify({
      acceptMessage: acceptedBitcoinContractOffer,
    }),
    headers: { 'Content-Type': 'application/json' },
  }).then((res) => res.json());
}

const signedBitcoinContract = await sendAcceptedBitcoinContractOfferToRouterWallet(
  acceptedBitcoinContract,
  counterpartyWalletURL
);
```

### Sign and Broadcast

To sign and broadcast a Bitcoin Contract, you must utilize the countersign_and_broadcast() function, which accepts a signed bitcoin contract in JSON format as a param.

```ts
const signedBitcoinContractJSON = JSON.stringify(signedBitcoinContract);

const txID = await bitcoinContractInterface.countersign_and_broadcast(signedBitcoinContractJSON);
```

After signing and broadcasting a Bitcoin Contract, you can use the received transaction ID to view the transaction on a block explorer.
