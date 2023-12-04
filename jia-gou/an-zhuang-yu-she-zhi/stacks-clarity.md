# Stacks/Clarity

The Clarity 合约带来 DLC.Link 函数到Stacks可以在这里找到: [this repository.](https://github.com/dlc-link/dlc-clarity)

理解与我们的Clarity智能合约的集成是通过阅读我们的[example protocol-contract](https://github.com/DLC-link/dlc-clarity/blob/main/examples/sample-contract-loan.clar).

由于Stacks 节点的限制，目前我们必须手动注册希望与我们通信的合约。我们的`register-contract`功能由DLC.Link区块链观察者选中，从而允许我们正确地听到和处理函数的外部调用。请与我们联系，以便我们采取必要的步骤。

希望与我们通信的契约必须实现以下[dlc-link-callback-trait](https://github.com/DLC-link/dlc-clarity/blob/main/contracts/dlc-link-callback-trait.clar)，以启用适当的回调功能:

```clarity
(use-trait cb-trait 'ST1PQHQKV0RJXZFY1DGX8MNSNYVE3VGZJSRTPGZGM.dlc-link-callback-trait-v2.dlc-link-callback-trait)
(impl-trait 'ST1PQHQKV0RJXZFY1DGX8MNSNYVE3VGZJSRTPGZGM.dlc-link-callback-trait-v2.dlc-link-callback-trait)
```

这需要执行以下函数:

```clarity
(post-create-dlc-handler (uint (buff 32)) (response bool uint)) ;; called after succesful DLC creation
(post-close-dlc-handler ((buff 32)) (response bool uint))       ;; called after succesful DLC closing
(get-btc-price-callback (uint (buff 32)) (response bool uint))  ;; a function to pass back validated BTC price fetched from Redstone
(set-status-funded ((buff 32)) (response bool uint))            ;; called when BTC is successfully locked in the DLC on the Bitcoin blockchain
```

&#x20;要请求创建一个 DLC, call our contract like so (`target` is the callback-contract's address, `loan-id` is some local identifier for the requested DLC):

```clarity
(unwrap! (ok (contract-call? 'ST1PQHQKV0RJXZFY1DGX8MNSNYVE3VGZJSRTPGZGM.dlc-manager-priced-v0-1 create-dlc emergency-refund-time target loan-id)) err-contract-call-failed)
```

相似的, 请求关闭一个 DLC `UUID` and an `outcome` (0 指全额退款给接受方, 100 指支付给要约方全部款项。它有两位小数的精度):

```clarity
(unwrap! (ok (as-contract (contract-call? 'ST1PQHQKV0RJXZFY1DGX8MNSNYVE3VGZJSRTPGZGM.dlc-manager-priced-v0-1 close-dlc uuid u0))) err-contract-call-failed)
```

我们还通过redstone提供BTC价格获取和验证，通过以下函数:

```clarity
(unwrap! (ok (as-contract (contract-call? 'ST1PQHQKV0RJXZFY1DGX8MNSNYVE3VGZJSRTPGZGM.dlc-manager-priced-v0-1 get-btc-price uuid))) err-contract-call-failed)
```

这将通过上面提到的 `get-btc-price-callback` 函数返回值.
