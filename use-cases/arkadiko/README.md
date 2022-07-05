# Arkadiko

## Overview

### Background

Background Arkadiko is a decentralized, non-custodial liquidity protocol where users can collateralize their STX tokens and borrow a stablecoin called USDA. It enables you to gain increased liquidity in the form of a soft-pegged US Dollar stablecoin, while maintaining original asset exposure. Your STX tokens generate a yield, which pays back the USDA loan automatically over time.

The proposed addition to the protocol would allow users to collateralize native Bitcoin to mint a soft-pegged US Dollar stablecoin. Similarly to USDA, this loan would be contained in an Arkadiko vault with similar terms and agreements around liquidation ratios and penalty fees. The benefit here is that a Discreet Log Contract (DLC) will be used to guarantee outcomes in market conditions where the Bitcoin price fluctuates and execute automatically to settle debt and repayment directly in BTC.

DLC.Link can provide part of the infrastructure and mechanism to create this BTC-backed stablecoin and handle management of the “locked” BTC to ensure the user and protocol are secured with Bitcoin payouts directly on the Bitcoin blockchain. DLC.Link integrates closely with the Stacks blockchain to provide DLC management controlled by transactions on the Stacks blockchain through the Clarity programming language. The price feed for BTC/USD would be fetched either using an oracle solution such as Redstone, or in later stages Chainlink price feeds.
