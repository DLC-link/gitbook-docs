# Minting USDB and Repayment

## Minting&#x20;

A user connects a DLC-enabled wallet to Arkadiko’s site and navigates to the vaults section. The user determines the vault (loan) parameters which are sent to the DLC.Link oracle system, either through a web 2 api, or via a smart contract. These include the amounts, liquidation details, dates, etc. The user would likely also identify his/her STX wallet address as the destination for the stable coin to be minted.

The DLC.Link oracle exposes a DLC event announcement which has the payout outcomes programmed into it. This announcement value is used within the Arkadiko wallet to make a DLC offer to the user’s DLC-enabled wallet. The user accepts from within their wallet, and then the BTC transaction is put on the chain either by Arkadiko or the user. The user’s BTC is now locked into the DLC along with the Arkadiko Treasury Wallet and the liquidation price and payout outcomes are set in the payout curve of the contract. Depending on how things are configured with Arkadiko, the STX-based stable coin would either automatically be transferred to the user’s STX wallet, or the user would need to take an action on Stacks to retrieve the funds.&#x20;

## Repaying

To repay the loan and have the BTC returned, the user can navigate to their loan on the Arakadio site and send USDB to an Arkadiko smart contract which communicates with the DLC.Link smart contract, also on clarity. This way the smart contracts let the DLC.Link oracle server know (via an off-chain listener) that the debt is repaid and can attest to the corresponding “close event” which either the user or the Arkadiko wallet can obtain to unlock the BTC in the DLC to be returned to the user.&#x20;

## Diagram

![](<../../.gitbook/assets/Arkadiko + DLC.Link - Page 1 (1).png>)
