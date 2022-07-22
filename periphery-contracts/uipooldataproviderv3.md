# UiPoolDataProviderV3

## UiPoolDataProviderV3

Contract that returns an array of all reserve or user data for a particular market, used by the [Aave Interface](https://github.com/aave/interface/) to display Markets and Dashboard data. Compatible with both V2 and V3 of the Aave Protocol.

The [Aave Utilities SDK](https://github.com/aave/aave-utilities#data-formatting-methods) includes an interface to make calls to this contract, and functions to format the response for frontend use-cases.

## Data Structures

### AggregatedReserveData

View fields of `AggregatedReserveData` defined at [Github](https://github.com/aave/aave-v3-periphery/blob/ed38b6719d4bbd9d17dfbd6b9849326a0bdeea2c/contracts/misc/interfaces/IUiPoolDataProviderV3.sol#L8).

### UserReserveData

| Name                            | Type    | Description                                                                            |
| ------------------------------- | ------- | -------------------------------------------------------------------------------------- |
| underlyingAsset                 | address | Address of the underlying asset supplied/borrowed                                      |
| scaledATokenBalance             | uint256 | <p>scaled balance of aToken<br><br><em>scaledBalance = balance/liquidityIndex</em></p> |
| usageAsCollateralEnabledOnUser  | bool    | true if supplied asset is enabled to be used as collateral                             |
| stableBorrowRate                | uint256 | Stable rate at which underlying asset is borrowed by the user. 0 ⇒ no debt             |
| scaledVariableDebt              | uint256 | <p>scaled balance of vToken<br><br><em>scaledBalance = balance/liquidityIndex</em></p> |
| principalStableDebt             | uint256 | Principal amount borrowed at stable rate                                               |
| stableBorrowLastUpdateTimestamp | uint256 | unix timestamp of last update on user’s stable borrow position.                        |

### BaseCurrencyInfo

Info data struct for base currency of the Aave protocol market.

| Name                              | Type    | Description                                       |
| --------------------------------- | ------- | ------------------------------------------------- |
| marketReferenceCurrencyUnit       | uint256 | Reference aka base currency of the Aave market    |
| marketReferenceCurrencyPriceInUsd | int256  | Price of reference aka base currency in USD       |
| networkBaseTokenPriceInUsd        | int256  | Price of native token of the network/chain in USD |
| networkBaseTokenPriceDecimals     | uint8   | Decimals of native token of the network/chain     |

## Methods

### getReservesList

`function getReservesList(IPoolAddressesProvider provider)`

Returns the list of initialised reserves in the Pool associated with the given [`provider`](../core-contracts/pooladdressesprovider.md).

### getReservesData

`function getReservesData(IPoolAddressesProvider provider)`

Returns `BaseCurrencyInfo` of the Pool and `AggregatedReserveData[]` for all the initialised reserves in the Pool associated with the given [`provider`](../core-contracts/pooladdressesprovider.md).

### getUserReservesData

`function getUserReservesData(IPoolAddressesProvider provider, address user)`

Returns `UserReserveData[]` for all user reserves in the Pool associated with the given [`provider`](../core-contracts/pooladdressesprovider.md).
