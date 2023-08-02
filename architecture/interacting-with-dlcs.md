# Sign DLC contracts

The Bitcoin Contract Interface enables users to receive, accept, and sign DLCs (Discreet Log Contracts) offered by another party, all within the user's wallet.

## Solidity / Clarity Smart Contracts

To enable the utilization of DLCs and enable users to transact with native bitcoin directly, it is essential to acquaint yourself with either DLC.Link's Clarity or
Solidity contract.

- Solidity: https://github.com/DLC-link/dlc-solidity
- Clarity: https://github.com/DLC-link/dlc-clarity

To interact with our DLC Manager contract, you need to create your own smart contract that communicates with the DLC Manager contract. You can write this contract using either the Solidity language for deployment on the Ethereum blockchain or the Clarity language for deployment on the Stacks blockchain. Once you deploy your contract, please share it's address with our developers. This way, we can whitelist it and allow it to interact with our DLC Manager contract.

## Router Wallet

To enable DLC creation as a counterparty, you have two options: either run your own Router Wallet or connect to an existing one. The Router Wallet plays a crucial role as it communicates with both the dlc-manager and the attestors. It offers an API that allows you to create and manage transactions. When you have your own Router Wallet set up, other users can fetch offers from your wallet and accept them.

Once you have set up your Router Wallet, please share its address with our developers. By doing so, we can whitelist it, granting permission for it to interact with our DLC Manager contract.

### Integrating smart contract functionality into your dApp

To interact with the smart contracts (Solidity or Clarity) in you dApp, you should import and install the corresponding package(s) into your project.
For Solidity, you should import the 'ethers.js' or 'web3.js' package, and for Clarity, you should import the 'stacks.js' package.

- ethers.js: https://docs.ethers.org/v5/
- web3.js: https://web3js.readthedocs.io/en/v1.10.0/
- stacks.js: https://www.hiro.so/stacks-js

  After installing the necessary package(s), the next is to setup a function that will enable the user's wallet to interact with the dApp, and the smart contract(s) within it. This step should also set the desired network.

#### Connect Wallet | Set Provider and Contracts / Solidity Example

```jsx
<li>
  <button onClick={() => connectMetaMaskAccount(EthereumNetworks[0].id)}>Mainnet</button>
  <button onClick={() => connectMetaMaskAccount(EthereumNetworks[1].id)}>Goerli</button>
  <button onClick={() => connectMetaMaskAccount(EthereumNetworks[2].id)}>Sepolia</button>
</li>
```

```ts
import { ethers } from 'ethers';

const EthereumNetworks = {
  Mainnet: {
    id: '1',
    protocolContractAddress: env.MAINNET_PROTOCOL_CONTRACT_ADDRESS,
    usdcContractAddress: env.MAINNET_USDC_ADDRESS,
  },
  Goerli: {
    id: '5',
    protocolContractAddress: env.GOERLI_PROTOCOL_CONTRACT_ADDRESS,
    usdcContractAddress: env.GOERLI_USDC_ADDRESS,
  },
  Sepolia: {
    id: '11155111',
    protocolContractAddress: env.SEPOLIA_PROTOCOL_CONTRACT_ADDRESS,
    usdcContractAddress: env.SEPOLIA_USDC_ADDRESS,
  },
};

const [currentAccount, setCurrentAccount] = useState();
const [currentNetwork, setCurrentNetwork] = useState();
const [protocolContract, setProtocolContract] = useState();
const [usdcContract, setUsdcContract] = useState();

async function connectMetaMaskAccount(blockchain: string): Promise<void> {
  const { ethereum } = window;

  if (!ethereum) return alert('Install MetaMask!');

  const metamaskAccounts = await ethereum.request({
    method: 'eth_requestAccounts',
  });

  setCurrentAccount(metamaskAccounts[0]);
  setCurrentNetwork(blockchain);

  await setEthereumProvider();
}
```

To set the Ethereum provider and contracts, you should have the contracts' ABI and addresses, which you can get from the smart contract's repository.

```ts
async function setEthereumProvider(): Promise<void> {
  const { protocolContractAddress, usdcContractAddress } = EthereumNetworks[currentNetwork];
  const { ethereum } = window;
  const provider = new ethers.providers.Web3Provider(ethereum);
  const signer = provider.getSigner();
  const { chainId } = await provider.getNetwork();

  if (chainId !== currentNetwork) {
    await changeEthereumNetwork();
  }
  const currentProtocolContract = new ethers.Contract(protocolContractAddress, protocolContractABI, signer);
  const currentUsdcContract = new ethers.Contract(usdcContractAddress, usdcABI, signer);

  setProtocolContract(currentProtocolContract);
  setUsdcContract(currentUsdcContract);
}
```

Make sure to wrap the functions in a try/catch block to handle errors.

To utilize the smart contract functions, you should store the user's address, the network, and the contracts in the state or in a store, and then use them to call the functions.

#### Create Vault / Solidity Example

To create a vault on Ethereum, you should utilize a function like the example below.

Our protocol contract has a function called 'setupLoan' that takes the following parameters:

BTCDeposit: The amount of BTC the user is willing to deposit.
liquidationRatio: The ratio at which the vault will be liquidated.
liquidationFee: The fee that will be paid to the liquidator.
attestorCount: The number of attestors that will be used to create a DLC.

Note that you should should unshift the decimal point of the liquidationRatio and liquidationFee by 2 places, and shift the BTCDeposit by 8 places.

```ts
async function createVault(
  BTCDeposit: number,
  liquidationRatio: number,
  liquidationFee: number,
  attestorCount: number
) {
  protocolContract.setupLoan(BTCDeposit, liquidationRatio, liquidationFee, attestorCount);
}
```

This function will ping the DLC Manager contract, and create a vault on the Ethereum blockchain.

After the vault is created, and the transaction is confirmed. The vault's status will be set to 'Ready', and the user will be able to receive and accept offers.

#### Close Vault / Solidity Example

To close a vault on Ethereum, you should utilize a function like the example below.

Our protocol contract has a function called 'closeLoan' that takes the UUID of the vault as a parameter.

First it is necessary to fetch the vault by the UUID, and then pass the vault's ID to the 'closeLoan' function.

```ts
async function closeVault(vaultUUID: string) {
  const loan = await getEthereumLoanByUUID(UUID);
  protocolContractETH.closeLoan(parseInt(loan.id._hex));
}
```

## Fetching Bitcoin Contract Offer from the Router Wallet

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

To accept the received offer and engage with a Bitcoin wallet, you need to communicate with the wallet's API in the following manner:

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

```ts
const urlParams: BitcoinContractRequestParams = { ...params };

const result = await window.btc.request('acceptBitcoinContractOffer', urlParams);
```

This function will pop up a window that will allow the user to review and accept the offer, and then sign and broadcast the contract.

<img src="https://drive.google.com/uc?id=1cjWdzsAzOmOI4Aw_xylKRvj_9kLOr44r"
     alt="Hiro Wallet Popup Window"
     style="display: block; margin-right: auto; margin-left: auto; width: 35%;" />
