# FlashLoanReceiverBase

Base contract to develop a flashloan-receiver contract.

Please look at the [IFlashLoanReceiver](../interfaces/IFlashLoanReceiver.md) interface and [Flash Loan guide](../../../guides/flash-loans.md).

The source code is available on [GitHub](https://github.com/aave/aave-v3-core/blob/master/contracts/flashloan/base/FlashLoanReceiverBase.sol).

## ABI
<details>
<summary>FlashLoanReceiverBase ABI</summary>

```
[
    {
        "inputs": [],
        "name": "ADDRESSES_PROVIDER",
        "outputs": [
            {
                "internalType": "contract IPoolAddressesProvider",
                "name": "",
                "type": "address"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "POOL",
        "outputs": [
            {
                "internalType": "contract IPool",
                "name": "",
                "type": "address"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address[]",
                "name": "assets",
                "type": "address[]"
            },
            {
                "internalType": "uint256[]",
                "name": "amounts",
                "type": "uint256[]"
            },
            {
                "internalType": "uint256[]",
                "name": "premiums",
                "type": "uint256[]"
            },
            {
                "internalType": "address",
                "name": "initiator",
                "type": "address"
            },
            {
                "internalType": "bytes",
                "name": "params",
                "type": "bytes"
            }
        ],
        "name": "executeOperation",
        "outputs": [
            {
                "internalType": "bool",
                "name": "",
                "type": "bool"
            }
        ],
        "stateMutability": "nonpayable",
        "type": "function"
    }
]
```
</details>