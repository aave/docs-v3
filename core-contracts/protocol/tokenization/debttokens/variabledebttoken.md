# VariableDebtToken

Implements a variable debt token to track the borrowing positions of users at variable rate mode.

`transfer` and `approve` functionalities are disabled as variable debt tokens are non-transferable.

The vToken value is pegged 1:1 to the value of underlying borrowed asset and represents the current total amount owed to the protocol i.e. principal debt + interest accrued.

The VariableDebtToken contract inherits the [DebtTokenBase](../base/debttokenbase.md) and [ScaledBalanceTokenBase](../base/incentivizederc20.md) token contracts.

## Write Methods

### initialize

```solidity
function initialize(
    IPool initializingPool,
    address underlyingAsset,
    IAaveIncentivesController incentivesController,
    uint8 debtTokenDecimals,
    string memory debtTokenName,
    string memory debtTokenSymbol,
    bytes calldata params
) external override initializer 
```

Initializes the debt token.

#### Input Parameters:

| Name                 | Type                        | Description                                                              |
| :------------------- | :-------------------------- | :----------------------------------------------------------------------- |
| initializingPool     | `IPool`                     | The pool contract that is initializing this contract                     |
| underlyingAsset      | `address`                   | The address of the underlying asset of this aToken (E.g. WETH for aWETH) |
| incentivesController | `IAaveIncentivesController` | The smart contract managing potential incentives distribution            |
| debtTokenDecimals    | `uint8`                     | The decimals of the variableDebtToken, same as the underlying asset's    |
| debtTokenName        | `string`                    | The name of the variable debt token                                      |
| debtTokenSymbol      | `string`                    | The symbol of the variable debt token                                    |
| params               | `bytes`                     | A set of encoded parameters for additional initialization                |

### mint

```solidity
function mint(
    address user,
    address onBehalfOf,
    uint256 amount,
    uint256 index
) external virtual override onlyPool returns (bool, uint256)
```

Mints the variable debt token to the `onBehalfOf` address.

#### Input Parameters:

| Name       | Type      | Description                                                                                                                      |
| :--------- | :-------- | :------------------------------------------------------------------------------------------------------------------------------- |
| user       | `address` | The address receiving the borrowed underlying, being the delegatee in case of credit delegate, or same as `onBehalfOf` otherwise |
| onBehalfOf | `address` | The address receiving the variable debt tokens                                                                                   |
| amount     | `uint256` | The amount of variable debt tokens to mint                                                                                       |
| index      | `uint256` | The variable debt index of the reserve                                                                                           |

#### Return Values:

| Type      | Description                                                        |
| :-------- | :----------------------------------------------------------------- |
| `bool`    | `true` if the previous balance of the user is 0, `false` otherwise |
| `uint256` | The scaled total debt of the reserve                               |

### burn

```solidity
function burn(
    address from,
    uint256 amount,
    uint256 index
) external virtual override onlyPool returns (uint256)
```

Burns user variable debt.

In some instances, a burn transaction will emit a mint event if the amount to burn is less than the interest that the user accrued.

#### Input Parameters:

| Name   | Type      | Description                                    |
| :----- | :-------- | :--------------------------------------------- |
| from   | `address` | The address from which the debt will be burned |
| amount | `uint256` | The amount of debt tokens that will be burned  |
| index  | `uint256` | The variable debt index of the reserve         |

#### Return Values:

| Type      | Description                          |
| :-------- | :----------------------------------- |
| `uint256` | The scaled total debt of the reserve |

## View Methods

### UNDERLYING_ASSET_ADDRESS

```solidity
function UNDERLYING_ASSET_ADDRESS() external view override returns (address)
```

Returns the address of the underlying asset of this variableDebtToken (e.g. WETH for variableDebtWETH)

#### Return Values:

| Type      | Description                         |
| :-------- | :---------------------------------- |
| `address` | The address of the underlying asset |

### balanceOf

```solidity
function balanceOf(address account) public view virtual override returns (uint256) 
```

Returns the amount of tokens owned by `account` - the most up to date accumulated debt (principal + interest) of the user.

Standard ERC20 function. 

#### Input Parameters:

| Name    | Type      | Description                 |
| :------ | :-------- | :-------------------------- |
| account | `address` | The balance of this address |

#### Return Values:

| Type      | Description                             |
| :-------- | :-------------------------------------- |
| `uint256` | The amount of tokens owned by `account` |

### totalSupply

```solidity
function totalSupply() public view virtual override returns (uint256) 
```

Returns the amount of tokens in existence - the most up to date total debt accrued by all protocol users for that specific variable rate of debt token.

Standard ERC20 function. 

#### Return Values:

| Type      | Description                       |
| :-------- | :-------------------------------- |
| `uint256` | The amount of tokens in existence |

## Pure Methods

### getRevision

```solidity
function getRevision() internal pure virtual override returns (uint256)
```

Returns the revision number of the contract. Needs to be defined in the inherited class as a constant.

Returns `0x1`.

#### Return Values:

| Type      | Description         |
| :-------- | :------------------ |
| `uint256` | The revision number | 

## NOT SUPPORTED OPERATIONS

Being non-transferrable, the variable debt token does not implement any of the standard ERC20 functions for `transfer` and `allowance`.

The functions below will revert with the error code 80, `OPERATION_NOT_SUPPORTED`.

### transfer

```solidity
function transfer(address, uint256) external virtual override returns (bool)
```

{% hint style="danger" %}
Operation not supported. 
{% endhint %}

### allowance

```solidity
function allowance(address, address) external view virtual override returns (uint256)
```

{% hint style="danger" %}
Operation not supported. 
{% endhint %}

### approve

```solidity
function approve(address, uint256) external virtual override returns (bool) 
```

