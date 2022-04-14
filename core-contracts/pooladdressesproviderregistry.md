# PoolAddressesProviderRegistry

## PoolAddressesProviderRegistry

A register of the active `[PoolAddressesProvider](PoolAddressesProvider%20e4804383e03649a28386e289acf5e591.md)` contracts, covering all markets. This contract is immutable and the address will never change.

For example, the Pool address for the main market is different from the Pool address for the AMM market.

The source code can be found on [Github](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/configuration/PoolAddressesProviderRegistry.sol).

## View Methods

### getAddressesProviderList

`function getAddressesProvidersList()`

Returns a list of active [`PoolAddressesProvider`](pooladdressesprovider.md) contracts for the registered Aave protocol markets.

Returs Values

| Type       | Description                          |
| ---------- | ------------------------------------ |
| address\[] | List of active PoolAddressesProvider |

### getAddressesProviderIdByAddress

`function getAddressesProviderIdByAddress(address addressesProvider)`

Returns Id of `PoolAddressesProvider` .

Call Params

| Name              | Type    | Description                          |
| ----------------- | ------- | ------------------------------------ |
| addressesProvider | address | Address of the PoolAddressesProvider |

Return Values

| Type    | Description                                                                                       |
| ------- | ------------------------------------------------------------------------------------------------- |
| uint256 | Id of the associated PoolAddressesProvider. 0 indicates not a valid PoolAddressesProvider address |
