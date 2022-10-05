# TransferStrategyBase

Base contract for transfer strategies.

## Write Methods

### performTransfer

```solidity
function performTransfer(address to, address reward, uint256 amount) external virtual returns (bool)
```

Perform custom transfer logic via delegate call from source contract to a TransferStrategy implementation.

#### Input Parameters:

| Name   | Type      | Description                                          |
| :----- | :-------- | :--------------------------------------------------- |
| to     | `address` | The address to transfer rewards to                   |
| reward | `address` | The address of the reward token                      |
| amount | `uint256` | The amount to transfer to the `to` address parameter |

#### Return Values:

| Type      | Description                                   |
| :-------- | :-------------------------------------------- |
| `address` | Returns `true` if the transfer logic succeeds |

### emergencyWithdrawal

```solidity
function emergencyWithdrawal(address token, address to, uint256 amount) external onlyRewardsAdmin
```
Perform an emergency token withdrawal only callable by the Rewards admin.

#### Input Parameters:

| Name   | Type      | Description                                                   |
| :----- | :-------- | :------------------------------------------------------------ |
| token  | `address` | The address of the token to withdraw funds from this contract |
| to     | `address` | The address of the recipient of the withdrawal                |
| amount | `uint256` | The amount of the withdrawal                                  |

## View Methods

### getIncentivesController

```solidity
function getIncentivesController() external view override returns (address)
```

Returns the address of the Incentives Controller.

#### Return Values:

| Type      | Description                              |
| :-------- | :--------------------------------------- |
| `address` | The address of the Incentives Controller |

### getRewardsAdmin

```solidity
function getRewardsAdmin() external view override returns (address)
```

Returns the address of the Rewards admin.

#### Return Values:

| Type      | Description                      |
| :-------- | :------------------------------- |
| `address` | The address of the Rewards admin |

## ABI
<details>
<summary>TransferStrategyBase ABI</summary>

```
[
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "caller",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "token",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "to",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            }
        ],
        "name": "EmergencyWithdrawal",
        "type": "event"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "token",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "to",
                "type": "address"
            },
            {
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            }
        ],
        "name": "emergencyWithdrawal",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "getIncentivesController",
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
        "name": "getRewardsAdmin",
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
        "inputs": [
            {
                "internalType": "address",
                "name": "to",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "reward",
                "type": "address"
            },
            {
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            }
        ],
        "name": "performTransfer",
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