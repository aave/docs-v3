# AaveProtocolDataProvider

Peripheral contract to collect and pre-process information from the Pool.

Code available on [github](https://github.com/aave/aave-v3-core/blob/master/contracts/misc/AaveProtocolDataProvider.sol#L164).

## Methods

### getAllReservesTokens
`function getAllReservesTokens() external view returns (TokenData[] memory)`

Returns list of the existing reserves in the pool.

Return Value
| Type      | Description                                 |
| --------- | ------------------------------------------- |
| `string`  | The symbol of the underlying reserve asset  |
| `address` | The address of the underlying reserve asset |

### getAllATokens
`function getAllATokens() external view returns (TokenData[] memory)`

Returns list of the existing ATokens in the pool.

Return Value
| Type      | Description                              |
| --------- | ---------------------------------------- |
| `string`  | The symbol of aToken of the reserve  |
| `address` | The address of aToken of the reserve |

### getReserveConfigurationData
`function getReserveConfigurationData(address asset) external view returns (....)`

Returns the configuration data of the reserve as described below:

Return Value
| Type      | Description                                 |
| --------- | ------------------------------------------- |
| `uint256` | The number of decimals of the reserve       |
| `uint256` | The ltv of the reserve                      |
| `uint256` | The liquidationThreshold of the reserve     |
| `uint256` | The liquidationBonus of the reserve         |
| `uint256` | The reserveFactor of the reserve |
| `bool`    | True if the usage as collateral is enabled, false otherwise |
| `bool`    | True if borrowing is enabled, false otherwise |
| `bool`    | True if stable rate borrowing is enabled, false otherwise |
| `bool`    | True if reserve is active, false otherwise |
| `bool`    | True if reserve is frozen, false otherwise |

### getSiloedBorrowing

`function getSiloedBorrowing(address asset) external view returns (bool)`

Returns true if the asset is _**siloed for borrowing**_.

Call Params
| Name  | Type      | Description                                        |
| ----- | --------- | -------------------------------------------------- |
| asset | `address` | The address of the underlying asset of the reserve |
