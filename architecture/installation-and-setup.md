---
description: Our libraries make setting up your first DLC easy
---

# Installation and Setup

DLC.Link has libraries allowing for simple and fast configuration of DLCs in your product.

Depending on what type of product you're integrating into, we have a different library build just for you.

### Smart Contracts

#### Ethereum / Solidity

To use DLCs from your Ethereum contract, and let users move native bitcoin directly, import our contract and call the public functions as shown below.

```solidity
import "github.com/dlc-link/dlc-solidity-smart-contract/contracts/DLCManager.sol";

// In your constructor, create your DLCManager instance, pointing to our public contract.
DLCManager _dlcManager = DLCManager(publcDLCManagerContractAddress);

// createDLC: Creates the DLC in the DLC Manager contract, as well as in the Oracle network.
//
// @parameters
// emergencyRefundTime: The time at which the DLC can be cancelled, if any. Format: seconds from epoch
// nonce: An ID unique to this application to match the response in the callback.
//
// @return
// UUID (bytes32): The UUID of the DLC.
bytes32 dlcUUID = _dlcManager.createDLC(emergencyRefundTime, nonce);

// If desired, mint an NFT representing the Bitcoin contract which can be transfered and borrowed against.
_dlcManager.mintBtcNft(dlcUUID);

// Close the DLC, which sends the collateral out of the Bitcoin DLC at the given ratio.
//
// @parameters
// payoutRatio: Number between 0 and 100 representing the % of bitcoin paid out from the DLC to the original participants
//              0 means the user gets all the collateral, 100 means this contract gets all the collateral.
 _dlcManager.closeDLC(dlcUUID, payoutRatio);
```

#### Stacks / Clarity



### Bitcoin Wallets

### &#x20;Oracle Service
