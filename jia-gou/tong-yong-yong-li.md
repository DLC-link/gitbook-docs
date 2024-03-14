---
description: DLCs的潜在用例的运行列表
---

# Solidity

### 智能合约

#### Ethereum / Solidity

要使用以太坊合约中的DLCs，并让用户直接移动本地比特币，请导入我们的合约并调用公共函数，如下所示。&#x20;

注意:要始终获得有关函数库的最新信息，请参阅此处的repo的README: [https://github.com/DLC-link/dlc-solidity](https://github.com/DLC-link/dlc-solidity)

```solidity
import "github.com/dlc-link/dlc-solidity/contracts/DLCManager.sol";

// In your constructor, create your DLCManager instance, pointing to our public contract.
DLCManager _dlcManager = DLCManager(publcDLCManagerContractAddress);

// createDLC: Creates the DLC in the DLC Manager contract, as well as in the Attestor network.
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
