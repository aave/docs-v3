# CollectorController

The CollectorController contracts allows the owner of the contract to approve or transfer tokens from the collector proxy contract.

{% hint style="danger" %}
The admin of the Collector proxy can't be the same as the fundsAdmin address. This is needed due the usage of transparent proxy pattern.
{% endhint %}

## Write Methods

### approve

```solidity
function approve(IERC20 token, address recipient, uint256 amount) external onlyOwner
```

Transfer an amount of tokens to the recipient.

#### Input Parameters:

| Name      | Type      | Description                                                                 |
| :-------- | :-------- | :-------------------------------------------------------------------------- |
| token     | `IERC20`  | The address of the asset                                                    | 
| recipient | `address` | The address of the entity allowed to pull tokens                            | 
| amount    | `uint256` | The amount allowed to be pulled. If set to `0`, it will revoke the approval | 


### transfer

```solidity
function transfer(IERC20 token, address recipient, uint256 amount) external onlyOwner
```

Transfer an amount of tokens to the recipient.

#### Input Parameters:

| Name      | Type      | Description                                      |
| :-------- | :-------- | :----------------------------------------------- |
| token     | `IERC20`  | The address of the asset                         | 
| recipient | `address` | The address of the entity to transfer the tokens | 
| amount    | `uint256` | The amount to be transferred                     | 

## ABI
<details>
<summary>CollectorController ABI</summary>

```
[
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "owner",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "collectorProxy",
                "type": "address"
            }
        ],
        "stateMutability": "nonpayable",
        "type": "constructor"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "previousOwner",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "newOwner",
                "type": "address"
            }
        ],
        "name": "OwnershipTransferred",
        "type": "event"
    },
    {
        "inputs": [],
        "name": "COLLECTOR",
        "outputs": [
            {
                "internalType": "contract ICollector",
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
                "internalType": "contract IERC20",
                "name": "token",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "recipient",
                "type": "address"
            },
            {
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            }
        ],
        "name": "approve",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "owner",
        "outputs": [
            {
                "internalType": "address",
                "name": "",
                "type": "address"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "renounceOwnership",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "contract IERC20",
                "name": "token",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "recipient",
                "type": "address"
            },
            {
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            }
        ],
        "name": "transfer",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "newOwner",
                "type": "address"
            }
        ],
        "name": "transferOwnership",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    }
]
```
</details>