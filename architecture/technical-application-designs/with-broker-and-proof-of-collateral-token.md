# With Liquidator and Proof of Collateral NFT

DLC.Link has designed a token standard using ERC-721 (NFT) or ERC-1155 (Semi-fungible token) tokens which represent Bitcoin locked in the DLC escrow. This type of token is often referred to as a **Proof of Reserve** or **Proof of Collateral** token.

This **Proof of Collateral** token implements the "DLC-open" and "DLC-close" functions in the token's mint and burn functions, respectively. Unlike wrapped Bitcoin (wBTC) or its variants, the token itself doesn't hold value. Instead, it provides cryptographic proof that funds are locked on Bitcoin chain. As such, the token is much **safer** than a traditional wrapped Bitcoin.

The token can be redeemed via a "peg-out" service provided by a liquidator, as described in the preceding sections.

### Setup Loan Flow with Proof of Collateral Token

Below is a diagram showing how a PoC token is minted by a smart contract when a user locks their Bitcoin into a DLC. Once minted, the PoC token represents a guarantee that funds exist and can't be moved. The PoC token is then sent to a defi platform to be used as collateral.

When the borrower wants to access their Bitcoin again, they simply repay the funds, get their PoC token back, and burn it, which unlocks the DLC and their collateral is returned.

<figure><img src="../../.gitbook/assets/DLC.Link_POCToken_Open.png" alt=""><figcaption></figcaption></figure>

1. Borrowing user browses to the DeFi app and sets up the details of the desired loan.
2. They are directed to a bridging application, which may be part of the DeFi app, directly part of DLC.Link, or a 3rd party. Here they set up the details of the DLC and identify their Bitcoin collateral.
3. The DLC is set up via the DLC.Link smart contracts and Bitcoin Attestors.
4. The user accepts the details of the DLC in their Bitcoin wallet.
5. The Bitcoin is locked into the DLC.
6. The bridging application from step 2 mints the Proof of Collateral tokens representing the collateral.
7. The user is directed back to the DeFi lending app where they can use the PoC tokens as collateral.
8. The user sends the PoC tokens to the defi application's smart contract to borrow stablecoin.
9. The user's stablecoin loan is issued to their wallet.

### Liquidation Flow with Proof of Reserve Token

In the liquidation case, the user's NFT has been moved to another party. This can happen if the user traded their NFT, or if their NFT was seized as a consequence of defaulting on a loan.

When the token holder wants to redeem the Bitcoin collateral, they need to use a liquidation service associated whitelisted by the protocol that originally minted the token.

<figure><img src="../../.gitbook/assets/DLC.Link_POCToken_Liquidation.png" alt=""><figcaption></figcaption></figure>

1. 3rd party liquidator(s) send payment to the DeFi protocol to cover the loan.
2. The Proof of Collateral tokens are sent from the defi protocol to the liquidator(s).
3. The PoC tokens are burned, which is picked up by the DLC.Link smart contract.
4. The DLC is closed with the outcome associated with the liquidation event. Specifically, what % of collateral should go to the liquidator(s), and what % should go to the original borrower.
5. The DLC is closed, releasing the BTC to the liquidator and the borrower.
6. The liquidator pays out the liquidator(s) depending on their pre-negotiated terms.
