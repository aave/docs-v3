# StakedTokenTransferStrategy

Transfer strategy that stakes the rewards into a staking contract and transfers the staking contract token. The underlying token must be transferred to this contract to be able to stake it on demand.

## Write Methods

### performTransfer

```solidity
function performTransfer(address to, address reward, uint256 amount) external override (
    TransferStrategyBase, 
    ITransferStrategyBase
) onlyIncentivesController returns (bool)
```

Perform custom transfer logic via delegate call from source contract to a TransferStrategy implementation.

#### Input Parameters:

| Name   | Type      | Description                                      |
| :----- | :-------- | :----------------------------------------------- |
| to     | `address` | The account to transfer rewards                  |
| reward | `address` | The address of the reward token                  |
| amount | `uint256` | The amount to transfer to `to` address parameter |

#### Return Values:

| Type   | Description                                   |
| :----- | :-------------------------------------------- |
| `bool` | Returns `true` if the transfer logic succeeds |

### renewApproval

```solidity
function renewApproval() external onlyRewardsAdmin
```

Perform a `MAX_UINT` approval of AAVE to the Staked Aave contract.

### dropApproval

```solidity
function dropApproval() external onlyRewardsAdmin
```

Drop approval of AAVE to the Staked Aave contract in case of emergency.

## View Methods

### getStakeContract

```solidity
function getStakeContract() external view returns (address) 
```

Returns the Staked Token contract address.

#### Return Values:

| Type      | Description                       |
| :-------- | :-------------------------------- |
| `address` | The Staked Token contract address |

### getUnderlyingToken

```solidity
function getUnderlyingToken() external view returns (address)
```

Returns the underlying token address from the stake contract.

#### Return Values:

| Type      | Description                                          |
| :-------- | :--------------------------------------------------- |
| `address` | The underlying token address from the stake contract |

## ABI
<details>
<summary>StakedTokenTransferStrategy ABI</summary>

```
[
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "incentivesController",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "rewardsAdmin",
                "type": "address"
            },
            {
                "internalType": "contract IStakedToken",
                "name": "stakeToken",
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
        "inputs": [],
        "name": "dropApproval",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
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
        "inputs": [],
        "name": "getStakeContract",
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
        "name": "getUnderlyingToken",
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
    },
    {
        "inputs": [],
        "name": "renewApproval",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    }
]
```
</details>