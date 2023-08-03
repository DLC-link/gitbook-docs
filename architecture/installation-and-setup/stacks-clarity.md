# Stacks / Clarity

The Clarity contract bringing DLC.Link functionality to Stacks can be found in [this repository.](https://github.com/dlc-link/dlc-clarity)

The best way to understanding integration with our Clarity smart contracts is by reading our [example protocol-contract](https://github.com/DLC-link/dlc-clarity/blob/main/examples/sample-contract-loan.clar).

Due to the limitations of Stacks nodes, currently we must manually register contracts that wish to communicate with ours. Our `register-contract` function is picked up by the DLC.Link Blockchain Observer, thus allowing us to hear and process external calls of our functions correctly. Please contact us so that we can do the necessary steps.

Contracts wishing to communicate with ours must implement the following [dlc-link-callback-trait](https://github.com/DLC-link/dlc-clarity/blob/main/contracts/dlc-link-callback-trait.clar) to enable the proper callback functionalities:

```clarity
(use-trait cb-trait 'ST1PQHQKV0RJXZFY1DGX8MNSNYVE3VGZJSRTPGZGM.dlc-link-callback-trait-v2.dlc-link-callback-trait)
(impl-trait 'ST1PQHQKV0RJXZFY1DGX8MNSNYVE3VGZJSRTPGZGM.dlc-link-callback-trait-v2.dlc-link-callback-trait)
```

This requires implementing the following functions:

```clarity
(post-create-dlc-handler (uint (buff 32)) (response bool uint)) ;; called after succesful DLC creation
(post-close-dlc-handler ((buff 32)) (response bool uint))       ;; called after succesful DLC closing
(get-btc-price-callback (uint (buff 32)) (response bool uint))  ;; a function to pass back validated BTC price fetched from Redstone
(set-status-funded ((buff 32)) (response bool uint))            ;; called when BTC is successfully locked in the DLC on the Bitcoin blockchain
```

To request the creation of a DLC, call our contract like so (`target` is the callback-contract's address, `loan-id` is some local identifier for the requested DLC):

```clarity
(unwrap! (ok (contract-call? 'ST1PQHQKV0RJXZFY1DGX8MNSNYVE3VGZJSRTPGZGM.dlc-manager-priced-v0-1 create-dlc emergency-refund-time target loan-id)) err-contract-call-failed)
```

And similarly, to request a simple close with a DLC `UUID` and an `outcome` (0 means full refund to Accept party, 100 means full payment to Offer party. it has two decimals precision):

```clarity
(unwrap! (ok (as-contract (contract-call? 'ST1PQHQKV0RJXZFY1DGX8MNSNYVE3VGZJSRTPGZGM.dlc-manager-priced-v0-1 close-dlc uuid u0))) err-contract-call-failed)
```

We also offer BTC price fetching and validation through redstone through the following function:

```clarity
(unwrap! (ok (as-contract (contract-call? 'ST1PQHQKV0RJXZFY1DGX8MNSNYVE3VGZJSRTPGZGM.dlc-manager-priced-v0-1 get-btc-price uuid))) err-contract-call-failed)
```

This will return values through the above mentioned `get-btc-price-callback` function.
