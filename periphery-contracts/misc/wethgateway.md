# WETHGateway

The WETH Gateway contract is a helper to easily wrap and unwrap ETH (or the native currency of the chain eg. MATIC, AVAX) as necessary when interacting with the protocol.

## Write Methods

### depositETH

```solidity
function depositETH(address pool, address onBehalfOf, uint16 referralCode) external payable override
```

Supplies the `msg.value` amount of WETH into the reserve, using native ETH (or native chain token). The corresponding amount of the overlying asset (aTokens) is minted and transferring to the `onBehalfOf` address.

{% hint style="info" %}
Ensure that the `depositETH()` transaction also includes the amount of ETH you are supplying in the `msg.value`.
{% endhint %}

{% hint style="info" %}
Referral supply is currently inactive, you can pass `0` as `referralCode`. This program may be activated in the future through an Aave governance proposal.
{% endhint %}

#### Input Parameters:

| Name         | Type      | Description                                                                                                                                                                                                                      |
| :----------- | :-------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| pool         | `address` | The address of the targeted underlying pool                                                                                                                                                                                      |
| onBehalfOf   | `address` | The address of the user who will receive the aTokens representing the supplied. Use `msg.sender` when the aTokens should be sent to the caller                                                                                   |
| referralCode | `uint16`  | Referral supply is currently inactive, you can pass `0`. This code is used to register the integrator originating the operation, for potential rewards. 0 if the action is executed directly by the user, without any middle-men |

### withdrawETH

```solidity
function withdrawETH(address pool, uint256 amount, address to) external override
```

Withdraws the `amount` of WETH (or wrapped native chain token) from `msg.sender`, unwraps it and transfers ETH (or native chain token) to the `to` address.

{% hint style="info" %}
Ensure you set the relevant `aToken` allowance, before calling this function, so the `WETHGateway` contract can transfer the associated aWETH.
{% endhint %}

#### Input Parameters:

| Name   | Type      | Description                                                                                                                         |
| :----- | :-------- | :---------------------------------------------------------------------------------------------------------------------------------- |
| pool   | `address` | The address of the targeted underlying pool                                                                                         |
| amount | `uint256` | The amount of aWETH to withdraw and receive native ETH, expressed in wei units. Use `type(uint).max` to withdraw the entire balance |
| to     | `address` | The address of the user who will receive native unwrapped ETH                                                                       |

### repayETH

```solidity
function repayETH(address pool, uint256 amount, uint256 rateMode, address onBehalfOf) external payable override
```

Repays a borrow on the WETH reserve, for the specified amount (or for the whole amount, if `uint256(-1)` is specified). This is repaying `onBehalfOf`'s debt `amount` of ETH which has a `rateMode`.

{% hint style="info" %}
Ensure that the `repayETH()` transaction also includes the amount of ETH you are repaying in the `msg.value`.
{% endhint %}

#### Input Parameters:

| Name       | Type      | Description                                                                                             |
| :--------- | :-------- | :------------------------------------------------------------------------------------------------------ |
| pool       | `address` | The address of the targeted underlying lending pool                                                     |
| amount     | `uint256` | The amount to repay, expressed in wei units. Or use `uint256(-1)` if the user wants to repay everything |
| rateMode   | `uint256` | The rate mode to repay (type of borrow debt): 1 for Stable, 2 for Variable                              |
| onBehalfOf | `address` | The address of user who will repay. Use `msg.sender` when not calling on behalf of a different user     |

### borrowETH

```solidity
function borrowETH(address pool, uint256 amount, uint256 interesRateMode, uint16 referralCode) external override 
```

Borrows WETH, unwraps to ETH and sends both the ETH and DebtTokens to `msg.sender`, via `approveDelegation` and onBehalf argument in `Pool.borrow`.

{% hint style="info" %}
Referral supply is currently inactive, you can pass `0` as `referralCode`. This program may be activated in the future through an Aave governance proposal.
{% endhint %}

#### Input Parameters:

| Name            | Type      | Description                                                                                                                                                                                                                      |
| :-------------- | :-------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| pool            | `address` | The address of the targeted underlying pool                                                                                                                                                                                      |
| amount          | `uint256` | The amount of ETH to borrow, expressed in wei units                                                                                                                                                                              |
| interesRateMode | `uint256` | The interest rate mode : 1 for Stable, 2 for Variable                                                                                                                                                                            |
| referralCode    | `uint16`  | Referral supply is currently inactive, you can pass `0`. This code is used to register the integrator originating the operation, for potential rewards. 0 if the action is executed directly by the user, without any middle-men |

### withdrawETHWithPermit

```solidity
function withdrawETHWithPermit(
    address pool,
    uint256 amount,
    address to,
    uint256 deadline,
    uint8 permitV,
    bytes32 permitR,
    bytes32 permitS
) external override
```

