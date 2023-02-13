---
description: Our libraries make setting up your first DLC easy
---

# Installation and Setup

DLC.Link has libraries allowing for simple and fast configuration of DLCs in your product.

Depending on what type of product you're integrating into, we have a different library build just for you.

### Smart Contracts

#### Ethereum / Solidity

To use DLCs from your Ethereum contract, and let users move native bitcoin directly, import our contract and call the public functions as shown below.

````
```solidity
import "github.com/dlc-link/dlc-solidity-smart-contract/contracts/DLCManager.sol";
```

// Create your DLCManager instance, pointing to our public contract.
```solidity
DLCManager _dlcManager = DLCManager(publcDLCManagerContractAddress);
```

// 
```solidity
// createDLC: Creates the DLC in the DLC Manager contract, as well as in the Oracle network.
//
// @parameters
// emergencyRefundTime: The time at which the DLC can be cancelled, if any. Format: seconds from epoch
// nonce: An ID unique to this application to match the response in the callback.
bytes32 _uuid = _dlcManager.createDLC(emergencyRefundTime, nonce);
```

// 
```solidity
_dlcManager.mintBtcNft(_uuid, _collateral);
```

//
```solidity
 _dlcManager.closeDLC(_vault.dlcUUID, _payoutRatio);
```
````

#### Stacks / Clarity



### Bitcoin Wallets

### &#x20;Oracle Service
