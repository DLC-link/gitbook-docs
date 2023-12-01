# Ordinals in DLCs

## Ordinal Support

DLC.Link is implementing support for the locking of an ordinal as part of the collateral of a DLC.

Notes:

* An ordinal is a unique numeric reference to a single satoshi, usually one that has had some content permanently inscribed onto it. However, that satoshi is often part of a larger UTXO. DLCs lock UTXOs, so please keep that context in mind.
* In all cases, the transaction fee to lock and later unlock the DLC is split between both parties.

### Supported types of DLCs

#### DLC with only the ordinal

This package supports the use case when the offering party of the DLC locks the ordinal-containing UTXO into the DLC, and the counter-party (the acceptor) does not lock any collateral.

In this case, the outcome is essentially binary, either the ordinal containing UTXO goes to the offeror, or the acceptor.

#### Ordinal with additional collateral

This package also supports the case when the offering party of the DLC locks the ordinal-contianing UTXO into the dlc, AND any amount of addition collateral is locked into the DLC by either party.

1. In this case, the outcome of the UTXO containing the ordinal will go to either party A or B, without resizing (as described earlier)
2. The remaining collateral locked by either, or both, parties can be split between the parties based on the standard DLC enumerated or numerical outcome guidelines.

### How it works

In order to ensure that the ordinal does not get lost to fee, the DLC transactions for DLCs including an ordinal are created in the following way:

* The ordinal input is always set as the first one in the funding transaction,
* The funding output is always set as the first one in the funding transaction,
* The party getting the ordinal will always have its CET output in the first position.

#### Locking an ordinal in the DLC

To lock an ordinal, an `OrdDescriptor` must be used. This descriptor contains information about the ordinal being included in the DLC (location in the blockchain and the transaction that includes is), as well as information about the event upon which the DLC is based.

Event outcomes can be, as for regular contracts, enumerated or numerical.

#### Enumerated events

Ordinal DLCs based on enumerated events include, in addition to the regular enumerated event information, an array of boolean indicating for each possible outcome whether the ordinal should be given to the _offer_ party (note this means that this array _must_ have the same number of elements as there are possible outcomes).

#### Numerical events

Ordinal DLCs based on numerical events include, in addition to the regular numerical event information, an array or intervals. These intervals indicate the ranges of outcomes for which the ordinal should be given to the offer party.

### Limitations

#### Changing postage

It is currently not possible to change the postage of the ordinal.

This means that if an ordinal is contained in a 1BTC UTXO, the entire 1BTC will be included in the DLC, and given to the ordinal winner without changing the sats in the UTXO.

Changing the postage should thus be done prior to including the ordinal in a DLC by the wallet.

In addition, the postage of the ordinal will be merged with the payout of the winner.

This means that if a party is given an ordinal with a postage of 1BTC in addition to a payout of 1BTC, the output of the CET including the ordinal will have a value of 2BTC.
