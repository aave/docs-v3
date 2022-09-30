# PoolAddressesProviderRegistry

The main registery of the active [PoolAddressesProvider](./pooladdressesprovider.md) contracts, covering all Aave markets. This contract is immutable and the address will never change.

This contract is used for indexing purposes of the Aave protocol's markets. The `id` assigned to a `PoolAddressesProvider` refers to the market it is connected with, for example, with `1` for the Aave main market and `2` for the next created.

The source code can be found [here](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/configuration/PoolAddressesProviderRegistry.sol).

## Write Methods

### registerAddressesProvider

```solidity
function registerAddressesProvider(address provider, uint256 id) external override onlyOwner
```

Registers an addresses provider. The `PoolAddressesProvider` must not already be registered in the registry, and the `id` must not be used by an already registered `PoolAddressesProvider`.

#### Input Parameters:

| Name     | Type      | Description                                                                     |
| :------- | :-------- | :------------------------------------------------------------------------------ |
| provider | `address` | The address of the new PoolAddressesProvider                                    |
| id       | `uint256` | The id for the new PoolAddressesProvider, referring to the market it belongs to |

### unregisterAddressesProvider

```solidity
function unregisterAddressesProvider(address provider) external override onlyOwner
```

Removes an addresses provider from the list of registered addresses providers.

#### Return Values:

| Name     | Type      | Description                       |
| :------- | :-------- | :-------------------------------- |
| provider | `address` | The PoolAddressesProvider address |

## View Methods

### getAddressesProvidersList

```solidity
function getAddressesProvidersList() external view override returns (address[] memory)
```

Returns a list of active [`PoolAddressesProvider`](pooladdressesprovider.md) contracts for the registered Aave protocol markets.

#### Return Values:

| Type        | Description                            |
| :---------- | :------------------------------------- |
| `address[]` | The list of active addresses providers |

### getAddressesProviderIdByAddress

```solidity
function getAddressesProviderIdByAddress(address addressesProvider) external view override returns (uint256)
```

Returns the id of a registered `PoolAddressesProvider`.

#### Input Parameters:

| Name              | Type      | Description                              |
| :---------------- | :-------- | :--------------------------------------- |
| addressesProvider | `address` | The address of the PoolAddressesProvider |

#### Return Values:

| Type      | Description                                                                                        |
| :-------- | :------------------------------------------------------------------------------------------------- |
| `uint256` | The id of the associated PoolAddressesProvider. 0 indicates the address is not registered or valid |

### getAddressesProviderAddressById

```solidity
function getAddressesProviderAddressById(uint256 id) external view override returns (address)
```

Returns the address of a registered `PoolAddressesProvider`.

#### Input Parameters:

| Name | Type      | Description          |
| :--- | :-------- | :------------------- |
| id   | `uint256` | The id of the market |

#### Return Values:

| Type      | Description                                                                                        |
| :-------- | :------------------------------------------------------------------------------------------------- |
| `address` | The address of the PoolAddressesProvider with the given id or zero address if it is not registered |


## ABI
<details>
<summary>PoolAddressesProviderRegistry ABI</summary>

