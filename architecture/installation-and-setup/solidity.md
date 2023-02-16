# Solidity

### Smart Contracts

#### Ethereum / Solidity

To use DLCs from your Ethereum contract, and let users move native bitcoin directly, import our contract and call the public functions as shown below.

Note: to always get the most up to date information about the library, please see the README of the repo here: [https://github.com/DLC-link/dlc-solidity](https://github.com/DLC-link/dlc-solidity)

```solidity
import "github.com/dlc-link/dlc-solidity/contracts/DLCManager.sol";

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
