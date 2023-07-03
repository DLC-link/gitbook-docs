# DeFi Loan Example

### Components

At a high level, the most important parts of the DLC.Link technical systems architecture are the following:

* The Bitcoin Attestors, which act as mediators by signing the outcome for the DLC participants, ([learn more about DLCs](../../additional-resources/discreet-log-contracts.md))
* The smart contracts used by the DeFi protocol (e.g. Solidity)
* The DLC.Link Management Contract
* And the wallets that participate in the DLC

As an example of an implementation, consider a DeFi lender on Ethereum that loans stablecoins against native, on-chain Bitcoin.

### Open Loan Flow Example

<figure><img src="../../.gitbook/assets/DLC.Link_OpenLoan_TechnicalFlow_NewA5.png" alt=""><figcaption></figcaption></figure>

1. The user browses to the DeFi application's webpage and fills in the details of the loan they want to take, using BTC as collateral.
2. The DeFi application updates their smart contract on the blockchain to setup a new collateralized loan and calls to the DLC.Link smart contract to set up the Bitcoin DLC contract on the Bitcoin Attestors.
3. The details of the DLC from the Bitcoin Attestors are forwarded to the user, who can review and sign the DLC with their DLC-enabled Bitcoin wallet. The application also signs their side of the DLC using provided the DLC.Link protocol wallet service.
4. Once signed, the DLC transaction is submitted to the Bitcoin blockchain. The application's smart contract is notified once the DLC is confirmed.&#x20;
5. The application's smart contract now has a guarantee of BTC collateral and releases the stablecoin into the user's wallet.

### Close Loan via Repayment

<figure><img src="../../.gitbook/assets/DLC.Link_CloseLoan_TechnicalFlow_NewA (1).png" alt=""><figcaption></figcaption></figure>

Above a user wants to repay their stable-coin loan and regain access to their bitcoin collateral.&#x20;

1. The user navigates to the defi applications webpage and chooses to repay the loan.
2. The user initiates the sending of the stablecoin via the application's smart contract.
3. The application's smart contact communicates with the DLC.Link management smart contract to begin the closing of the DLC. The Bitcoin Attestors sign an outcome for the repayment (other possible outcome could have been liquidation).&#x20;
4. A callback to the application's smart contract notifies it that the DLC is ready to be closed, and the defi applications website is updated.
5. The application's protocol wallet puts the closing transaction on the BTC blockchain.
6. &#x20;The user can now see the funds in their BTC wallet, and the loan closing is complete.

### Close Loan via Liquidation

When the value of a user's collateral drops below a certain threshold, there can be a liquidation event. Here, a **liquidator** identifies the undercollateralized loan and starts a process to repay the loaned asset and 'buy out' the remaining collateral at a discount, thus keeping the value of the loaned asset stable.

<figure><img src="../../.gitbook/assets/DLC.Link_SimpleLiquidationFlow.png" alt=""><figcaption></figcaption></figure>