```
[
    {
        "inputs": [
            {
                "internalType": "contract IPoolAddressesProvider",
                "name": "addressesProvider",
                "type": "address"
            }
        ],
        "stateMutability": "nonpayable",
        "type": "constructor"
    },
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
        "inputs": [
            {
                "internalType": "address",
                "name": "asset",
                "type": "address"
            }
        ],
        "name": "getATokenTotalSupply",
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
        "name": "getAllATokens",
        "outputs": [
            {
                "components": [
                    {
                        "internalType": "string",
                        "name": "symbol",
                        "type": "string"
                    },
                    {
                        "internalType": "address",
                        "name": "tokenAddress",
                        "type": "address"
                    }
                ],
                "internalType": "struct AaveProtocolDataProvider.TokenData[]",
                "name": "",
                "type": "tuple[]"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "getAllReservesTokens",
        "outputs": [
            {
                "components": [
                    {
                        "internalType": "string",
                        "name": "symbol",
                        "type": "string"
                    },
                    {
                        "internalType": "address",
                        "name": "tokenAddress",
                        "type": "address"
                    }
                ],
                "internalType": "struct AaveProtocolDataProvider.TokenData[]",
                "name": "",
                "type": "tuple[]"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "asset",
                "type": "address"
            }
        ],
        "name": "getDebtCeiling",
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
        "name": "getDebtCeilingDecimals",
        "outputs": [
            {
                "internalType": "uint256",
                "name": "",
                "type": "uint256"
            }
        ],
        "stateMutability": "pure",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "asset",
                "type": "address"
            }
        ],
        "name": "getInterestRateStrategyAddress",
        "outputs": [
            {
                "internalType": "address",
                "name": "irStrategyAddress",
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
                "name": "asset",
                "type": "address"
            }
        ],
        "name": "getLiquidationProtocolFee",
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
                "name": "asset",
                "type": "address"
            }
        ],
        "name": "getPaused",
        "outputs": [
            {
                "internalType": "bool",
                "name": "isPaused",
                "type": "bool"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "asset",
                "type": "address"
            }
        ],
        "name": "getReserveCaps",
        "outputs": [
            {
                "internalType": "uint256",
                "name": "borrowCap",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "supplyCap",
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
                "name": "asset",
                "type": "address"
            }
        ],
        "name": "getReserveConfigurationData",
        "outputs": [
            {
                "internalType": "uint256",
                "name": "decimals",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "ltv",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "liquidationThreshold",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "liquidationBonus",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "reserveFactor",
                "type": "uint256"
            },
            {
                "internalType": "bool",
                "name": "usageAsCollateralEnabled",
                "type": "bool"
            },
            {
                "internalType": "bool",
                "name": "borrowingEnabled",
                "type": "bool"
            },
            {
                "internalType": "bool",
                "name": "stableBorrowRateEnabled",
                "type": "bool"
            },
            {
                "internalType": "bool",
                "name": "isActive",
                "type": "bool"
            },
            {
                "internalType": "bool",
                "name": "isFrozen",
                "type": "bool"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "asset",
                "type": "address"
            }
        ],
        "name": "getReserveData",
        "outputs": [
            {
                "internalType": "uint256",
                "name": "unbacked",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "accruedToTreasuryScaled",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "totalAToken",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "totalStableDebt",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "totalVariableDebt",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "liquidityRate",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "variableBorrowRate",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "stableBorrowRate",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "averageStableBorrowRate",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "liquidityIndex",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "variableBorrowIndex",
                "type": "uint256"
            },
            {
                "internalType": "uint40",
                "name": "lastUpdateTimestamp",
                "type": "uint40"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "asset",
                "type": "address"
            }
        ],
        "name": "getReserveEModeCategory",
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
                "name": "asset",
                "type": "address"
            }
        ],
        "name": "getReserveTokensAddresses",
        "outputs": [
            {
                "internalType": "address",
                "name": "aTokenAddress",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "stableDebtTokenAddress",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "variableDebtTokenAddress",
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
                "name": "asset",
                "type": "address"
            }
        ],
        "name": "getSiloedBorrowing",
        "outputs": [
            {
                "internalType": "bool",
                "name": "",
                "type": "bool"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "asset",
                "type": "address"
            }
        ],
        "name": "getTotalDebt",
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
                "name": "asset",
                "type": "address"
            }
        ],
        "name": "getUnbackedMintCap",
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
                "name": "asset",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "user",
                "type": "address"
            }
        ],
        "name": "getUserReserveData",
        "outputs": [
            {
                "internalType": "uint256",
                "name": "currentATokenBalance",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "currentStableDebt",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "currentVariableDebt",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "principalStableDebt",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "scaledVariableDebt",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "stableBorrowRate",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "liquidityRate",
                "type": "uint256"
            },
            {
                "internalType": "uint40",
                "name": "stableRateLastUpdated",
                "type": "uint40"
            },
            {
                "internalType": "bool",
                "name": "usageAsCollateralEnabled",
                "type": "bool"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    }
]
```
</details>