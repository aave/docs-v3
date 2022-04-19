# AaveProtocolDataProvider

Peripheral contract to collect and pre-process information from the Pool.

Code available on [github](https://github.com/aave/aave-v3-core/blob/master/contracts/misc/AaveProtocolDataProvider.sol#L164).

## Methods

### getSiloedBorrowing

`function getSiloedBorrowing(address asset) external view returns (bool)`

Returns true if the asset is _**siloed for borrowing**_.

| Name  | Type      | Description                                        |
| ----- | --------- | -------------------------------------------------- |
| asset | `address` | The address of the underlying asset of the reserve |
