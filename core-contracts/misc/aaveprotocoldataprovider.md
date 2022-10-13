# AaveProtocolDataProvider

Peripheral contract to collect and pre-process information from the Pool.

The source code is available on [GitHub](https://github.com/aave/aave-v3-core/blob/master/contracts/misc/AaveProtocolDataProvider.sol).

## View Methods

### getAllReservesTokens

```solidity
function getAllReservesTokens() external view returns (TokenData[] memory)
```

Returns the list of the existing reserves in the pool, pairs include the `symbol` and `tokenAddress`. Handles [MKR](https://github.com/aave/aave-v3-core/blob/master/contracts/misc/AaveProtocolDataProvider.sol#L25) and [ETH](https://github.com/aave/aave-v3-core/blob/master/contracts/misc/AaveProtocolDataProvider.sol#L26) in a different way since they do not have standard symbol functions.

#### Return Values:

| Type           | Description                                          | 
| :------------- | :--------------------------------------------------- | 
| `TokenData[]`  | The list of reserves, pairs of symbols and addresses | 

The [TokenData](https://github.com/aave/aave-v3-core/blob/master/contracts/misc/AaveProtocolDataProvider.sol#L28) struct is composed of the following fields:

| Name         | Type      | Description                                 | 
| :----------- | :-------- | :------------------------------------------ | 
| symbol       | `string`  | The symbol of the underlying reserve asset  | 
| tokenAddress | `address` | The address of the underlying reserve asset |

### getAllATokens

```solidity
function getAllATokens() external view returns (TokenData[] memory)
```

Returns list of the existing ATokens in the pool, pairs include the `symbol` and `tokenAddress`.

#### Return Values:

| Type           | Description                                         | 
| :------------- | :-------------------------------------------------- | 
| `TokenData[]`  | The list of ATokens, pairs of symbols and addresses | 

The [TokenData](https://github.com/aave/aave-v3-core/blob/master/contracts/misc/AaveProtocolDataProvider.sol#L28) struct is composed of the following fields:

| Name         | Type      | Description                          | 
| :----------- | :-------- | :----------------------------------- | 
| symbol       | `string`  | The symbol of aToken of the reserve  | 
| tokenAddress | `address` | The address of aToken of the reserve |

### getReserveConfigurationData

```solidity
function getReserveConfigurationData(address asset) external view returns (
    uint256 decimals,
    uint256 ltv,
    uint256 liquidationThreshold,
    uint256 liquidationBonus,
    uint256 reserveFactor,
    bool usageAsCollateralEnabled,
    bool borrowingEnabled,
    bool stableBorrowRateEnabled,
    bool isActive,
    bool isFrozen
)
```

Returns the configuration data of the reserve as described below. Does not return borrow and supply caps, nor pause flag for compatibility.

#### Input Parameters:

| Name  | Type      | Description                                        | 
| :---- | :-------- | :------------------------------------------------- | 
| asset | `address` | The address of the underlying asset of the reserve |

#### Return Values:

| Name                     | Type      | Description                                                     |
| :------------------------| :-------- | :-------------------------------------------------------------- |
| decimals                 | `uint256` | The number of decimals of the reserve                           |
| ltv                      | `uint256` | The ltv of the reserve                                          |
| liquidationThreshold     | `uint256` | The liquidation threshold of the reserve                        |
| liquidationBonus         | `uint256` | The liquidation bonus of the reserve                            |
| reserveFactor            | `uint256` | The reserve factor of the reserve                               |
| usageAsCollateralEnabled | `bool`    | `true` if the usage as collateral is enabled, `false` otherwise |
| borrowingEnabled         | `bool`    | `true` if borrowing is enabled, `false` otherwise               |
| stableBorrowRateEnabled  | `bool`    | `true` if stable rate borrowing is enabled, `false` otherwise   |
| isActive                 | `bool`    | `true` if reserve is active, `false` otherwise                  |
| isFrozen                 | `bool`    | `true` if reserve is frozen, `false` otherwise                  |

### getReserveEModeCategory

```solidity
function getReserveEModeCategory(address asset) external view returns (uint256)
```

Returns the reserve's efficiency mode category.

#### Input Parameters: 

| Name  | Type      | Description                                        | 
| :---- | :-------- | :------------------------------------------------- | 
| asset | `address` | The address of the underlying asset of the reserve |

#### Return Values:

| Type      | Description                 |
| :-------- | :-------------------------- |
| `uint256` | The eMode id of the reserve |

### getReserveCaps

```solidity
function getReserveCaps(address asset) external view returns (uint256 borrowCap, uint256 supplyCap)
```

Returns the caps parameters of the reserve.

#### Input Parameters: 

| Name  | Type      | Description                                        | 
| :---- | :-------- | :------------------------------------------------- | 
| asset | `address` | The address of the underlying asset of the reserve |

### Return Values: 

| Name      | Type      | Description                   | 
| :-------- | :-------- | :---------------------------- | 
| borrowCap | `uint256` | The borrow cap of the reserve | 
| supplyCap | `uint256` | The supply cap of the reserve |

### getPaused

```solidity
function getPaused(address asset) external view returns (bool isPaused)
```

Returns true if the pool `isPaused`.

#### Input Parameters: 

| Name  | Type      | Description                                        | 
| :---- | :-------- | :------------------------------------------------- | 
| asset | `address` | The address of the underlying asset of the reserve |

#### Return Values:

| Name     | Type   | Description                                     |
| :------- | :----- | :---------------------------------------------- |
| isPaused | `bool` | `true` if the pool is paused, `false` otherwise |

### getSiloedBorrowing

```solidity
function getSiloedBorrowing(address asset) external view returns (bool)
```

Returns the siloed borrowing flag. It returns true if the `asset` is [siloed for borrowing](../whats-new/siloed-borrowing.md).

#### Input Parameters: 

| Name  | Type      | Description                                        | 
| :---- | :-------- | :------------------------------------------------- | 
| asset | `address` | The address of the underlying asset of the reserve |

#### Return Values:

| Type   | Description                                 |
| :----- | :------------------------------------------ |
| `bool` | `true` if the asset is siloed for borrowing |


### getLiquidationProtocolFee

```solidity
function getLiquidationProtocolFee(address asset) external view returns (uint256)
```

Returns the protocol fee on the liquidation bonus.

#### Input Parameters:

| Name  | Type      | Description                                        | 
| :---- | :-------- | :------------------------------------------------- | 
| asset | `address` | The address of the underlying asset of the reserve |

#### Return Values:

| Type      | Description                     |
| :-------- | :------------------------------ |
| `uint256` | The protocol fee on liquidation |

### getUnbackedMintCap

```solidity
function getUnbackedMintCap(address asset) external view returns (uint256)
```

Returns the unbacked mint cap of the reserve.

#### Input Parameters: 

| Name  | Type      | Description                                        | 
| :---- | :-------- | :------------------------------------------------- | 
| asset | `address` | The address of the underlying asset of the reserve |

#### Return Values: 

| Type      | Description                          |
| :-------- | :----------------------------------- |
| `uint256` | The unbacked mint cap of the reserve |

### getDebtCeiling

```solidity
function getDebtCeiling(address asset) external view returns (uint256)
```

Returns the debt ceiling of the reserve.

#### Input Parameters: 

| Name  | Type      | Description                                        | 
| :---- | :-------- | :------------------------------------------------- | 
| asset | `address` | The address of the underlying asset of the reserve |

#### Return Values:

| Type      | Description                     |
| :-------- | :------------------------------ | 
| `uint256` | The debt ceiling of the reserve |

### getReserveData

```solidity
function getReserveData(address asset) external view override returns (
    uint256 unbacked,
    uint256 accruedToTreasuryScaled,
    uint256 totalAToken,
    uint256 totalStableDebt,
    uint256 totalVariableDebt,
    uint256 liquidityRate,
    uint256 variableBorrowRate,
    uint256 stableBorrowRate,
    uint256 averageStableBorrowRate,
    uint256 liquidityIndex,
    uint256 variableBorrowIndex,
    uint40 lastUpdateTimestamp
)
```

Returns the reserve data.

#### Input Parameters:

| Name  | Type      | Description                                        | 
| :---- | :-------- | :------------------------------------------------- | 
| asset | `address` | The address of the underlying asset of the reserve |

#### Return Values: 

| Name                    | Type      | Description                                                          |
| :---------------------- | :-------- | :------------------------------------------------------------------- |
| unbacked                | `uint256` | The amount of unbacked aTokens of the reserve                        |
| accruedToTreasuryScaled | `uint256` | The scaled amount of tokens accrued to treasury that is to be minted |
| totalAToken             | `uint256` | The total supply of the aToken                                       |
| totalStableDebt         | `uint256` | The total stable debt of the reserve                                 |
| totalVariableDebt       | `uint256` | The total variable debt of the reserve                               |
| liquidityRate           | `uint256` | The liquidity rate of the reserve                                    |
| variableBorrowRate      | `uint256` | The variable borrow rate of the reserve                              |
| stableBorrowRate        | `uint256` | The stable borrow rate of the reserve                                |
| averageStableBorrowRate | `uint256` | The average stable borrow rate of the reserve                        |
| liquidityIndex          | `uint256` | The liquidity index of the reserve                                   |
| variableBorrowIndex     | `uint256` | The variable borrow index of the reserve                             |
| lastUpdateTimestamp     | `uint40`  | The timestamp of the last update of the reserve                      |

### getATokenTotalSupply

```solidity
function getATokenTotalSupply(address asset) external view override returns (uint256)
```

Returns the total supply of aTokens for a given `asset`.

#### Input Parameters:

| Name  | Type      | Description                                        |
| :---- | :-------- | :------------------------------------------------- |
| asset | `address` | The address of the underlying asset of the reserve |

#### Return Values:

| Type      | Description                    |
| :-------- | :----------------------------- |
| `uint256` | The total supply of the aToken |

#### getTotalDebt

```solidity
function getTotalDebt(address asset) external view override returns (uint256)
```

Returns the total debt for a given `asset`.

#### Input Parameters:

| Name  | Type      | Description                                        |
| :---- | :-------- | :------------------------------------------------- |
| asset | `address` | The address of the underlying asset of the reserve |

#### Return Values:

| Type      | Description                                     | 
| :-------- | :---------------------------------------------- | 
| `uint256` | The total debt (stable + variable) for an asset |

#### getUserReserveData

```solidity
function getUserReserveData(address asset, address user) external view returns (
    uint256 currentATokenBalance,
    uint256 currentStableDebt,
    uint256 currentVariableDebt,
    uint256 principalStableDebt,
    uint256 scaledVariableDebt,
    uint256 stableBorrowRate,
    uint256 liquidityRate,
    uint40 stableRateLastUpdated,
    bool usageAsCollateralEnabled
)
```

Returns the following `user` reserve data.

#### Input Parameters:

| Name  | Type      | Description                                        |
| :---- | :-------- | :------------------------------------------------- |
| asset | `address` | The address of the underlying asset of the reserve |
| user  | `address` | The address of the user                            |

#### Return Values:

| Name                     | Type      | Description                                                       |
| :----------------------- | :-------- | :---------------------------------------------------------------- |
| currentATokenBalance     | `uint256` | The current AToken balance of the user                            |
| currentStableDebt        | `uint256` | The   of the user                               |
| currentVariableDebt      | `uint256` | The current variable debt of the user                             |
| principalStableDebt      | `uint256` | The principal stable debt of the user                             |
| scaledVariableDebt       | `uint256` | The scaled variable debt of the user                              |
| stableBorrowRate         | `uint256` | The stable borrow rate of the user                                |
| liquidityRate            | `uint256` | The liquidity rate of the reserve                                 |
| stableRateLastUpdated    | `uint40`  | The timestamp of the last update of the user stable rate          |
| usageAsCollateralEnabled | `bool`    | `true` if the user is using the asset as collateral, else `false` |

#### getReserveTokensAddresses

```solidity
function getReserveTokensAddresses(address asset) external view returns (
    address aTokenAddress, 
    address stableDebtTokenAddress, 
    address variableDebtTokenAddress
)
```

Returns the addresses of `aToken`, `stableDebtToken` and `variableDebtToken` of the reserve.

#### Input Parameters:

| Name  | Type      | Description                                        |
| :---- | :-------- | :------------------------------------------------- |
| asset | `address` | The address of the underlying asset of the reserve |

#### Return Values:

| Name                     | Type      | Description                                  |
| :----------------------- | :-------- | :------------------------------------------- |
| aTokenAddress            | `address` | The AToken address of the reserve            |
| stableDebtTokenAddress   | `address` | The StableDebtToken address of the reserve   |
| variableDebtTokenAddress | `address` | The VariableDebtToken address of the reserve |

#### getInterestRateStrategyAddress

```solidity
function getInterestRateStrategyAddress(address asset) external view returns (address irStrategyAddress)
```

Returns the address of the [Interest Rate strategy](../protocol/pool/interestratestrategies/interestratestrategies.md).

#### Input Parameters:

| Name  | Type      | Description                                        |
| :---- | :-------- | :------------------------------------------------- |
| asset | `address` | The address of the underlying asset of the reserve |

#### Return Values:

| Name              | Type      | Description                               |
| :---------------- | :-------- | :---------------------------------------- |
| irStrategyAddress | `address` | The address of the Interest Rate strategy |

## Pure Methods

#### getDebtCeilingDecimals

```solidity
function getDebtCeilingDecimals() external pure returns (uint256)
```

Returns the debt ceiling decimals.

#### Return Values: 

| Type      | Description               | 
| --------- | ------------------------- | 
| `uint256` | The debt ceiling decimals |

## ABI
<details>
<summary>AaveProtocolDataProvider</summary>

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