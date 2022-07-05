# Minting USDB and Liquidation

## Minting

The mint process is identical as described in the [previous scenario](https://app.gitbook.com/s/4HE0RJ2niQGWGQHbIHC8/\~/changes/Pk4WS7X8uJRuIh50gE1B/use-cases/arkadiko/minting-usdb-and-repayment).

## Liquidation

In the event that the price of BTC decreases to a point where the collateralization ratio falls below the threshold, the DLC.Link oracle attests to the price of BTC which triggers the ability to close the DLC. The price is fetched from a price feed oracle service such as Redstone/Chainlink. The locked BTC is sent back, in part, to both the user and to Arkadiko’s BTC wallet as agreed upon during origination for this BTC price.

## Diagram

![Partial loan collateral returned to borrower; remaining is atomic swapped and added to the liquidation pool.](<../../.gitbook/assets/Arkadiko + DLC.Link - Page 2.png>)
