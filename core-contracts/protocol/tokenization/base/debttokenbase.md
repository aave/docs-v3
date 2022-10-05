# DebtTokenBase

Base contract for different types of debt tokens, like StableDebtToken or VariableDebtToken.

## Write Methods

### approveDelegation

```solidity
function approveDelegation(address delegatee, uint256 amount) external override
```

Delegates borrowing power to a user on the specific debt token. Delegation will still respect the liquidation constraints (even if delegated, a delegatee cannot force a delegator HF to go below 1).

#### Input Parameters: 

| Name      | Type      | Description                                         | 
| :-------- | :-------- | :-------------------------------------------------- | 
| delegatee | `address` | The address receiving the delegated borrowing power |
| amount    | `uint256` | The maximum amount being delegated                  |

### delegationWithSig

```solidity
function delegationWithSig(
    address delegator,
    address delegatee,
    uint256 value,
    uint256 deadline,
    uint8 v,
    bytes32 r,
    bytes32 s
) external
```

Delegates borrowing power to a user on the specific debt token via ERC712 signature.

#### Input Parameters: 

| Name      | Type      | Description                                                  |  
| :-------- | :-------- | :----------------------------------------------------------- | 
| delegator | `address` | The delegator of the credit                                  |
| delegatee | `address` | The delegatee that can use the credit                        |
| value     | `uint256` | The amount to be delegated                                   |
| deadline  | `uint256` | The deadline timestamp, `type(uint256).max` for max deadline |
| v         | `uint8`   | The V signature param                                        |
| r         | `bytes32` | The S signature param                                        |
| s         | `bytes32` | The R signature param                                        |

## View Methods

### borrowAllowance

```solidity
function borrowAllowance(address fromUser, address toUser)
    external
    view
    override
    returns (uint256)
```

Returns the borrow allowance of the user.

#### Input Parameters: 

| Name     | Type      | Description                   |  
| :------- | :-------- | :---------------------------- | 
| fromUser | `address` | The user to giving allowance  |
| toUser   | `address` | The user to give allowance to |

#### Return Values:

| Type      | Description                       |
| :-------- | :-------------------------------- | 
| `uint256` | The current allowance of `toUser` |


## ABI
<details>
<summary>DebtTokenBase ABI</summary>

```
[
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "fromUser",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "toUser",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "asset",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            }
        ],
        "name": "BorrowAllowanceDelegated",
        "type": "event"
    },
    {
        "inputs": [],
        "name": "DELEGATION_WITH_SIG_TYPEHASH",
        "outputs": [
            {
                "internalType": "bytes32",
                "name": "",
                "type": "bytes32"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "DOMAIN_SEPARATOR",
        "outputs": [
            {
                "internalType": "bytes32",
                "name": "",
                "type": "bytes32"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "EIP712_REVISION",
        "outputs": [
            {
                "internalType": "bytes",
                "name": "",
                "type": "bytes"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "delegatee",
                "type": "address"
            },
            {
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            }
        ],
        "name": "approveDelegation",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "fromUser",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "toUser",
                "type": "address"
            }
        ],
        "name": "borrowAllowance",
        "outputs": [
            {
                "internalType": "uint256",
                "name": "",
                "type": "uint256"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "delegator",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "delegatee",
                "type": "address"
            },
            {
                "internalType": "uint256",
                "name": "value",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "deadline",
                "type": "uint256"
            },
            {
                "internalType": "uint8",
                "name": "v",
                "type": "uint8"
            },
            {
                "internalType": "bytes32",
                "name": "r",
                "type": "bytes32"
            },
            {
                "internalType": "bytes32",
                "name": "s",
                "type": "bytes32"
            }
        ],
        "name": "delegationWithSig",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "owner",
                "type": "address"
            }
        ],
        "name": "nonces",
        "outputs": [
            {
                "internalType": "uint256",
                "name": "",
                "type": "uint256"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    }
]
```
</details>