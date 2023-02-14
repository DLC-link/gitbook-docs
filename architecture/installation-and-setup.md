---
description: Our libraries make setting up your first DLC easy
---

# Installation and Setup

DLC.Link has libraries allowing for simple and fast configuration of DLCs in your product.

Depending on what type of product you're integrating into, we have a different library build just for you.

### Smart Contracts

#### Ethereum / Solidity

To use DLCs from your Ethereum contract, and let users move native bitcoin directly, import our contract and call the public functions as shown below.

Note: to always get the most up to date information about the library, please see the README of the repo here: [https://github.com/DLC-link/dlc-solidity-smart-contract](https://github.com/DLC-link/dlc-solidity-smart-contract)

```solidity
import "github.com/dlc-link/dlc-solidity-smart-contract/contracts/DLCManager.sol";

// In your constructor, create your DLCManager instance, pointing to our public contract.
DLCManager _dlcManager = DLCManager(publcDLCManagerContractAddress);

// createDLC: Creates the DLC in the DLC Manager contract, as well as in the Oracle network.
//
// @parameters
// emergencyRefundTime: The time at which the DLC can be cancelled, if any. Format: seconds from epoch
// nonce: An ID unique to this application to match the response in the callback.
bytes32 dlcUUID = _dlcManager.createDLC(emergencyRefundTime, nonce);

// Overwrite this function to complete your DLC setup logic
function postCreateDLCHandler(bytes32 uuid) external;

// Overwrite this function to run custom logic when the DLC has been funded
function setStatusFunded(bytes32 uuid) external;

// If desired, mint an NFT representing the Bitcoin contract which can be transfered and borrowed against.
_dlcManager.mintBtcNft(dlcUUID);

// Overwrite this function to complete the mintNFT logic
function postMintBtcNft(bytes32 uuid, uint256 nftId) external;

// Overwrite this function to receive the price of BTC when closing the DLC
function getBtcPriceCallback(bytes32 uuid, int256 price, uint256 timestamp) external;

// Close the DLC, which sends the collateral out of the Bitcoin DLC at the given ratio.
//
// @parameters
// payoutRatio: Number between 0 and 100 representing the % of bitcoin paid out from the DLC to the original participants
//              0 means the user gets all the collateral, 100 means this contract gets all the collateral.
 _dlcManager.closeDLC(dlcUUID, payoutRatio);
 
 // Overwrite this function to complete the close DLC logic
function postCloseDLCHandler(bytes32 uuid) external;
```

#### Stacks / Clarity

The Clarity contract bringing DLC.Link functionality to Stacks can be found in [this repository.](https://github.com/DLC-link/stacks-contracts-all)

The best way to understanding integration with our Clarity smart contracts is by reading our [example protocol-contract](https://github.com/DLC-link/stacks-contracts-all/blob/main/examples/sample-contract-loan.clar).

Due to the limitations of Stacks nodes, currently we must manually register contracts that wish to communicate with ours. Our `register-contract` function is picked up by the DLC.Link Blockchain Observer, thus allowing us to hear and process external calls of our functions correctly. Please contact us so that we can do the necessary steps.

Contracts wishing to communicate with ours must implement the following [dlc-link-callback-trait](https://github.com/DLC-link/stacks-contracts-all/blob/main/contracts/dlc-link-callback-trait.clar) to enable the proper callback functionalities:
```clarity
(use-trait cb-trait 'ST1PQHQKV0RJXZFY1DGX8MNSNYVE3VGZJSRTPGZGM.dlc-link-callback-trait-v2.dlc-link-callback-trait)
(impl-trait 'ST1PQHQKV0RJXZFY1DGX8MNSNYVE3VGZJSRTPGZGM.dlc-link-callback-trait-v2.dlc-link-callback-trait)
```
This requires implementing the following functions:
```clarity
(post-create-dlc-handler (uint (buff 32)) (response bool uint)) ;; called after succesful DLC creation
(post-close-dlc-handler ((buff 32)) (response bool uint))       ;; called after succesful DLC closing
(get-btc-price-callback (uint (buff 32)) (response bool uint))  ;; a function to pass back validated BTC price fetched from Redstone
(set-status-funded ((buff 32)) (response bool uint))            ;; called when BTC is successfully locked in the DLC on the Bitcoin blockchain
```

To request the creation of a DLC, call our contract like so (`target` is the callback-contract's address, `loan-id` is some local identifier for the requested DLC):
```clarity
(unwrap! (ok (contract-call? 'ST1PQHQKV0RJXZFY1DGX8MNSNYVE3VGZJSRTPGZGM.dlc-manager-priced-v0-1 create-dlc emergency-refund-time target loan-id)) err-contract-call-failed)
```

And similarly, to request a simple close with a DLC `UUID` and an `outcome` (0 means full refund to Accept party, 100 means full payment to Offer party. it has two decimals precision):
```clarity
(unwrap! (ok (as-contract (contract-call? 'ST1PQHQKV0RJXZFY1DGX8MNSNYVE3VGZJSRTPGZGM.dlc-manager-priced-v0-1 close-dlc uuid u0))) err-contract-call-failed)
```

We also offer BTC price fetching and validation through redstone through the following function:
```clarity
(unwrap! (ok (as-contract (contract-call? 'ST1PQHQKV0RJXZFY1DGX8MNSNYVE3VGZJSRTPGZGM.dlc-manager-priced-v0-1 get-btc-price uuid))) err-contract-call-failed)
```
This will return values through the above mentioned `get-btc-price-callback` function.

### Bitcoin Wallets

Signing a DLC requires functionality not always present in existing Bitcoin wallets on the market. To fulfill this requirement, DLC.Link will provide open-source libraries in the Rust and JavaScript programming languages.

DLC transactions are handled by two parties, an offeror and an acceptor. In our current setup, the Bitcoin wallet acts as the acceptor and the dApp acts as the offeror. It is the Bitcoin wallet that handles accepting signatures, building and signing Contract Execution Transactions (CETs), and broadcasting onto the BTC blockchain. 

There are a few components required to support the dlc-lib library.

Storage

To use the Javascript library it requires Storage. The purpose of this is to store the contracts. This can be local, chrome or your own cloud storage.

```typescript
const storage = new ContractRepository();
```
It is recommended that you create a storage class that implements the following interface.
```typescript
export interface ContractRepository {
    createContract(contract: AnyContract): Promise<void>;
    getContract(contractId: string): Promise <AnyContract>;
    getContracts(query?: ContractQuery): Promise<AnyContract[]>;
    updateContract(contract: AnyContract): Promise<void>;
    deleteContract(contractId: string): Promise<void>;
    hasContract(contractId: string): Promise<boolean>;
}
```

Blockchain Communication

To interact with the Bitcoin network in the context of a Discreet Log Contract, a protocol implementation is required. A bitcoin client facilitates connection to servers that validate transactions and blocks on the Bitcoin blockchain, allowing for interaction with the network.
```typescript
const blockchain = new Blockchain();
```
The following interface should be implemented by a Blockchain class.
```typescript
export interface Blockchain {
    getTransaction(txid: string): Promise<string>;
    sendRawTransaction(txHex: string): Promise<void>;
    getUtxosForAddress(address: string): Promise<Utxo[]>;
    isOutputSpent(txid: string, vout: number): Promise<boolean>;
}
```
Wallet
##
 In a Discreet Log Contract (DLC) flow, certain information and functionality provided by a wallet, such as the user's address, public and private keys, unspent transaction outputs (UTXOs), and Schnorr signing capabilities, are necessary for the DLC process. This information can be obtained through the use of a wallet object.
 ```typescript
const wallet = new Wallet();
```
The following interface should be implemented by a wallet class.

 ```typescript
export interface Wallet {
    // Get the bitcoin address of the user
  getAddress(): Promise<string>
    // Get the public key of the user
  getPublicKey(): Promise<string>
    // Get the private key associated with the given public key.
  getPrivateKeyForPublicKey(publicKey: string): Promise<string>
  // Get a set of utxos whose total value is at least `amount`. If successful, the utxos are marked as reserved.
  getUtxosForAmount(amount: number, feeRatePerVByte: number): Promise<Utxo[]>
  // Returns a DER encoded signature for the specified input using the private key associated with the given public key.
  getDerTxSignatureFromPubkey(
    tx: Transaction,
    inputIndex: number,
    inputAmount: number,
    pubkey: string,
    outputScript: string
  ): Promise<string>
  // Creates a signature for the given input and fills the transaction input witness.
  signP2WPKHTxInput(
    tx: Transaction,
    inputIndex: number,
    inputAmount: number,
    inputAddress: string
  ): Promise<void>
  // Unreserves a Utxo that was previsously marked as reserved.
  unreserveUtxo(txid: string, vout: number): Promise<void>
  // Returns the network targeted by the wallet
  getNetwork(): Network
}
 ```
Contract Updater

  ```typescript
const contractUpdater = new ContractUpdater(wallet, blockchain);
```
The ContractUpdater class is a utility class that is used to manage contracts on a blockchain network. It has several methods that help in preparing and creating DLC contracts, by getting all the inputs, payouts, and transaction details required for the contract, and using the wallet and blockchain instances to interact with the network. 

DLC Manager

  ```typescript
const dlcManager = new DlcManager(contractUpdater, storage);
```

The DlcManager class is a utility class that is used to manage the life cycle of decentralized lending and borrowing contracts on a blockchain network. It has several methods that handle different stages of the contract life cycle, such as creating, accepting, signing, and rejecting contracts. 

DLC Service

All communication is handled through the DLCService interface (which is an implementation of DlcAPI) All The functionality we need is wrapped up in it. 

  ```typescript
export class DlcService implements DlcAPI {
  constructor(
    readonly dlcManager: DlcManager,
    readonly contractRepository: ContractRepository
  ) {}
  getAllContracts(): Promise<AnyContract[]> {
    return this.contractRepository.getContracts()
  }
  processContractOffer(offer: string): Promise<AnyContract> {
    const offerMessage = offerMessageFromJson(offer)
    return this.dlcManager.onOfferMessage(offerMessage)
  }
  processContractSign(sign: string): Promise<AnyContract> {
    return this.dlcManager.onSignMessage(JSON.parse(sign))
  }
  getContract(contractId: string): Promise<AnyContract> {
    return this.contractRepository.getContract(contractId)
  }
  acceptContract(contractId: string): Promise<AnyContract> {
    return this.dlcManager.acceptOffer(contractId)
  }
  rejectContract(contractId: string): Promise<AnyContract> {
    return this.dlcManager.onRejectContract(contractId)
  }
}
  ```
  
Bitcoin Lending Flow

Step 1: Receive Offer

An offer will be received once the contract is set up in the dApp, sent to the dApp’s DLC-enabled BTC wallet (offeror’s wallet), validated, and signed by the offeror. An offer will include a contract that already contains an Oracle announcement and offeror parameters. 

The acceptor’s wallet will receive the offer requests initiated by the contract handling dApp. Following that, we hand the offer to the DlcService.
  ```typescript
// Get an offer from the backend.
  const resp = await axios.get<OfferMessage>(HOST + NEWOFFER + type, {
    transformResponse: [
      (data) => {
        return offerMessageFromJson(data)
      },
    ],
  })

  //Retrieve the offer from the received data object
  const offer = resp.data

  // Process the offer.
  const offeredContract = await dlcService.processContractOffer(offer);
  ```
processContractOffer returns an OfferedContract and stores it in the Storage. 

Step 2: Accept Offer

The acceptContract function of DlcService is invoked with a temporary contract id when the user accepts the contract.
  ```typescript
  // Accept the offer.
  const accept = await dlcService.acceptContract(offeredContract.temporaryContractId);
  ```
After acceptance, DlcService retrieves the stored contract from the repository, checks for sufficient utxos, collects them, creates a refund signature and constant ID, and gets all CETs signed by parties and Oracle(s), then it returns this extended contract to the Storage so that it can update the stored version accordingly.

Step 3: Sign

After accepting the contract, it should be returned to the dApps' wallet and a signed acceptance message should be received in response.
  ```typescript
  //Get the sign message from the backend wallet.
  const sign = (await axios.post<SignMessage>(HOST + POSTACCEPT, accept)).data;

  //Process the sign message.
  await dlcService.processContractSign(sign)
  ```
Then it should pass this message to the assigned processContractSign function of the DlcService. This function handles signing and verifying the contract signatures, and updates the contract information in storage.

Step 4: Broadcast

Once all necessary signatures and verifications have been completed, the contractUpdater sends the transaction to the blockchain.










### Oracle Service
