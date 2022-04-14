# PriceOracleSentinel

## PriceOracleSentinel

This contract validates if the operations are allowed depending on the PriceOracle health.

The `PriceOracle` is considered healthy once its completely up and the grace period has passed.

## View Methods

### isBorrowAllowed

`function isBorrowAllowed()`

Return Value

| Type | Description                                                   |
| ---- | ------------------------------------------------------------- |
| bool | Returns true if PriceOracle is up and grace period has passed |

### isLiquidationAllowed

`function isLiquidationAllowed()`

Return Value

| Type | Description                                                   |
| ---- | ------------------------------------------------------------- |
| bool | Returns true if PriceOracle is up and grace period has passed |

### getSequencerOracle

`function getSequencerOracle()`

Return Value

| Type    | Description                     |
| ------- | ------------------------------- |
| address | Address of the SequencerOracle. |

### getGracePeriod

`function getGracePeriod()`

Return Value

| Type    | Description                                  |
| ------- | -------------------------------------------- |
| uint256 | The duration of the grace period in seconds. |

## Write Methods

### setSequencerOracle

`function setSequencerOracle(address newSequencerOracle)`

{% hint style="warning" %}
Can be called only by PoolAdmin.
{% endhint %}

Call Params

| Name               | Type    | Description                                  |
| ------------------ | ------- | -------------------------------------------- |
| newSequencerOracle | address | address of the new SequecerOracle to be set. |

### setGracePeriod

`function setGracePeriod(uint256 newGracePeriod`

{% hint style="warning" %}
Can be called only by PoolAdmin or RiskAdmin.
{% endhint %}

Call Params

| Name           | Type    | Description                              |
| -------------- | ------- | ---------------------------------------- |
| newGracePeriod | uint256 | duration of new grace period in seconds. |