{% hint style="danger" %}
Operation not supported. 
{% endhint %}

### transferFrom

```solidity
function transferFrom(
    address,
    address,
    uint256
) external virtual override returns (bool)
```

{% hint style="danger" %}
Operation not supported. 
{% endhint %}

### increaseAllowance

```solidity
function increaseAllowance(address, uint256) external virtual override returns (bool)
```

{% hint style="danger" %}
Operation not supported. 
{% endhint %}

### decreaseAllowance
```solidity
function decreaseAllowance(address, uint256) external virtual override returns (bool)
```

{% hint style="danger" %}
Operation not supported. 
{% endhint %}

## ABI
<details>
<summary>VariableDebtToken ABI</summary>

```
[
    {
        "inputs": [
            {
                "internalType": "contract IPool",
                "name": "pool",
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
                "name": "owner",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "spender",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "value",
                "type": "uint256"
            }
        ],
        "name": "Approval",
        "type": "event"
    },
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
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "from",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "target",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "value",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "balanceIncrease",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "index",
                "type": "uint256"
            }
        ],
        "name": "Burn",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "underlyingAsset",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "pool",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "address",
                "name": "incentivesController",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "uint8",
                "name": "debtTokenDecimals",
                "type": "uint8"
            },
            {
                "indexed": false,
                "internalType": "string",
                "name": "debtTokenName",
                "type": "string"
            },
            {
                "indexed": false,
                "internalType": "string",
                "name": "debtTokenSymbol",
                "type": "string"
            },
            {
                "indexed": false,
                "internalType": "bytes",
                "name": "params",
                "type": "bytes"
            }
        ],
        "name": "Initialized",
        "type": "event"
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
                "name": "onBehalfOf",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "value",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "balanceIncrease",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "index",
                "type": "uint256"
            }
        ],
        "name": "Mint",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "from",
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
                "name": "value",
                "type": "uint256"
            }
        ],
        "name": "Transfer",
        "type": "event"
    },
    {
        "inputs": [],
        "name": "DEBT_TOKEN_REVISION",
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
        "inputs": [],
        "name": "UNDERLYING_ASSET_ADDRESS",
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
                "name": "",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "",
                "type": "address"
            }
        ],
        "name": "allowance",
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
                "name": "",
                "type": "address"
            },
            {
                "internalType": "uint256",
                "name": "",
                "type": "uint256"
            }
        ],
        "name": "approve",
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
                "name": "user",
                "type": "address"
            }
        ],
        "name": "balanceOf",
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
                "name": "from",
                "type": "address"
            },
            {
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "index",
                "type": "uint256"
            }
        ],
        "name": "burn",
        "outputs": [
            {
                "internalType": "uint256",
                "name": "",
                "type": "uint256"
            }
        ],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "decimals",
        "outputs": [
            {
                "internalType": "uint8",
                "name": "",
                "type": "uint8"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "",
                "type": "address"
            },
            {
                "internalType": "uint256",
                "name": "",
                "type": "uint256"
            }
        ],
        "name": "decreaseAllowance",
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
        "inputs": [],
        "name": "getIncentivesController",
        "outputs": [
            {
                "internalType": "contract IAaveIncentivesController",
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
                "name": "user",
                "type": "address"
            }
        ],
        "name": "getPreviousIndex",
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
                "name": "user",
                "type": "address"
            }
        ],
        "name": "getScaledUserBalanceAndSupply",
        "outputs": [
            {
                "internalType": "uint256",
                "name": "",
                "type": "uint256"
            },
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
                "name": "",
                "type": "address"
            },
            {
                "internalType": "uint256",
                "name": "",
                "type": "uint256"
            }
        ],
        "name": "increaseAllowance",
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
        "inputs": [
            {
                "internalType": "contract IPool",
                "name": "initializingPool",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "underlyingAsset",
                "type": "address"
            },
            {
                "internalType": "contract IAaveIncentivesController",
                "name": "incentivesController",
                "type": "address"
            },
            {
                "internalType": "uint8",
                "name": "debtTokenDecimals",
                "type": "uint8"
            },
            {
                "internalType": "string",
                "name": "debtTokenName",
                "type": "string"
            },
            {
                "internalType": "string",
                "name": "debtTokenSymbol",
                "type": "string"
            },
            {
                "internalType": "bytes",
                "name": "params",
                "type": "bytes"
            }
        ],
        "name": "initialize",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "user",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "onBehalfOf",
                "type": "address"
            },
            {
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "index",
                "type": "uint256"
            }
        ],
        "name": "mint",
        "outputs": [
            {
                "internalType": "bool",
                "name": "",
                "type": "bool"
            },
            {
                "internalType": "uint256",
                "name": "",
                "type": "uint256"
            }
        ],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "name",
        "outputs": [
            {
                "internalType": "string",
                "name": "",
                "type": "string"
            }
        ],
        "stateMutability": "view",
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
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "user",
                "type": "address"
            }
        ],
        "name": "scaledBalanceOf",
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
        "inputs": [],
        "name": "scaledTotalSupply",
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
                "internalType": "contract IAaveIncentivesController",
                "name": "controller",
                "type": "address"
            }
        ],
        "name": "setIncentivesController",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "symbol",
        "outputs": [
            {
                "internalType": "string",
                "name": "",
                "type": "string"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "totalSupply",
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
                "name": "",
                "type": "address"
            },
            {
                "internalType": "uint256",
                "name": "",
                "type": "uint256"
            }
        ],
        "name": "transfer",
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
        "inputs": [
            {
                "internalType": "address",
                "name": "",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "",
                "type": "address"
            },
            {
                "internalType": "uint256",
                "name": "",
                "type": "uint256"
            }
        ],
        "name": "transferFrom",
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