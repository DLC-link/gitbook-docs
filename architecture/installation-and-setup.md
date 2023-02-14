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

### Oracle Service
