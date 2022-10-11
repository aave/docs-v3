# IReserveInterestRateStrategy

Interface for the calculation of the [interest rates](../protocol/pool/interestratestrategies/defaultreserveinterestratestrategy.md).

The source code is available on [GitHub](https://github.com/aave/aave-v3-core/blob/master/contracts/interfaces/IReserveInterestRateStrategy.sol).

{% tabs %}
{% tab title="IReserveInterestRateStrategy.sol" %}
```solidity

// SPDX-License-Identifier: AGPL-3.0
pragma solidity ^0.8.0;

import {DataTypes} from '../protocol/libraries/types/DataTypes.sol';

/**
 * @title IReserveInterestRateStrategy
 * @author Aave
 * @notice Interface for the calculation of the interest rates
 */
interface IReserveInterestRateStrategy {
  /**
   * @notice Returns the base variable borrow rate
   * @return The base variable borrow rate, expressed in ray
   **/
  function getBaseVariableBorrowRate() external view returns (uint256);

  /**
   * @notice Returns the maximum variable borrow rate
   * @return The maximum variable borrow rate, expressed in ray
   **/
  function getMaxVariableBorrowRate() external view returns (uint256);

  /**
   * @notice Calculates the interest rates depending on the reserve's state and configurations
   * @param params The parameters needed to calculate interest rates
   * @return liquidityRate The liquidity rate expressed in rays
   * @return stableBorrowRate The stable borrow rate expressed in rays
   * @return variableBorrowRate The variable borrow rate expressed in rays
   **/
  function calculateInterestRates(DataTypes.CalculateInterestRatesParams memory params)
    external
    view
    returns (
      uint256,
      uint256,
      uint256
    );
}

```
{% endtab %}
{% endtabs %}