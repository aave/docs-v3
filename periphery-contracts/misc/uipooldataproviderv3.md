# UiPoolDataProviderV3

Contract that returns an array of all reserve or user data for a particular market, used by the [Aave Interface](https://github.com/aave/interface/) to display Markets and Dashboard data. Compatible with both V2 and V3 of the Aave Protocol.

The [Aave Utilities SDK](https://github.com/aave/aave-utilities#data-formatting-methods) includes an interface to make calls to this contract, and functions to format the response for frontend use-cases.

## View Methods

### getReservesList

```solidity
function getReservesList(IPoolAddressesProvider provider) public view override returns (address[] memory)
```

Returns the list of initialised reserves in the Pool associated with the given [`provider`](../core-contracts/pooladdressesprovider.md).

#### Input Parameters:

The pool associated with given [`provider`](../core-contracts/pooladdressesprovider.md).

#### Return Values:

| Type        | Description                                  |
| :---------- | :------------------------------------------- |
| `address[]` | The list of initialised reserves in the Pool |

### getReservesData

```solidity
function getReservesData(IPoolAddressesProvider provider) public view override returns (AggregatedReserveData[] memory, BaseCurrencyInfo memory)
```

Returns `BaseCurrencyInfo` of the Pool and `AggregatedReserveData[]` for all the initialised reserves in the Pool associated with the given [`provider`](../core-contracts/pooladdressesprovider.md).

#### Input Parameters:

The pool associated with given [`provider`](../core-contracts/pooladdressesprovider.md).

#### Return Values:

`AggregatedReserveData[]`
| Name                           | Type      | Description                                                            |
| :----------------------------- | :-------- | :--------------------------------------------------------------------- |
| underlyingAsset                | `address` | The address of the underlying asset of the reserve                     |
| name                           | `string`  | The name of the underlying reserve asset                               |
| symbol                         | `string`  | The symbol of the underlying reserve asset                             |
| decimals                       | `uint256` | The number of decimals of the reserve                                  |
| baseLTVasCollateral            | `uint256` | The ltv of the reserve                                                 |
| reserveLiquidationThreshold    | `uint256` | The liquidation threshold of the reserve                               |
| reserveLiquidationBonus        | `uint256` | The liquidation bonus of the resurve                                   |
| reserveFactor                  | `uint256` | The reserve factor of the reserve                                      |
| usageAsCollateralEnabled       | `bool`    | True if the asset is enabled to be used as collateral, false otherwise |
| borrowingEnabled               | `bool`    | True if borrowing is enabled, false otherwise                          |
| stableBorrowRateEnabled        | `bool`    | True if stable rate borrowing is enabled, false otherwise              |
| isActive                       | `bool`    | True if reserve is active, false otherwise                             |
| isFrozen                       | `bool`    | True if reserve is frozen, false otherwise                             |
| BASE DATA                      |           |                                                                        |
| liquidityIndex                 | `uint128` | The liquidity index of the reserve                                     |
| variableBorrowIndex            | `uint128` | The variable borrow index of the reserve                               |
| liquidityRate                  | `uint128` | The liquidity rate of the reserve                                      |
| variableBorrowRate             | `uint128` | The variable borrow rate of the reserve                                |
| stableBorrowRate               | `uint128` | The stable borrow rate of the reserve                                  |
| lastUpdateTimestamp            | `uint40`  | The timestamp of the last update of the reserve                        |
| aTokenAddress                  | `address` | The AToken address of the reserve                                      |
| stableDebtTokenAddress         | `address` | The StableDebtToken address of the reserve                             |
| variableDebtTokenAddress       | `address` | The VariableDebtToken address of the reserve                           |
| interestRateStrategyAddress    | `address` | The address of the Interest Rate strategy                              |
|                                |           |                                                                        | 
| availableLiquidity             | `uint256` | The liquidity available                                                |
| totalPrincipalStableDebt       | `uint256` | The principal stable debt                                              |
| averageStableRate              | `uint256` | The average stable rate                                                |
| stableDebtLastUpdateTimestamp  | `uint256` |  The timestamp of the last update of the reserve                       |
| totalScaledVariableDebt        | `uint256` | The total scaled variable debt                                         |
| priceInMarketReferenceCurrency | `uint256` | Price of reference aka base currency of Aave market                    |
| priceOracle                    | `address` | The address of the price oracle used by the associated market          |
| variableRateSlope1             | `uint256` | The variable rate slope                                                |
| variableRateSlope2             | `uint256` | The variable rate slope                                                |
| stableRateSlope1               | `uint256` | The stable rate slope                                                  |
| stableRateSlope2               | `uint256` | The stable rate slope                                                  |
| baseStableBorrowRate           | `uint256` | The base stable borrow rate, expressed in ray                          |
| baseVariableBorrowRate         | `uint256` | The base variable borrow rate, expressed in ray                        |
| optimalUsageRatio              | `uint256` | The optimal usage ratio                                                |
|                                |           |                                                                        |
| V3 ONLY                        |           |                                                                        |
| isPaused                       | `bool`    | True if the pool is paused, false otherwise                            |
| isSiloedBorrowing              | `bool`    | True if the asset is siloed for borrowing                              |
| accruedToTreasury              | `uint128` | The amount of tokens accrued to treasury that is to be minted          |
| unbacked                       | `uint128` | The amount of unbacked aTokens of the reserve                          |
| isolationModeTotalDebt         | `uint128` | The outstanding debt borrowed against this asset in isolation mode     |
|                                |           |                                                                        |
| debtCeiling                    | `uint256` | The debt ceiling of the reserve                                        |
| debtCeilingDecimals            | `uint256` | The debt ceiling decimals                                              |
| eModeCategoryId                | `uint8`   | The eMode id of the reserve                                            |
| borrowCap                      | `uint256` | The borrow cap of the reserve                                          |
| supplyCap                      | `uint256` | The supply cap of the reserve                                          |
| E-MODE                         |           |                                                                        |
| eModeLtv                       | `uint16`  | The custom Loan to Value for the  eMode category                       |
| eModeLiquidationThreshold      | `uint16`  | The custom liquidation threshold for the  eMode category               |
| eModeLiquidationBonus          | `uint16`  | The liquidation bonus for the  eMode category                          |
| eModePriceSource               | `address` | The custom price source for the  eMode category                        |
| eModeLabel                     | `string`  | The custom label describing the  eMode category                        |
| borrowableInIsolation          | `bool`    | True if the asset should be borrowable in isolation, false otherwise   |

`BaseCurrencyInfo`
| Name                              | Type      | Description                                       |
| :-------------------------------- | :-------- | :------------------------------------------------ |
| marketReferenceCurrencyUnit       | `uint256` | Reference aka base currency of the Aave market    |
| marketReferenceCurrencyPriceInUsd | `int256`  | Price of reference aka base currency in USD       |
| networkBaseTokenPriceInUsd        | `int256`  | Price of native token of the network/chain in USD |
| networkBaseTokenPriceDecimals     | `uint8`   | Decimals of native token of the network/chain     |

### getUserReservesData

```solidity
function getUserReservesData(IPoolAddressesProvider provider, address user) external view override returns (UserReserveData[] memory, uint8)
```

Returns `UserReserveData[]` for all user reserves in the Pool associated with the given [`provider`](../core-contracts/pooladdressesprovider.md).

#### Input Parameters:

The pool associated with given [`provider`](../core-contracts/pooladdressesprovider.md).

| Name | Type      | Description             |
| :--- | :-------- | :---------------------- |
| user | `address` | The address of the user |

### UserReserveData

| Name                            | Type      | Description                                                                                 |
| :------------------------------ | :-------- | :------------------------------------------------------------------------------------------ |
| underlyingAsset                 | `address` | The address of the underlying asset supplied/borrowed                                       |
| scaledATokenBalance             | `uint256` | The scaled balance of the aToken. scaledBalance = balance/liquidityIndex                    |
| usageAsCollateralEnabledOnUser  | `bool`    | True if the supplied asset is enabled to be used as collateral, false otherwise             |
| stableBorrowRate                | `uint256` | The stable rate at which underlying asset is borrowed by the user. 0 means there is no debt |
| scaledVariableDebt              | `uint256` | The scaled balance of vToken. scaledBalance = balance/liquidityIndex                        |
| principalStableDebt             | `uint256` | The principal amount borrowed at a stable rate                                              |
| stableBorrowLastUpdateTimestamp | `uint256` | The unix timestamp of last update on the user’s stable borrow position                      |

## ABI
<details>
<summary>UiPoolDataProviderV3</summary>

```
[
    {
        "inputs": [
            {
                "internalType": "contract IEACAggregatorProxy",
                "name": "_networkBaseTokenPriceInUsdProxyAggregator",
                "type": "address"
            },
            {
                "internalType": "contract IEACAggregatorProxy",
                "name": "_marketReferenceCurrencyPriceInUsdProxyAggregator",
                "type": "address"
            }
        ],
        "stateMutability": "nonpayable",
        "type": "constructor"
    },
    {
        "inputs": [],
        "name": "ETH_CURRENCY_UNIT",
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
        "name": "MKR_ADDRESS",
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
                "internalType": "bytes32",
                "name": "_bytes32",
                "type": "bytes32"
            }
        ],
        "name": "bytes32ToString",
        "outputs": [
            {
                "internalType": "string",
                "name": "",
                "type": "string"
            }
        ],
        "stateMutability": "pure",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "contract IPoolAddressesProvider",
                "name": "provider",
                "type": "address"
            }
        ],
        "name": "getReservesData",
        "outputs": [
            {
                "components": [
                    {
                        "internalType": "address",
                        "name": "underlyingAsset",
                        "type": "address"
                    },
                    {
                        "internalType": "string",
                        "name": "name",
                        "type": "string"
                    },
                    {
                        "internalType": "string",
                        "name": "symbol",
                        "type": "string"
                    },
                    {
                        "internalType": "uint256",
                        "name": "decimals",
                        "type": "uint256"
                    },
                    {
                        "internalType": "uint256",
                        "name": "baseLTVasCollateral",
                        "type": "uint256"
                    },
                    {
                        "internalType": "uint256",
                        "name": "reserveLiquidationThreshold",
                        "type": "uint256"
                    },
                    {
                        "internalType": "uint256",
                        "name": "reserveLiquidationBonus",
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
                    },
                    {
                        "internalType": "uint128",
                        "name": "liquidityIndex",
                        "type": "uint128"
                    },
                    {
                        "internalType": "uint128",
                        "name": "variableBorrowIndex",
                        "type": "uint128"
                    },
                    {
                        "internalType": "uint128",
                        "name": "liquidityRate",
                        "type": "uint128"
                    },
                    {
                        "internalType": "uint128",
                        "name": "variableBorrowRate",
                        "type": "uint128"
                    },
                    {
                        "internalType": "uint128",
                        "name": "stableBorrowRate",
                        "type": "uint128"
                    },
                    {
                        "internalType": "uint40",
                        "name": "lastUpdateTimestamp",
                        "type": "uint40"
                    },
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
                    },
                    {
                        "internalType": "address",
                        "name": "interestRateStrategyAddress",
                        "type": "address"
                    },
                    {
                        "internalType": "uint256",
                        "name": "availableLiquidity",
                        "type": "uint256"
                    },
                    {
                        "internalType": "uint256",
                        "name": "totalPrincipalStableDebt",
                        "type": "uint256"
                    },
                    {
                        "internalType": "uint256",
                        "name": "averageStableRate",
                        "type": "uint256"
                    },
                    {
                        "internalType": "uint256",
                        "name": "stableDebtLastUpdateTimestamp",
                        "type": "uint256"
                    },
                    {
                        "internalType": "uint256",
                        "name": "totalScaledVariableDebt",
                        "type": "uint256"
                    },
                    {
                        "internalType": "uint256",
                        "name": "priceInMarketReferenceCurrency",
                        "type": "uint256"
                    },
                    {
                        "internalType": "address",
                        "name": "priceOracle",
                        "type": "address"
                    },
                    {
                        "internalType": "uint256",
                        "name": "variableRateSlope1",
                        "type": "uint256"
                    },
                    {
                        "internalType": "uint256",
                        "name": "variableRateSlope2",
                        "type": "uint256"
                    },
                    {
                        "internalType": "uint256",
                        "name": "stableRateSlope1",
                        "type": "uint256"
                    },
                    {
                        "internalType": "uint256",
                        "name": "stableRateSlope2",
                        "type": "uint256"
                    },
                    {
                        "internalType": "uint256",
                        "name": "baseStableBorrowRate",
                        "type": "uint256"
                    },
                    {
                        "internalType": "uint256",
                        "name": "baseVariableBorrowRate",
                        "type": "uint256"
                    },
                    {
                        "internalType": "uint256",
                        "name": "optimalUsageRatio",
                        "type": "uint256"
                    },
                    {
                        "internalType": "bool",
                        "name": "isPaused",
                        "type": "bool"
                    },
                    {
                        "internalType": "bool",
                        "name": "isSiloedBorrowing",
                        "type": "bool"
                    },
                    {
                        "internalType": "uint128",
                        "name": "accruedToTreasury",
                        "type": "uint128"
                    },
                    {
                        "internalType": "uint128",
                        "name": "unbacked",
                        "type": "uint128"
                    },
                    {
                        "internalType": "uint128",
                        "name": "isolationModeTotalDebt",
                        "type": "uint128"
                    },
                    {
                        "internalType": "uint256",
                        "name": "debtCeiling",
                        "type": "uint256"
                    },
                    {
                        "internalType": "uint256",
                        "name": "debtCeilingDecimals",
                        "type": "uint256"
                    },
                    {
                        "internalType": "uint8",
                        "name": "eModeCategoryId",
                        "type": "uint8"
                    },
                    {
                        "internalType": "uint256",
                        "name": "borrowCap",
                        "type": "uint256"
                    },
                    {
                        "internalType": "uint256",
                        "name": "supplyCap",
                        "type": "uint256"
                    },
                    {
                        "internalType": "uint16",
                        "name": "eModeLtv",
                        "type": "uint16"
                    },
                    {
                        "internalType": "uint16",
                        "name": "eModeLiquidationThreshold",
                        "type": "uint16"
                    },
                    {
                        "internalType": "uint16",
                        "name": "eModeLiquidationBonus",
                        "type": "uint16"
                    },
                    {
                        "internalType": "address",
                        "name": "eModePriceSource",
                        "type": "address"
                    },
                    {
                        "internalType": "string",
                        "name": "eModeLabel",
                        "type": "string"
                    },
                    {
                        "internalType": "bool",
                        "name": "borrowableInIsolation",
                        "type": "bool"
                    }
                ],
                "internalType": "struct IUiPoolDataProviderV3.AggregatedReserveData[]",
                "name": "",
                "type": "tuple[]"
            },
            {
                "components": [
                    {
                        "internalType": "uint256",
                        "name": "marketReferenceCurrencyUnit",
                        "type": "uint256"
                    },
                    {
                        "internalType": "int256",
                        "name": "marketReferenceCurrencyPriceInUsd",
                        "type": "int256"
                    },
                    {
                        "internalType": "int256",
                        "name": "networkBaseTokenPriceInUsd",
                        "type": "int256"
                    },
                    {
                        "internalType": "uint8",
                        "name": "networkBaseTokenPriceDecimals",
                        "type": "uint8"
                    }
                ],
                "internalType": "struct IUiPoolDataProviderV3.BaseCurrencyInfo",
                "name": "",
                "type": "tuple"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "contract IPoolAddressesProvider",
                "name": "provider",
                "type": "address"
            }
        ],
        "name": "getReservesList",
        "outputs": [
            {
                "internalType": "address[]",
                "name": "",
                "type": "address[]"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "contract IPoolAddressesProvider",
                "name": "provider",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "user",
                "type": "address"
            }
        ],
        "name": "getUserReservesData",
        "outputs": [
            {
                "components": [
                    {
                        "internalType": "address",
                        "name": "underlyingAsset",
                        "type": "address"
                    },
                    {
                        "internalType": "uint256",
                        "name": "scaledATokenBalance",
                        "type": "uint256"
                    },
                    {
                        "internalType": "bool",
                        "name": "usageAsCollateralEnabledOnUser",
                        "type": "bool"
                    },
                    {
                        "internalType": "uint256",
                        "name": "stableBorrowRate",
                        "type": "uint256"
                    },
                    {
                        "internalType": "uint256",
                        "name": "scaledVariableDebt",
                        "type": "uint256"
                    },
                    {
                        "internalType": "uint256",
                        "name": "principalStableDebt",
                        "type": "uint256"
                    },
                    {
                        "internalType": "uint256",
                        "name": "stableBorrowLastUpdateTimestamp",
                        "type": "uint256"
                    }
                ],
                "internalType": "struct IUiPoolDataProviderV3.UserReserveData[]",
                "name": "",
                "type": "tuple[]"
            },
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
        "inputs": [],
        "name": "marketReferenceCurrencyPriceInUsdProxyAggregator",
        "outputs": [
            {
                "internalType": "contract IEACAggregatorProxy",
                "name": "",
                "type": "address"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "networkBaseTokenPriceInUsdProxyAggregator",
        "outputs": [
            {
                "internalType": "contract IEACAggregatorProxy",
                "name": "",
                "type": "address"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    }
]
```
</details>