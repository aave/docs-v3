# L2Pool

The L2Pool.sol is the contract for the L2 optimized user facing methods of the protocol that takes byte encoded input arguments. It exposes the liquidity management methods that can be invoked using either Solidity or Web3 libraries.\


{% hint style="info" %}
Pool methods not exposed in L2Pool.sol (such as flashLoan, setUserEMode etc.) are same on L2 as on other versions of protocol. Refer [Pool](../../core-contracts/pool.md) docs for rest of the methods.&#x20;
{% endhint %}

{% hint style="info" %}
Since we have a limited set of supported assets that are already given individual id, we use the 16 bit asset id in the encoded arguments instead of 160 bit asset address.
{% endhint %}

## Methods

### supply

`function supply(bytes32 args) external`

Supplies asset into the protocol, minting the same amount of corresponding aTokens, and transferring them to `msg.sender`.

{% hint style="info" %}
You can use data returned from [`encodeSupplyParams`](l2encoder.md#encodesupplyparams) method in L2Encoder helper contract to pass to this method.
{% endhint %}

Call Params

| Name | Type      | Description                                                                                                                                                                                                                                                                                                  |
| ---- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| args | `bytes32` | <p>Encoded supply params<br>bit 0-15: id of the reserve. based on its position in the list of the active reserves<br>bit 16-143: <code>uint128</code> Shortened amount from original <code>uint256</code><br><code></code>bit 144-159: <code>uint16</code> referral code used for 3rd party integrations</p> |

### supplyWithPermit

`function supplyWithPermit(bytes32 args, bytes32 r, bytes32 s) external`

Supply with transfer approval of supplied asset via permit function. This method removes the need for separate approval tx before supplying asset to the pool.

{% hint style="info" %}
You can use data returned from [`encodeSupplyWithPermitParams`](l2encoder.md#encodesupplywithpermitparams) method in L2Encoder helper contract to pass to this method.
{% endhint %}

| Name | Type      | Description                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| ---- | --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| args | `bytes32` | <p>Encoded supply with permit params<br>bit 0-15: id of the reserve. based on its position in the list of the active reserves<br>bit 16-143: <code>uint128</code> shortened amount from original <code>uint256</code><br><code></code>bit 144-159: <code>uint16</code> referral code used for 3rd party integrations<br>bit 160-191: <code>uint32</code> shortened deadline from original <code>uint256</code><br>bit 192-199: Signature parameter v</p> |
| r    | `bytes32` | Signature parameter r                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| s    | `bytes32` | Signature parameter s                                                                                                                                                                                                                                                                                                                                                                                                                                    |

### withdraw

`function withdraw(bytes32 args) external`

Withdraws `amount` of the underlying `asset`, i.e. redeems the underlying token and burns the aTokens.

If user has any existing debt backed by the underlying token, then the max _amount_ available to withdraw is the _amount_ that will not leave user health factor < 1 after withdrawal.

{% hint style="info" %}
You can use data returned from [`encodeWithdrawParams`](l2encoder.md#encodewithdrawparams) method in L2Encoder helper contract to pass to this method.
{% endhint %}

| Name | Type      | Description                                                                                                                                                                                                   |
| ---- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| args | `bytes32` | <p>Encoded supply params<br>bit 0-15: id of the reserve. based on its position in the list of the active reserves<br>bit 16-143: <code>uint128</code> Shortened amount from original <code>uint256</code></p> |

### borrow

`function borrow(bytes32 args) external`

Borrows `amount` of `asset` with `interestRateMode`, sending the `amount` to `msg.sender`, with the debt being incurred by `onBehalfOf`.

{% hint style="info" %}
You can use data returned from [`encodeBorrowParams`](l2encoder.md#encodeborrowparams) method in L2Encoder helper contract to pass to this method.
{% endhint %}

| Name | Type      | Description                                                                                                                                                                                                                                                                                                                                                                    |
| ---- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| args | `bytes32` | <p>Encoded supply params<br>bit 0-15: id of the reserve. based on its position in the list of the active reserves<br>bit 16-143: <code>uint128</code> Shortened amount from original <code>uint256</code><br><code></code>bit 144 - 151: <code>uint8</code> shortened InterestRateMode<br>bit 152 - 167: <code>uint16</code> referral code used for 3rd party integrations</p> |

### repay

`function repay(bytes32 args) external returns (uint256)`

Repays debt of an `asset` for the given `rateMode`.

{% hint style="info" %}
You can use data returned from [`encodeRepayParams`](l2encoder.md#encoderepayparams) method in L2Encoder helper contract to pass to this method.
{% endhint %}

| Name | Type      | Description                                                                                                                                                                                                                                                                                |
| ---- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| args | `bytes32` | <p>Encoded supply params<br>bit 0-15: id of the reserve. based on its position in the list of the active reserves<br>bit 16-143: <code>uint128</code> Shortened amount from original <code>uint256</code><br><code></code>bit 144 - 151: <code>uint8</code> shortened InterestRateMode</p> |

### repayWithPermit

`function repayWithPermit(bytes32 args, bytes32 r, bytes32 s) external returns (uint256)`

Repay with transfer approval of borrowed asset via permit function. This method removes the need for separate approval tx before repaying asset to the pool.

{% hint style="info" %}
You can use data returned from [`encodeRepayWithPermitParams`](l2encoder.md#encoderepaywithpermitparams) method in L2Encoder helper contract to pass to this method.​
{% endhint %}

| Name | Type      | Description                                                                                                                                                                                                                                                                                                                                                                                                                          |
| ---- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| args | `bytes32` | <p>Encoded supply with permit params<br>bit 0-15: id of the reserve. based on its position in the list of the active reserves<br>bit 16-143: <code>uint128</code> shortened amount from original <code>uint256</code><br><code></code>bit 144-151: <code>uint8</code> shortened InterestRateMode<br>bit 152-183: <code>uint32</code> shortened deadline from original <code>uint256</code><br>bit 184-191: Signature parameter v</p> |
| r    | `bytes32` | Signature parameter r                                                                                                                                                                                                                                                                                                                                                                                                                |
| s    | `bytes32` | Signature parameter s                                                                                                                                                                                                                                                                                                                                                                                                                |

### repayWithATokens

`function repayWithATokens(bytes32 args) external override returns (uint256)`

Allows user to repay with _aTokens_ of the underlying debt asset without any approvals eg. Pay DAI debt using aDAI tokens.

{% hint style="info" %}
You can use data data returned from [encodeRepayWithATokensParams](l2encoder.md#encoderepayparams-1) method in L2Encoder helper contract to pass to this method.
{% endhint %}

| Name | Type      | Description                                                                                                                                                                                                                                                                                |
| ---- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| args | `bytes32` | <p>Encoded supply params<br>bit 0-15: id of the reserve. based on its position in the list of the active reserves<br>bit 16-143: <code>uint128</code> Shortened amount from original <code>uint256</code><br><code></code>bit 144 - 151: <code>uint8</code> shortened InterestRateMode</p> |

### swapBorrowRateMode

`function swapBorrowRateMode(bytes32 args) external`

Swaps `msg.sender`'s borrow rate mode between stable and variable.

{% hint style="info" %}
You can use data returned from [`encodeSwapBorrowRateMode`](l2encoder.md#encodeswapborrowratemode) method in L2Encoder helper contract to pass to this method.​
{% endhint %}

| Name | Type      | Description                                                                                                                                                                |
| ---- | --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| args | `bytes32` | <p>Encoded params<br>bit 0-15: id of the reserve. based on its position in the list of the active reserves<br>bit 16-23: <code>uint8</code> shortened InterestRateMode</p> |

### rebalanceStableBorrowRate

`function rebalanceStableBorrowRate(bytes32 args) external`

Rebalances stable borrow rate of the user for given asset. In case of liquidity crunches on the protocol, stable rate borrows might need to be rebalanced to bring back equilibrium between the borrow and supply rates.

{% hint style="info" %}
You can use data returned from [`encodeRebalanceStableBorrowRate`](l2encoder.md#encoderebalancestableborrowrate) method in L2Encoder helper contract to pass to this method.​
{% endhint %}



| Name | Type      | Description                                                                                                                                |
| ---- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| args | `bytes32` | <p>Encoded params<br>bit 0-15: id of the reserve. based on its position in the list of the active reserves<br>bit 16-175: user address</p> |

### setUserUseReserveAsCollateral

`function setUserUseReserveAsCollateral(bytes32 args) external`

Sets the asset of `msg.sender` to be used as collateral or not.

{% hint style="info" %}
You can use data returned from [`encodeSetUserUseReserveAsCollateral`](l2encoder.md#encodesetuserusereserveascollateral) method in L2Encoder helper contract to pass to this method.​
{% endhint %}

| Name | Type      | Description                                                                                                                                                                           |
| ---- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| args | `bytes32` | <p>Encoded params<br>bit 0-15: id of the reserve. based on its position in the list of the active reserves<br>bit 16: 0=> enable use as collateral, 1=> disable use as collateral</p> |

### liquidationCall

`function liquidationCall(bytes32 args1, bytes32 args2) external`

Liquidate positions with a **health factor** below 1.

{% hint style="info" %}
You can use data returned from [`encodeLiquidationCall`](l2encoder.md#liquidationcall) method in L2Encoder helper contract to pass to this method.​
{% endhint %}

| Name  | Type      | Description                                                                                                                                                                                                                                                              |
| ----- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| args1 | `bytes32` | <p>Encoded params<br>bit 0-15: id of the collateral reserve. based on its position in the list of the active reserves<br>bit 16-31: id of the debt reserve. based on its position in the list of the active reserves<br>bit 32-191: address of user being liquidated</p> |
| args2 | `bytes32` | <p>Encoded params<br>bit 0-127: <code>uint128</code> shortened debt to cover from the original <code>uint256</code><br>bit 128: 0=> receive aToken, 1=> receive underlying asset</p>                                                                                     |
