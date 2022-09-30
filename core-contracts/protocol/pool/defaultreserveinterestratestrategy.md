# DefaultReserveInterestRateStrategy

Implements the calculation of the interest rates depending on the reserve state. The model of interest rate is based on 2 slopes, one before the `OPTIMAL_USAGE_RATIO` point of usage and another from that one to 100%.

{% hint style="info" %}
An instance of this same contract, can't be used across different Aave markets, due to the caching of the PoolAddressesProvider.
{% endhint %}

## View Methods

### getVariableRateSlope1

```solidity
function getVariableRateSlope1() external view returns (uint256)
```

Returns the variable rate slope below optimal usage ratio. It's the variable rate when usage ratio > 0 and <= OPTIMAL_USAGE_RATIO.

#### Return Values:

| Type      | Description             |
| :-------- | :---------------------- |
| `uint256` | The variable rate slope |

### getVariableRateSlope2

```solidity
function getVariableRateSlope2() external view returns (uint256)
```

Returns the variable rate slope above optimal usage ratio. It's the variable rate when usage ratio > OPTIMAL_USAGE_RATIO.

#### Return Values:

| Type      | Description             |
| :-------- | :---------------------- |
| `uint256` | The variable rate slope |

### getStableRateSlope1

```solidity
function getStableRateSlope1() external view returns (uint256)
```

Returns the stable rate slope below optimal usage ratio. It's the stable rate when usage ratio > 0 and <= OPTIMAL_USAGE_RATIO.

#### Return Values:

| Type      | Description           |
| :-------- | :-------------------- |
| `uint256` | The stable rate slope |

### getStableRateSlope2

```solidity
function getStableRateSlope2() external view returns (uint256)
```

Returns the stable rate slope above optimal usage ratio. Its the variable rate when usage ratio > OPTIMAL_USAGE_RATIO.

#### Return Values:

| Type      | Description           |
| :-------- | :-------------------- |
| `uint256` | The stable rate slope |

### getStableRateExcessOffset

```solidity
function getStableRateExcessOffset() external view returns (uint256) 
```

Returns the stable rate excess offset. An additional premium applied to the stable when stable debt > OPTIMAL_STABLE_TO_TOTAL_DEBT_RATIO.

#### Return Values:

| Type      | Description                   |
| :-------- | :---------------------------- |
| `uint256` | The stable rate excess offset |

### getBaseStableBorrowRate

```solidity
function getBaseStableBorrowRate() public view returns (uint256)
```

Returns the base stable borrow rate.

#### Return Values:

| Type      | Description                 |
| :-------- | :-------------------------- |
| `uint256` | The base stable borrow rate |

### getBaseVariableBorrowRate

```solidity
function getBaseVariableBorrowRate() external view override returns (uint256)
```
Returns the base variable borrow rate.

#### Return Values:

| Type      | Description                                     |
| :-------- | :---------------------------------------------- |
| `uint256` | The base variable borrow rate, expressed in ray |

### getMaxVariableBorrowRate

```solidity
function getMaxVariableBorrowRate() external view override returns (uint256)
```

Returns the maximum variable borrow rate.

#### Return Values:

| Type      | Description                                        |
| :-------- | :------------------------------------------------- |
| `uint256` | The maximum variable borrow rate, expressed in ray |

### calculateInterestRates

```solidity
function calculateInterestRates(
    DataTypes.CalculateInterestRatesParams calldata params
) external view override returns (uint256, uint256, uint256)
```

Calculates the interest rates depending on the reserve's state and configurations.

#### Input Parameters:

| Name                    | Type      | Description                                                                 |
| :---------------------- | :-------- | :-------------------------------------------------------------------------- |
| unbacked                | `uint256` | The amount of unbacked tokens                                               |
| liquidityAdded          | `uint256` | The liquidity added during the operation                                    |
| liquidityTaken          | `uint256` | The liquidity taken during the operation                                    |
| totalStableDebt         | `uint256` | The total borrowed from the reserve a stable rate                           |
| totalVariableDebt       | `uint256` | The total borrowed from the reserve at a variable rate                      |
| averageStableBorrowRate | `uint256` | The weighted average of all the stable rate loans                           |
| reserveFactor           | `uint256` | The reserve portion of the interest that goes to the treasury of the market |
| reserve                 | `address` | The address of the reserve                                                  |
| aToken                  | `address` | The address of the aToken                                                   |

#### Return Values:

| Name               | Type      | Description                                |
| :----------------- | :-------- | :----------------------------------------- |
| liquidityRate      | `uint256` | The liquidity rate expressed in rays       |
| stableBorrowRate   | `uint256` | The stable borrow rate expressed in rays   |
| variableBorrowRate | `uint256` | The variable borrow rate expressed in rays |