Withdraws `amount` of WETH (or wrapped native chain token) of `msg.sender` without a separate approval transaction. The ETH (or native chain token) is sent to the `to` address.

#### Input Parameters:

| Name     | Type      | Description                                                                                                                       |
| :------- | :-------- | :-------------------------------------------------------------------------------------------------------------------------------- |
| pool     | `address` | The address of the targeted underlying pool                                                                                       |
| amount   | `uint256` | The amount of aWETH (or aToken corresponding to native token of chain) to withdraw and receive native ETH (or native chain token) |
| to       | `address` | The address of the user who will receive native ETH (or native chain token)                                                       |
| deadline | `uint256` | The validity deadline of permit, and so depositWithPermit signature                                                               |
| permitV  | `uint8`   | The v parameter of ERC712 permit signature                                                                                        |
| permitR  | `bytes32` | The r parameter of ERC712 permit signature                                                                                        |
| permitS  | `bytes32` | The s parameter of ERC712 permit signature                                                                                        |

#### emergencyTokenTransfer

```solidity
function emergencyTokenTransfer(address token, address to, uint256 amount) external onlyOwner
```

Transfers ERC20 tokens from the utility contract. Method for ERC20 recovery in case of stuck tokens due direct transfers to the contract address.

{% hint style="info" %}
Can be called only by the owner of the contract i.e. Aave Governance.
{% endhint %}

#### Input Parameters:

| Name   | Type      | Description                                  |
| :----- | :-------- | :------------------------------------------- |
| token  | `address` | The address of the token to transfer         |
| to     | `address` | The address of the recipient of the transfer |
| amount | `uint256` | The amount to send                           |

### emergencyEtherTransfer

```solidity
  function emergencyEtherTransfer(address to, uint256 amount) external onlyOwner 
```

Transfer native Ether from the utility contract. Method for ETH (or native chain token) recovery in case of stuck ETH due self-destruct or transfer ether to pre-computated contract address before deployment.

{% hint style="info" %}
Can be called only by the owner of the contract i.e. Aave Governance.
{% endhint %}

#### Input Parameters:

| Name   | Type      | Description                                  |
| :----- | :-------- | :------------------------------------------- |
| to     | `address` | The address of the token to transfer         |
| amount | `uint256` | The address of the recipient of the transfer |

## View Methods

### getWETHAddress

```solidity
function getWETHAddress() external view returns (address)
```

Gets the WETH address used by WETHGateway.

#### Return Values:

| Type      | Description                              |
| :-------- | :--------------------------------------- |
| `address` | The WETH address used by the WETHGateway |

## ABI
<details>
<summary>WETHGateway ABI</summary>

```
[
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "weth",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "owner",
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
        "stateMutability": "payable",
        "type": "fallback"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "pool",
                "type": "address"
            }
        ],
        "name": "authorizePool",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "pool",
                "type": "address"
            },
            {
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "interesRateMode",
                "type": "uint256"
            },
            {
                "internalType": "uint16",
                "name": "referralCode",
                "type": "uint16"
            }
        ],
        "name": "borrowETH",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "pool",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "onBehalfOf",
                "type": "address"
            },
            {
                "internalType": "uint16",
                "name": "referralCode",
                "type": "uint16"
            }
        ],
        "name": "depositETH",
        "outputs": [],
        "stateMutability": "payable",
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
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            }
        ],
        "name": "emergencyEtherTransfer",
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
        "name": "emergencyTokenTransfer",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "getWETHAddress",
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
                "internalType": "address",
                "name": "pool",
                "type": "address"
            },
            {
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "rateMode",
                "type": "uint256"
            },
            {
                "internalType": "address",
                "name": "onBehalfOf",
                "type": "address"
            }
        ],
        "name": "repayETH",
        "outputs": [],
        "stateMutability": "payable",
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
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "pool",
                "type": "address"
            },
            {
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            },
            {
                "internalType": "address",
                "name": "to",
                "type": "address"
            }
        ],
        "name": "withdrawETH",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "pool",
                "type": "address"
            },
            {
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            },
            {
                "internalType": "address",
                "name": "to",
                "type": "address"
            },
            {
                "internalType": "uint256",
                "name": "deadline",
                "type": "uint256"
            },
            {
                "internalType": "uint8",
                "name": "permitV",
                "type": "uint8"
            },
            {
                "internalType": "bytes32",
                "name": "permitR",
                "type": "bytes32"
            },
            {
                "internalType": "bytes32",
                "name": "permitS",
                "type": "bytes32"
            }
        ],
        "name": "withdrawETHWithPermit",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "stateMutability": "payable",
        "type": "receive"
    }
]
```
</details>