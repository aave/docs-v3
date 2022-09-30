# Pool

This contract is the main user facing contract.  Most user interactions with the Aave Protocol occur via the Pool contract. It exposes the liquidity management methods that can be invoked using either _**Solidity**_ or _**Web3**_ libraries. 

`Pool.sol` allows users to:
- Supply
- Withdraw
- Borrow
- Repay
- Swap their loans between variable and stable rate
- Enable/disable their supplied assets as collateral rebalance stable rate borrow positions
- Liquidate positions
- Execute Flash Loans

Pool is to be covered by a proxy contract, and is owned by the PoolAddressesProvider of the specific market. All admin functions are callable by the PoolConfigurator contract defined also in the PoolAddressesProvider.

## Write Methods

### initialize
```solidity
function initialize(IPoolAddressesProvider provider) external virtual initializer
```

Initializes the Pool.

Function is invoked by the proxy contract when the Pool contract is added to the PoolAddressesProvider of the market.

Caching the address of the PoolAddressesProvider in order to reduce gas consumption on subsequent operations.

#### Input Parameters:

| Name     | Type                     | Description                              |
| :------- | :----------------------- | :--------------------------------------- |
| provider | `IPoolAddressesProvider` | The address of the PoolAddressesProvider |

### supply

```solidity
function supply(
    address asset,
    uint256 amount,
    address onBehalfOf,
    uint16 referralCode
) public virtual override
```
Supplies a certain `amount` of an `asset` into the protocol, minting the same amount of corresponding aTokens and transferring them to the `onBehalfOf` address. For example, if a user supplies 100 USDC and onBehalfOf address is the same as `msg.sender`, they will get 100 aUSDC in return. 

The `referralCode` is emitted in Supply event and can be for third party referral integrations. To activate referral feature and obtain a unique referral code, integrators need to submit proposal to Aave Governance.

{% hint style="warning" %}
When supplying, the `Pool` contract must have `allowance()` to spend funds on behalf of `msg.sender` for at-least `amount` for the `asset` being supplied. This can be done via the standard ERC20 `approve()`method on the underlying token contract.
{% endhint %}

{% hint style="info" %}
Referral supply is currently inactive, you can pass `0` as `referralCode`. This program may be activated in the future through an Aave governance proposal.
{% endhint %}

#### Input Parameters:

| Name         | Type      | Description                                                                                                                                                                                                                                                                                                             |
| :----------- | :-------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| asset        | `address` | The address of the underlying asset being supplied to the pool                                                                                                                                                                                                                                                          |
| amount       | `uint256` | The amount of asset to be supplied                                                                                                                                                                                                                                                                                      |
| onBehalfOf   | `address` | The address that will receive the corresponding aTokens. This is the only address that will be able to withdraw the asset from the pool. This will be the same as msg.sender if the user wants to receive aTokens into their own wallet, or use a different address if the beneficiary of aTokens is a different wallet |
| referralCode | `uint16`  | Referral supply is currently inactive, you can pass `0`. This code is used to register the integrator originating the operation, for potential rewards. 0 if the action is executed directly by the user, without any middle-men                                                                                        |

### supplyWithPermit

```solidity
function supplyWithPermit(
    address asset,
    uint256 amount,
    address onBehalfOf,
    uint16 referralCode,
    uint256 deadline,
    uint8 permitV,
    bytes32 permitR,
    bytes32 permitS
) public virtual override
```

Supply with transfer approval of the asset to be supplied via permit function. This method removes the need for separate approval tx before supplying asset to the pool. See: https://eips.ethereum.org/EIPS/eip-2612.

{% hint style="info" %}
Permit signature must be signed by `msg.sender` with spender as Pool address.
{% endhint %}

{% hint style="info" %}
Referral program is currently inactive, you can pass `0` as `referralCode`. This program may be activated in the future through an Aave governance proposal.
{% endhint %}

#### Input Parameters:

| Name         | Type      | Description                                                                                                                                                                                                                      |
| :----------- | :-------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| asset        | `address` | The address of underlying asset being supplied. The same asset as used in permit v, s, and r                                                                                                                                     |
| amount       | `uint256` | The amount of asset to be supplied and signed for approval. The same amount as used in permit v, s, and r                                                                                                                        |
| onBehalfOf   | `address` | The address that will receive the aTokens. This will be the same as msg.sender if the user wants to receive aTokens into their own wallet, or use a different address if the beneficiary of aTokens is a different wallet        |
| referralCode | `uint16`  | Referral supply is currently inactive, you can pass `0`. This code is used to register the integrator originating the operation, for potential rewards. 0 if the action is executed directly by the user, without any middle-men |
| deadline     | `uint256` | The unix timestamp up until which the permit signature is valid                                                                                                                                                                  |
| permitV      | `uint8`   | The v parameter of the ERC712 permit signature                                                                                                                                                                                   |
| permitR      | `bytes32` | The r parameter of the ERC712 permit signature                                                                                                                                                                                   |
| permitS      | `bytes32` | The s parameter of the ERC712 permit signature                                                                                                                                                                                   |

### withdraw

```solidity
function withdraw(address asset, uint256 amount, address to) public virtual override returns (uint256)
```

Withdraws an `amount` of underlying `asset` from the reserve, burning the equivalent aTokens owned. For example, if a user has 100 aUSDC and calls withdraw(), they will receive 100 USDC, burning the 100 aUSDC.

If user has any existing debt backed by the underlying token, then the maximum `amount` available to withdraw is the `amount` that will not leave user's health factor < 1 after withdrawal.

{% hint style="info" %}
When withdrawing `to` another address, `msg.sender` should have `aToken` that will be burned by `Pool`.
{% endhint %}

#### Input Parameters:

| Name   | Type      | Description                                                                                                                                                                                                                  |
| :----- | :-------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| asset  | `address` | The address of the underlying asset to withdraw, not the aToken                                                                                                                                                              |
| amount | `uint256` | The underlying amount to be withdrawn (the amount deposited), expressed in wei units. Use `type(uint).max` to withdraw the entire aToken balance                                                                             |
| to     | `address` | The address that will receive the underlying `asset`. This will be the same as msg.sender if the user wants to receive the tokens into their own wallet, or use a different address if the beneficiary is a different wallet |

#### Return Values:

| Type      | Description                |
| :-------- | :------------------------- |
| `uint256` | The final amount withdrawn |

### borrow

```solidity
function borrow(
    address asset,
    uint256 amount,
    uint256 interestRateMode,
    uint16 referralCode,
    address onBehalfOf
) public virtual override 
```

Allows users to borrow a specific `amount` of the reserve underlying `asset`, provided the borrower has already supplied enough collateral, or they were given enough allowance by a credit delegator on the corresponding debt token (StableDebtToken or VariableDebtToken). For example, if a user borrows 100 USDC passing their own address as `onBehalfOf`, they will receive 100 USDC into their wallet and 100 stable/variable debt tokens, depending on the `interestRateMode`.

{% hint style="info" %}
NOTE: If `onBehalfOf` is not same as `msg.sender`, then `onBehalfOf` must have supplied enough collateral via `supply()` and have delegated credit to `msg.sender` via `approveDelegation()`.
{% endhint %}

{% hint style="info" %}
Referral program is currently inactive, you can pass `0` as `referralCode`. This program may be activated in the future through an Aave governance proposal.
{% endhint %}

#### Input Parameters:

| Name             | Type      | Description                                                                                                                                                                                                                                                       |
| :--------------- | :-------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| asset            | `address` | The address of the underlying asset to borrow                                                                                                                                                                                                                     |
| amount           | `uint256` | The amount to be borrowed, expressed in wei units                                                                                                                                                                                                                 |
| interestRateMode | `uint256` | The interest rate mode at which the user wants to borrow: 1 for Stable, 2 for Variable                                                                                                                                                                            |
| referralCode     | `uint16`  | Referral supply is currently inactive, you can pass `0`. This code is used to register the integrator originating the operation, for potential rewards. 0 if the action is executed directly by the user, without any middle-men                                  |
| onBehalfOf       | `address` | The address of the user who will incur the debt. This should be the address of the borrower calling the function if they want to borrow against their own collateral, or the address of the credit delegator, if they have been given credit delegation allowance |

### repay

```solidity
function repay(
    address asset,
    uint256 amount,
    uint256 interestRateMode,
    address onBehalfOf
) public virtual override returns (uint256)
```

Repays a borrowed `amount` on a specific reserve, burning the equivalent debt tokens owned. For example, if a user repays 100 USDC, the 100 variable/stable debt tokens owned by the `onBehalfOf` address will be burned.

{% hint style="warning" %}
When repaying, the `Pool` contract must have `allowance()` to spend funds on behalf of `msg.sender` for at-least the `amount` for the asset you are repaying with. This can be done via the standard ERC20 `approve()`method on the underlying token contract.
{% endhint %}

{% hint style="info" %}
You cannot call `repay()` multiple times on the same block.
{% endhint %}

#### Input Parameters:

| Name             | Type      | Description                                                                                                                                                                                                                                                                                                   |
| :--------------- | :-------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| asset            | `address` | The address of the borrowed underlying asset previously borrowed                                                                                                                                                                                                                                              |
| amount           | `uint256` | The amount to repay, expressed in wei units. Use `type(uint256).max` in order to repay the whole debt, ONLY when the repayment is not executed on behalf of a 3rd party. In case of repayments on behalf of another user, it's recommended to send an amount slightly higher than the current borrowed amount |
| interestRateMode | `uint256` | The interest rate mode of the debt the user wants to repay: 1 for Stable, 2 for Variable                                                                                                                                                                                                                      |
| onBehalfOf       | `address` | The address of the user who will get their debt reduced/removed. This should be the address of the user calling the function if they want to reduce/remove their own debt, or the address of any other borrower whose debt should be removed                                                                  |

#### Return Values:

| Type      | Description             |
| :-------- | :---------------------- |
| `uint256` | The final amount repaid |

### repayWithPermit

```solidity
function repayWithPermit(
    address asset,
    uint256 amount,
    uint256 interestRateMode,
    address onBehalfOf,
    uint256 deadline,
    uint8 permitV,
    bytes32 permitR,
    bytes32 permitS
) public virtual override returns (uint256)
```

Repay with transfer approval of the borrowed asset to be repaid, done via permit function. This method removes the need for separate approval tx before repaying asset to the pool. See: https://eips.ethereum.org/EIPS/eip-2612.

{% hint style="info" %}
Permit signature must be signed by `msg.sender` with spender value as `Pool` address.
{% endhint %}

#### Input Parameters:

| Name             | Type      | Description                                                                                                                                                                                                                                  |
| :--------------- | :-------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| asset            | `address` | The address of the borrowed underlying asset previously borrowed. The same asset as used in permit v, r, and s                                                                                                                               |
| amount           | `uint256` | The amount to repay, expressed in wei units. Use `type(uint256).max` in order to repay the whole debt to pay without leaving aToken dust. The same amount as used in permit s,v,r                                                        |
| interestRateMode | `uint256` | The interest rate mode at of the debt the user wants to repay: 1 for Stable, 2 for Variable                                                                                                                                                  |
| onBehalfOf       | `address` | The address of the user who will get their debt reduced/removed. This should be the address of the user calling the function if they want to reduce/remove their own debt, or the address of any other borrower whose debt should be removed |
| deadline         | `uint256` | The unix timestamp up until which the permit signature is valid                                                                                                                                                                              |
| permitV          | `uint8`   | The v parameter of the ERC712 permit signature                                                                                                                                                                                               |
| permitR          | `bytes32` | The r parameter of the ERC712 permit signature                                                                                                                                                                                               |
| permitS          | `bytes32` | The s parameter of the ERC712 permit signature                                                                                                                                                                                               |

#### Return Values:

| Type      | Description             |
| :-------- | :---------------------- |
| `uint256` | The final amount repaid |

### repayWithATokens

```solidity
function repayWithATokens(address asset, uint256 amount, uint256 interestRateMode
) public virtual override returns (uint256)
```

Allows a user to repay a borrowed `amount` on a specific reserve using the reserve aTokens, burning the equivalent debt tokens. For example, a user repays 100 USDC using 100 aUSDC, burning 100 variable/stable debt tokens. Passing `uint256.max`as the amount will clean up any residual aToken dust balance, if the user aToken balance is not enough to cover the whole debt.

#### Input Parameters:

| Name             | Type      | Description                                                                                                                  |
| :--------------- | :-------- | :--------------------------------------------------------------------------------------------------------------------------- |
| asset            | `address` | The address of the borrowed underlying asset previously borrowed                                                             |
| amount           | `uint256` | The amount to repay. Use `type(uint256).max` in order to repay the whole debt for `asset` to pay without leaving aToken dust |
| interestRateMode | `uint256` | The interest rate mode at of the debt the user wants to repay: 1 for Stable, 2 for Variable                                  |

#### Return Values:

| Type      | Description             |
| :-------- | :---------------------- |
| `uint256` | The final amount repaid |

### swapBorrowRateMode

```solidity
function swapBorrowRateMode(address asset, uint256 interestRateMode) public virtual override
```

Allows a borrower (`msg.sender`) to swap their debt between stable and variable mode, or vice versa.

#### Input Parameters:

| Name             | Type      | Description                                                                                |
| :--------------- | :-------- | :----------------------------------------------------------------------------------------- |
| asset            | `address` | The address of the underlying asset borrowed                                               |
| interestRateMode | `uint256` | The current interest rate mode of the position being swapped: 1 for Stable, 2 for Variable |

### rebalanceStableBorrowRate

```solidity
function rebalanceStableBorrowRate(address asset, address user) public virtual override
```

Rebalances the stable interest rate of a `user` for given `asset` to the current stable rate defined on the reserve. In case of liquidity crunches on the protocol, stable rate borrows might need to be rebalanced to bring back equilibrium between the borrow and supply rates.

Users can be rebalanced if the following conditions are satisfied:
1. Usage ratio is above 95%
2. The current supply APY is below REBALANCE_UP_THRESHOLD * maxVariableBorrowRate, which means that too much has been borrowed at a stable rate and suppliers are not earning enough

#### Input Parameters:

| Name  | Type      | Description                                                                             |
| :---- | :-------- | :-------------------------------------------------------------------------------------- |
| asset | `address` | The address of the underlying asset borrowed for which the position is being rebalanced |
| user  | `address` | The address of the user to be rebalanced                                                |

### setUserUseReserveAsCollateral

```solidity
function setUserUseReserveAsCollateral(address asset, bool useAsCollateral) public virtual override
```

Allows suppliers to enable/disable a specific supplied asset as collateral. Sets the `asset` of `msg.sender` to be used as collateral or not.

{% hint style="info" %}
An asset in [Isolation Mode](../whats-new/isolation-mode.md#isolation-mode) can be enabled to use as collateral only if no other asset is already enabled to use as collateral.
{% endhint %}

{% hint style="info" %}
The user won’t be able to disable an asset as collateral if they have an outstanding debt position which could be left with the Health Factor < `HEALTH_FACTOR_LIQUIDATION_THRESHOLD` on disabling the given asset as collateral.
{% endhint %}

#### Input Parameters:

| Name            | Type      | Description                                                             |
| :-------------- | :-------- | :---------------------------------------------------------------------- |
| asset           | `address` | The address of the underlying asset supplied                            |
| useAsCollateral | `bool`    | True if the user wants to use the supply as collateral, false otherwise |

### liquidationCall

```solidity
function liquidationCall(
    address collateralAsset,
    address debtAsset,
    address user,
    uint256 debtToCover,
    bool receiveAToken
) public virtual override
```

Function to liquidate a non-healthy position collateral-wise, with Health Factor below 1

When the health factor of a position is below 1, the caller (liquidator) repays the `debtToCover` amount of debt of the user getting liquidated. This is part or all of the outstanding borrowed amount on behalf of the borrower. The caller then receives a proportional amount of the `collateralAsset` (discounted amount of collateral) plus a liquidation bonus to cover market risk. 

Liquidators can decide if they want to receive an equivalent amount of collateral aTokens instead of the underlying asset. When the liquidation is completed successfully, the health factor of the position is increased, bringing the health factor above 1.

Liquidators can only close a certain amount of collateral defined by a close factor. Currently the **close factor is 0.5**. In other words, liquidators can only liquidate a maximum of 50% of the amount pending to be repaid in a position. The liquidation discount applies to this amount.

* _In most scenarios_, profitable liquidators will choose to liquidate as much as they can (50% of the `user` position).
* `debtToCover` parameter can be set to `uint(-1)` and the protocol will proceed with the highest possible liquidation allowed by the close factor.
* To check a user's health factor, use [`getUserAccountData()`.](pool.md#getuseraccountdata)

{% hint style="info" %}
Liquidators must `approve()` the `Pool` contract to use `debtToCover` of the underlying ERC20 of the `asset` used for the liquidation.
{% endhint %}

#### Input Parameters:

| Name            | Type      | Description                                                                                                                                                        |
| :-------------- | :-------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| collateralAsset | `address` | The address of the underlying asset used as collateral, to receive as result of the liquidation                                                                    |
| debtAsset       | `address` | The address of the underlying borrowed asset to be repaid with the liquidation                                                                                     |
| user            | `address` | The address of the borrower getting liquidated                                                                                                                     |
| debtToCover     | `uint256` | The debt amount of borrowed `asset` the liquidator will repay                                                                                                      |
| receiveAToken   | `bool`    | True if the liquidator wants to receive the aTokens equivalent of the purchased collateral, false if they want to receive the underlying collateral asset directly |

### flashLoan

```solidity
function flashLoan(
    address receiverAddress,
    address[] calldata assets,
    uint256[] calldata amounts,
    uint256[] calldata interestRateModes,
    address onBehalfOf,
    bytes calldata params,
    uint16 referralCode
) public virtual override
```

Allows users to access liquidity of the pool for a given list of assets within one transaction, as long as the amount taken plus a fee is returned. The receiver must approve the `Pool` contract for at least the _amount borrowed + fee_, otherwise the transaction will revert.

The flash loan fee is waived for approved `FLASH_BORROWER`.

{% hint style="warning" %}
There are security concerns for developers of flashloan receiver contracts that must be kept into consideration. For further details please visit https://developers.aave.com
{% endhint %}

{% hint style="info" %}
Referral program is currently inactive, you can pass `0` as `referralCode`. This program may be activated in the future through an Aave governance proposal.
{% endhint %}

#### Input Parameters:

| Name              | Type        | Description                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| :---------------- | :---------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| receiverAddress   | `address`   | The address of the contract receiving the flash-borrowed funds, implementing the IFlashLoanReceiver interface                                                                                                                                                                                                                                                                                                                                   |
| assets            | `address[]` | The addresses of the assets being flash-borrowed                                                                                                                                                                                                                                                                                                                                                                                                |
| amounts           | `uint256[]` | The amounts of the assets being flash-borrowed. This needs to contain the same number of enteries as assets                                                                                                                                                                                                                                                                                                                                     |
| interestRateModes | `uint256[]` | <p>The types of the debt position to open if the flash loan is not returned:<br>0 -> Don't open any debt, the amount + fee must be paid in this case or just revert if the funds can't be transferred from the receiver<br>1 -> Open debt at a stable rate for the value of the amount flash-borrowed to the `onBehalfOf` address<br>2 -> Open debt at variable rate for the value of the amount flash-borrowed to the `onBehalfOf` address</p> |
| onBehalfOf        | `address`   | The address that will receive the debt if the associated `interestRateModes` is 1 or 2. `onBehalfOf` must already have approved sufficient borrow allowance of the associated asset to `msg.sender`                                                                                                                                                                                                                                             |
| params            | `bytes`     | Variadic packed params to pass to the receiver as extra information                                                                                                                                                                                                                                                                                                                                                                             |
| referralCode      | `uint16`    | Referral supply is currently inactive, you can pass `0`. This code is used to register the integrator originating the operation, for potential rewards. 0 if the action is executed directly by the user, without any middle-men                                                                                                                                                                                                                |

### flashLoanSimple

```solidity
function flashLoanSimple(
    address receiverAddress,
    address asset,
    uint256 amount,
    bytes calldata params,
    uint16 referralCode
) public virtual override
```

Allows users to access liquidity of the pool for a given asset within one transaction, as long as the amount taken plus a fee is returned. The receiver must approve the `Pool` contract for at least the _amount borrowed + fee_, otherwise the transaction will revert.

This function does not waive the fee for approved `FLASH_BORROWER`, nor does it allow for opening a debt position instead of repaying.

{% hint style="warning" %}
There are security concerns for developers of flashloan receiver contracts that must be kept into consideration. For further details please visit https://developers.aave.com
{% endhint %}

{% hint style="info" %}
Referral program is currently inactive, you can pass `0` as `referralCode`. This program may be activated in the future through an Aave governance proposal.
{% endhint %}

#### Input Parameters:

| Name            | Type      | Description                                                                                                                                                                                                                      |
| :-------------- | :-------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| receiverAddress | `address` | The address of the contract receiving the flash-borrowed funds, implementing the IFlashLoanReceiver interface                                                                                                                    |
| asset           | `address` | The address of the asset being flash-borrowed                                                                                                                                                                                    |
| amount          | `uint256` | The amount of the asset being flash-borrowed                                                                                                                                                                                     |
| params          | `bytes`   | Variadic packed params to pass to the receiver as extra information                                                                                                                                                              |
| referralCode    | `uint16`  | Referral supply is currently inactive, you can pass `0`. This code is used to register the integrator originating the operation, for potential rewards. 0 if the action is executed directly by the user, without any middle-men |

### mintToTreasury

```solidity
function mintToTreasury(address[] calldata assets) external virtual override
```

Mints the assets accrued through the reserve factor to the treasury in the form of aTokens for the given list of assets.

#### Input Parameters:

| Name   | Type        | Description                                                     |
| :----- | :---------- | :-------------------------------------------------------------- |
| assets | `address[]` | The list of reserves for which the minting needs to be executed |

### finalizeTransfer

```solidity
function finalizeTransfer(
    address asset,
    address from,
    address to,
    uint256 amount,
    uint256 balanceFromBefore,
    uint256 balanceToBefore
) external virtual override
```

Validates and finalizes an aToken transfer. It is only callable by the overlying aToken of the `asset`.

#### Input Parameters:

| Name              | Type      | Description                                               |
| :---------------- | :-------- | :-------------------------------------------------------- |
| asset             | `address` | The address of the underlying asset of the aToken         |
| from              | `address` | The user from which the aTokens are transferred           |
| to                | `address` | The user receiving the aTokens                            |
| amount            | `uint256` | The amount being transferred/withdrawn                    |
| balanceFromBefore | `uint256` | The aToken balance of the `from` user before the transfer |
| balanceToBefore   | `uint256` | The aToken balance of the `to` user before the transfer   |

### setUserEMode

```solidity
function setUserEMode(uint8 categoryId) external virtual override 
```

Allows a user to use the protocol in efficiency mode. The category id must be a valid id already defined by _Pool or Risk Admins_.

{% hint style="info" %}
Will revert if user is borrowing non-compatible asset or if the change will drop the Health Factor < `HEALTH_FACTOR_LIQUIDATION_THRESHOLD`.
{% endhint %}

#### Input Parameters:

| Name       | Type    | Description                                                                                                   |
| :--------- | :------ | :------------------------------------------------------------------------------------------------------------ |
| categoryId | `uint8` | The eMode category id (0 - 255) defined by Risk or Pool Admins. `categoryId` set to 0 is a non eMode category |

### mintUnbacked

```solidity
function mintUnbacked(
    address asset,
    uint256 amount,
    address onBehalfOf,
    uint16 referralCode
) external virtual override onlyBridge
```

Allows contracts, with `BRIDGE` role permission, to mint an `amount` of unbacked aTokens to the `onBehalfOf` address. This method is part of the V3 [Portal](../whats-new/portal.md) feature.

{% hint style="info" %}
Only available to the addresses with `BRIDGE` role. Bridge addresses can be whitelisted by Aave governance.
{% endhint %}

{% hint style="info" %}
Referral program is currently inactive, you can pass `0` as `referralCode`. This program may be activated in the future through an Aave governance proposal.
{% endhint %}

#### Input Parameters:

| Name         | Type      | Description                                                                                                                                                                                                                      |
| :----------- | :-------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| asset        | `address` | The address of the underlying asset to mint                                                                                                                                                                                      |
| amount       | `uint256` | The amount to mint                                                                                                                                                                                                               |
| onBehalfOf   | `address` | The address that will receive the aTokens                                                                                                                                                                                        |
| referralCode | `uint16`  | Referral supply is currently inactive, you can pass `0`. This code is used to register the integrator originating the operation, for potential rewards. 0 if the action is executed directly by the user, without any middle-men |

### backUnbacked

```solidity
function backUnbacked(address asset, uint256 amount, uint256 fee) external virtual override onlyBridge
  ```

Allows contracts, with `BRIDGE` role permission, to back the current unbacked underlying aTokens with `amount` and pay `fee`. This method is part of the V3 [Portal](../whats-new/portal.md) feature.

{% hint style="info" %}
Only available to the addresses with `BRIDGE` role. Bridge addresses can be whitelisted by the governance.
{% endhint %}

#### Input Parameters:

| Name   | Type      | Description                                              |
| :----- | :-------- | :------------------------------------------------------- |
| asset  | `address` | The address of the underlying asset to back              |
| amount | `uint256` | The amount of asset supplied to back the unbacked tokens |
| fee    | `uint256` | The amount paid in fees                                  |

### initReserve

```solidity
function initReserve(
    address asset,
    address aTokenAddress,
    address stableDebtAddress,
    address variableDebtAddress,
    address interestRateStrategyAddress
) external virtual override onlyPoolConfigurator
```

Initializes a reserve, activating it, assigning an aToken and debt tokens and an interest rate strategy. 

{% hint style="info" %}
Only callable by the `PoolConfigurator` contract.
{% endhint %}

#### Input Parameters:

| Name                        | Type      | Description                                                               |
| :-------------------------- | :-------- | :------------------------------------------------------------------------ |
| asset                       | `address` | The address of the underlying asset of the reserve                        |
| aTokenAddress               | `address` | The address of the aToken that will be assigned to the reserve            |
| stableDebtAddress           | `address` | The address of the StableDebtToken that will be assigned to the reserve   |
| variableDebtAddress         | `address` | The address of the VariableDebtToken that will be assigned to the reserve |
| interestRateStrategyAddress | `address` | The address of the interest rate strategy contract                        |

### dropReserve

```solidity
function dropReserve(address asset) external virtual override onlyPoolConfigurator
```

Drop a reserve.

{% hint style="info" %}
Only callable by the `PoolConfigurator` contract.
{% endhint %}

#### Input Parameters:

| Name  | Type      | Description                                        |
| :---- | :-------- | :------------------------------------------------- |
| asset | `address` | The address of the underlying asset of the reserve |

### setReserveInterestRateStrategyAddress

```solidity
function setReserveInterestRateStrategyAddress(address asset, address rateStrategyAddress) external virtual override onlyPoolConfigurator
```

Updates the address of the interest rate strategy contract.

{% hint style="info" %}
Only callable by the `PoolConfigurator` contract.
{% endhint %}

#### Input Parameters:

| Name                | Type      | Description                                        |
| :------------------ | :-------- | :------------------------------------------------- |
| asset               | `address` | The address of the underlying asset of the reserve |
| rateStrategyAddress | `address` | The address of the interest rate strategy contract |

### setConfiguration

```solidity
function setConfiguration(address asset, DataTypes.ReserveConfigurationMap calldata configuration) external virtual override onlyPoolConfigurator    
```

Sets the configuration bitmap of the reserve as a whole. 

{% hint style="info" %}
Only callable by the `PoolConfigurator` contract.
{% endhint %}

#### Input Parameters:

| Name  | Type      | Description                                                                                                                                                                    |
| :---- | :-------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| asset | `address` | The address of the underlying asset of the reserve                                                                                                                             |
| data  | `uint256` | <p>The new configuration bitmap<br>bit 0-15: LTV<br>bit 16-31: Liquidation threshold<br>bit 32-47: Liquidation bonus<br>bit 48-55: Decimals<br>bit 56: reserve is active<br>bit 57: reserve is frozen<br>bit 58: borrowing is enabled<br>bit 59: stable rate borrowing enabled<br>bit 60: asset is paused<br>bit 61: borrowing in isolation mode is enabled<br>bit 62-63: reserved<br>bit 64-79: reserve factor<br>bit 80-115: borrow cap in whole tokens, 0 ⇒ no cap<br>bit 116-151: supply cap in whole tokens, 0 ⇒ no cap<br>bit 152-167: liquidation protocol fee<br>bit 168-175: eMode category<br>bit 176-211: unbacked mint cap in whole tokens, 0 ⇒ no cap<br>bit 212-251: debt ceiling for isolation mode with decimals bit 252-255: unused</p> |

### updateBridgeProtocolFee

```solidity
function updateBridgeProtocolFee(uint256 protocolFee) external virtual override onlyPoolConfigurator
```

Updates the protocol fee on the bridging. 

{% hint style="info" %}
Only callable by the `PoolConfigurator` contract.
{% endhint %}

#### Input Parameters:

| Name        | Type      | Description                                           |
| :---------- | :-------- | :---------------------------------------------------- |
| protocolFee | `uint256` | The part of the premium sent to the protocol treasury |

### updateFlashloanPremiums

```solidity
function updateFlashloanPremiums(uint128 flashLoanPremiumTotal, uint128 flashLoanPremiumToProtocol) external virtual override onlyPoolConfigurator
```

Updates flash loan premiums. A flash loan premium consists of two parts:
- A part is sent to aToken holders as extra, one time accumulated interest
- A part is collected by the protocol treasury

The total premium is calculated on the total borrowed amount. The premium to protocol is calculated on the total premium, being a percentage of `flashLoanPremiumTotal`.

{% hint style="info" %}
Only callable by the `PoolConfigurator` contract.
{% endhint %}

#### Input Parameters:

| Name                       | Type      | Description                                                             |
| :------------------------- | :-------- | :---------------------------------------------------------------------- |
| flashLoanPremiumTotal      | `uint128` | The total premium, expressed in bps                                     |
| flashLoanPremiumToProtocol | `uint128` | The part of the premium sent to the protocol treasury, expressed in bps |

### configureEModeCategory

```solidity
function configureEModeCategory(uint8 id, DataTypes.EModeCategory memory category) external virtual override onlyPoolConfigurator
```

Configures a new category for the eMode. In eMode, the protocol allows very high borrowing power to borrow assets of the same category. The category 0 is reserved for volatile heterogeneous assets and it's always disabled.

Each eMode category has a custom ltv and liquidation threshold. Each eMode category may or may not have a custom oracle to override the individual assets price oracles.

{% hint style="info" %}
Only callable by the `PoolConfigurator` contract.
{% endhint %}

#### Input Parameters:

| Name                 | Type      | Description                                                 |
| :------------------- | :-------- | :---------------------------------------------------------- |
| id                   | `uint8`   | The total premium, expressed in bps                         |
| ltv                  | `uint16`  | The custom Loan to Value for the new eMode category         |
| liquidationThreshold | `uint16`  | The custom liquidation threshold for the new eMode category |
| liquidationBonus     | `uint16`  | The liquidation bonus for the new eMode category            |
| priceSource          | `address` | The custom price source for the new eMode category          |
| label                | `string`  | The custom label describing the new eMode category          |

### resetIsolationModeTotalDebt

```solidity
function resetIsolationModeTotalDebt(address asset) external virtual override onlyPoolConfigurator
```

Resets the isolation mode total debt of the given asset to zero. It requires the given asset to have a zero debt ceiling.

{% hint style="info" %}
Only callable by the `PoolConfigurator` contract.
{% endhint %}

#### Input Parameters:

| Name  | Type      | Description                                                             |
| :---- | :-------- | :---------------------------------------------------------------------- |
| asset | `address` | The address of the underlying asset to reset the isolationModeTotalDebt |

### rescueTokens

```solidity
function rescueTokens(address token, address to, uint256 amount) external virtual override onlyPoolAdmin
``` 

Rescue and transfer tokens locked in this contract.

{% hint style="info" %}
Only available to `POOL_ADMIN` role. Pool admin is selected by the governance.
{% endhint %}

#### Input Parameters:

| Name   | Type      | Description                     |
| :----- | :-------- | :------------------------------ |
| token  | `address` | The address of the token        |
| to     | `address` | The address of the recipient    |
| amount | `uint256` | The amount of token to transfer |

## View Methods

### getUserAccountData

```solidity
function getUserAccountData(address user) external view virtual override returns (
    uint256 totalCollateralBase,
    uint256 totalDebtBase,
    uint256 availableBorrowsBase,
    uint256 currentLiquidationThreshold,
    uint256 ltv,
    uint256 healthFactor
)
```

Returns the user account data across all the reserves

#### Input Parameters:

| Name | Type      | Description             |
| :--- | :-------  | :---------------------- |
| user | `address` | The address of the user |

#### Return Values:

| Name                        | Type      | Description                                                                      |
| :-------------------------- | :-------- | :------------------------------------------------------------------------------- |
| totalCollateralBase         | `uint256` | The total collateral of the user in the base currency used by the price feed     |
| totalDebtBase               | `uint256` | The total debt of the user in the base currency used by the price feed           |
| availableBorrowsBase        | `uint256` | The borrowing power left of the user in the base currency used by the price feed |
| currentLiquidationThreshold | `uint256` | The liquidation threshold of the user                                            |
| ltv                         | `uint256` | The loan to value of the user                                                    |
| healthFactor                | `uint256` | The current health factor of the user                                            |

### getConfiguration

```solidity
function getConfiguration(address asset) external view virtual override returns (DataTypes.ReserveConfigurationMap memory)
```

Returns the configuration of the reserve.

#### Input Parameters:

| Name  | Type      | Description                                        |
| :---- | :-------- | :------------------------------------------------- |
| asset | `address` | The address of the underlying asset of the reserve |

#### Return Values:

| Type      | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| :-------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `uint256` | <p>The new configuration bitmap<br>bit 0-15: LTV<br>bit 16-31: Liquidation threshold<br>bit 32-47: Liquidation bonus<br>bit 48-55: Decimals<br>bit 56: reserve is active<br>bit 57: reserve is frozen<br>bit 58: borrowing is enabled<br>bit 59: stable rate borrowing enabled<br>bit 60: asset is paused<br>bit 61: borrowing in isolation mode is enabled<br>bit 62-63: reserved<br>bit 64-79: reserve factor<br>bit 80-115: borrow cap in whole tokens, 0 ⇒ no cap<br>bit 116-151: supply cap in whole tokens, 0 ⇒ no cap<br>bit 152-167: liquidation protocol fee<br>bit 168-175: eMode category<br>bit 176-211: unbacked mint cap in whole tokens, 0 ⇒ no cap<br>bit 212-251: debt ceiling for isolation mode with decimals bit 252-255: unused</p> |

### getUserConfiguration

```solidity
function getUserConfiguration(address user) external view virtual override returns (DataTypes.UserConfigurationMap memory)
```

Returns the configuration of the user across all the reserves.

#### Input Parameters:

| Name | Type      | Description      |
| :--- | :-------- | :--------------- |
| user | `address` | The user address |

#### Return Values:

| Type      | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| :-------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `uint256` | <p>The new configuration bitmap<br>bit 0-15: LTV<br>bit 16-31: Liquidation threshold<br>bit 32-47: Liquidation bonus<br>bit 48-55: Decimals<br>bit 56: reserve is active<br>bit 57: reserve is frozen<br>bit 58: borrowing is enabled<br>bit 59: stable rate borrowing enabled<br>bit 60: asset is paused<br>bit 61: borrowing in isolation mode is enabled<br>bit 62-63: reserved<br>bit 64-79: reserve factor<br>bit 80-115: borrow cap in whole tokens, 0 ⇒ no cap<br>bit 116-151: supply cap in whole tokens, 0 ⇒ no cap<br>bit 152-167: liquidation protocol fee<br>bit 168-175: eMode category<br>bit 176-211: unbacked mint cap in whole tokens, 0 ⇒ no cap<br>bit 212-251: debt ceiling for isolation mode with decimals bit 252-255: unused</p> |

### getReserveNormalizedIncome

```solidity
function getReserveNormalizedIncome(address asset) external view virtual override returns (uint256)
```

Returns the ongoing normalized income for the reserve.

A value of 1e27 means there is no income. As time passes, the yield is accrued. A value of 2\*1e27 means for each unit of asset one unit of income has been accrued.

#### Input Parameters:

| Name  | Type      | Description                                        |
| :---- | :-------- | :------------------------------------------------- |
| asset | `address` | The address of the underlying asset of the reserve |

#### Return Values:

| Type      | Description                     |
| :-------- | :------------------------------ |
| `uint256` | The reserve's normalized income |

### getReserveNormalizedVariableDebt

```solidity
function getReserveNormalizedVariableDebt(address asset) external view virtual override returns (uint256)
```

Returns the normalized variable debt per unit of asset.

A value of 1e27 means there is no debt. As time passes, the debt is accrued. A value of 2\*1e27 means that for each unit of debt, one unit worth of interest has been accumulated.

#### Input Parameters:

| Name  | Type      | Description                                        |
| :---- | :-------- | :------------------------------------------------- |
| asset | `address` | The address of the underlying asset of the reserve |

#### Return Values:

| Type      | Description                          |
| :-------- | :----------------------------------- |
| `uint256` | The reserve normalized variable debt |

### getReserveData

```solidity
function getReserveData(address asset) external view virtual override returns (DataTypes.ReserveData memory)
```

Returns the state and configuration of the reserve.

#### Input Parameters:

| Name  | Type      | Description                                        |
| :---- | :-------- | :------------------------------------------------- |
| asset | `address` | The address of the underlying asset of the reserve |

#### Return Values:

| Name                        | Type      | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| :-------------------------- | :-------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| configuration               | `uint256` | <p>The new configuration bitmap<br>bit 0-15: LTV<br>bit 16-31: Liquidation threshold<br>bit 32-47: Liquidation bonus<br>bit 48-55: Decimals<br>bit 56: reserve is active<br>bit 57: reserve is frozen<br>bit 58: borrowing is enabled<br>bit 59: stable rate borrowing enabled<br>bit 60: asset is paused<br>bit 61: borrowing in isolation mode is enabled<br>bit 62-63: reserved<br>bit 64-79: reserve factor<br>bit 80-115: borrow cap in whole tokens, 0 ⇒ no cap<br>bit 116-151: supply cap in whole tokens, 0 ⇒ no cap<br>bit 152-167: liquidation protocol fee<br>bit 168-175: eMode category<br>bit 176-211: unbacked mint cap in whole tokens, 0 ⇒ no cap<br>bit 212-251: debt ceiling for isolation mode with decimals bit 252-255: unused</p> |
| liquidityIndex              | `uint128` | The yield generated by the reserve during time interval since lastUpdatedTimestamp. Expressed in ray                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| currentLiquidityRate        | `uint128` | The current supply rate. Expressed in ray                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| variableBorrowIndex         | `uint128` | The yield accrued by reserve during time interval since lastUpdatedTimestamp. Expressed in ray                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| currentVariableBorrowRate   | `uint128` | The current variable borrow rate. Expressed in ray                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| currentStableBorrowRate     | `uint128` | The current stable borrow rate. Expressed in ray                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| lastUpdateTimestamp         | `uint40`  | The timestamp of when reserve data was last updated. Used for yield calculation                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| id                          | `uint16`  | The id of the reserve. It represents the reserve’s position in the list of active reserves                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| aTokenAddress               | `address` | The address of associated aToken                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| stableDebtTokenAddress      | `address` | The address of associated stable debt token                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| variableDebtTokenAddress    | `address` | The address of associated variable debt token                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| interestRateStrategyAddress | `address` | The address of interest rate strategy                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| accruedToTreasury           | `uint128` | The current treasury balance (scaled)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| unbacked                    | `uint128` | The outstanding unbacked aTokens minted through the bridging feature                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| isolationModeTotalDebt      | `uint128` | The outstanding debt borrowed against this asset in isolation mode                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |

### getReservesList

```solidity
function getReservesList() external view virtual override returns (address[] memory) 
```

Returns the list of the underlying assets of all the initialized reserves. It does not include dropped reserves.

#### Return Values:

| Type        | Description                                                        |
| :---------- | :----------------------------------------------------------------- |
| `address[]` | The addresses of the underlying assets of the initialized reserves |

### getReserveAddressById

```solidity
function getReserveAddressById(uint16 id) external view returns (address)
```

Returns the address of the underlying asset of a reserve by the reserve id as stored in the DataTypes.ReserveData struct.

#### Input Parameters:

| Name | Type     | Description                                                         |
| :--- | :------- | :------------------------------------------------------------------ |
| id   | `uint16` | The id of the reserve as stored in the DataTypes.ReserveData struct |

#### Return Values:

| Type      | Description                                   |
| :-------- | :-------------------------------------------- |
| `address` | The address of the reserve associated with id |

### getEModeCategoryData

```solidity
function getEModeCategoryData(uint8 id) external view virtual override returns (DataTypes.EModeCategory memory)
```

Returns the data of an eMode category.

Each eMode category has a custom ltv and liquidation threshold. Each eMode category may or may not have a custom oracle to override the individual assets price oracles.

#### Input Parameters:

| Name | Type    | Description            |
| :--- | :------ | :--------------------- |
| id   | `uint8` | The id of the category |

#### Return Values:

| Name                 | Type      | Description                                              |
| :------------------- | :-------- | :------------------------------------------------------- |
| ltv                  | `uint16`  | The custom Loan to Value for the  eMode category         |
| liquidationThreshold | `uint16`  | The custom liquidation threshold for the  eMode category |
| liquidationBonus     | `uint16`  | The liquidation bonus for the  eMode category            |
| priceSource          | `address` | The custom price source for the  eMode category          |
| label                | `string`  | The custom label describing the  eMode category          |

### getUserEMode

```solidity
function getUserEMode(address user) external view virtual override returns (uint256)
```

Returns eMode the user is using. 0 is a non eMode category.

#### Input Parameters:

| Name | Type      | Description            |
| :--- | :-------- | :--------------------- |
| user | `address` | The address of the user |

#### Return Values:

| Name | Type      | Description  |
| :--- | :-------- | :----------- |
| id   | `uint256` | The eMode id |

### MAX_STABLE_RATE_BORROW_SIZE_PERCENT

```solidity
function MAX_STABLE_RATE_BORROW_SIZE_PERCENT() public view virtual override returns (uint256)
```

Returns the percentage of available liquidity that can be borrowed at once at stable rate.

#### Return Values:

| Type      | Description                                                       |
| :-------- | :---------------------------------------------------------------- |
| `uint256` | The percentage of available liquidity to borrow, expressed in bps |

### FLASHLOAN\_PREMIUM\_TOTAL

```solidity
function FLASHLOAN_PREMIUM_TOTAL() public view virtual override returns (uint128)
```

Returns the percent of total flashloan premium paid by the borrower.

A part of this premium is added to reserve's liquidity index i.e. paid to the liquidity provider and the other part is paid to the protocol i.e. accrued to the treasury.

#### Return Values:

| Type      | Description                 |
| :-------- | :-------------------------- |
| `uint128` | The total fee on flashloans |

### BRIDGE_PROTOCOL_FEE

```solidity
function BRIDGE_PROTOCOL_FEE() public view virtual override returns (uint256)
```

Returns the part of the bridge fees sent to protocol.

#### Return Values:

| Type      | Description                                                       |
| :-------- | :---------------------------------------------------------------- |
| `uint256` | The percentage of available liquidity to borrow, expressed in bps |

### FLASHLOAN\_PREMIUM\_TO\_PROTOCOL

```solidity
function FLASHLOAN_PREMIUM_TO_PROTOCOL() public view virtual override returns (uint128)
```

Returns the percent of flashloan premium that is accrued to the treasury.

#### Return Values:

| Type      | Description                                                       |
| :-------- | :---------------------------------------------------------------- |
| `uint128` | The percentage of available liquidity to borrow, expressed in bps |

### MAX_NUMBER_RESERVES

```solidity
function MAX_NUMBER_RESERVES() public view virtual override returns (uint16)
```

Returns the maximum number of reserves supported to be listed in this Pool.

#### Return Values:

| Type     | Description                              |
| :------- | :--------------------------------------- |
| `uint16` | The maximum number of reserves supported |

## Pure Methods

### getRevision

```solidity
function getRevision() internal pure virtual override returns (uint256)
```

Returns the revision number of the contract. Needs to be defined in the inherited class as a constant.

Returns `0x1`.

#### Return Values:

| Type      | Description         |
| :-------- | :------------------ |
| `uint256` | The revision number | 

## ABI
<details>
<summary>Pool ABI</summary>

```
[
    {
        "inputs": [
            {
                "internalType": "contract IPoolAddressesProvider",
                "name": "provider",
                "type": "address"
            }
        ],
        "stateMutability": "nonpayable",
        "type": "constructor"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "reserve",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "backer",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "fee",
                "type": "uint256"
            }
        ],
        "name": "BackUnbacked",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "reserve",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "address",
                "name": "user",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "onBehalfOf",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "enum DataTypes.InterestRateMode",
                "name": "interestRateMode",
                "type": "uint8"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "borrowRate",
                "type": "uint256"
            },
            {
                "indexed": true,
                "internalType": "uint16",
                "name": "referralCode",
                "type": "uint16"
            }
        ],
        "name": "Borrow",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "target",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "address",
                "name": "initiator",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "asset",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "enum DataTypes.InterestRateMode",
                "name": "interestRateMode",
                "type": "uint8"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "premium",
                "type": "uint256"
            },
            {
                "indexed": true,
                "internalType": "uint16",
                "name": "referralCode",
                "type": "uint16"
            }
        ],
        "name": "FlashLoan",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "asset",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "totalDebt",
                "type": "uint256"
            }
        ],
        "name": "IsolationModeTotalDebtUpdated",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "collateralAsset",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "debtAsset",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "user",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "debtToCover",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "liquidatedCollateralAmount",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "address",
                "name": "liquidator",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "bool",
                "name": "receiveAToken",
                "type": "bool"
            }
        ],
        "name": "LiquidationCall",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "reserve",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "address",
                "name": "user",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "onBehalfOf",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            },
            {
                "indexed": true,
                "internalType": "uint16",
                "name": "referralCode",
                "type": "uint16"
            }
        ],
        "name": "MintUnbacked",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "reserve",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "amountMinted",
                "type": "uint256"
            }
        ],
        "name": "MintedToTreasury",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "reserve",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "user",
                "type": "address"
            }
        ],
        "name": "RebalanceStableBorrowRate",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "reserve",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "user",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "repayer",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "bool",
                "name": "useATokens",
                "type": "bool"
            }
        ],
        "name": "Repay",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "reserve",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "liquidityRate",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "stableBorrowRate",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "variableBorrowRate",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "liquidityIndex",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "variableBorrowIndex",
                "type": "uint256"
            }
        ],
        "name": "ReserveDataUpdated",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "reserve",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "user",
                "type": "address"
            }
        ],
        "name": "ReserveUsedAsCollateralDisabled",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "reserve",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "user",
                "type": "address"
            }
        ],
        "name": "ReserveUsedAsCollateralEnabled",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "reserve",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "address",
                "name": "user",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "onBehalfOf",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            },
            {
                "indexed": true,
                "internalType": "uint16",
                "name": "referralCode",
                "type": "uint16"
            }
        ],
        "name": "Supply",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "reserve",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "user",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "enum DataTypes.InterestRateMode",
                "name": "interestRateMode",
                "type": "uint8"
            }
        ],
        "name": "SwapBorrowRateMode",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "user",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "uint8",
                "name": "categoryId",
                "type": "uint8"
            }
        ],
        "name": "UserEModeSet",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "reserve",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "user",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "to",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            }
        ],
        "name": "Withdraw",
        "type": "event"
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
        "inputs": [],
        "name": "BRIDGE_PROTOCOL_FEE",
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
        "name": "FLASHLOAN_PREMIUM_TOTAL",
        "outputs": [
            {
                "internalType": "uint128",
                "name": "",
                "type": "uint128"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "FLASHLOAN_PREMIUM_TO_PROTOCOL",
        "outputs": [
            {
                "internalType": "uint128",
                "name": "",
                "type": "uint128"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "MAX_NUMBER_RESERVES",
        "outputs": [
            {
                "internalType": "uint16",
                "name": "",
                "type": "uint16"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "MAX_STABLE_RATE_BORROW_SIZE_PERCENT",
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
        "name": "POOL_REVISION",
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
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "fee",
                "type": "uint256"
            }
        ],
        "name": "backUnbacked",
        "outputs": [],
        "stateMutability": "nonpayable",
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
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "interestRateMode",
                "type": "uint256"
            },
            {
                "internalType": "uint16",
                "name": "referralCode",
                "type": "uint16"
            },
            {
                "internalType": "address",
                "name": "onBehalfOf",
                "type": "address"
            }
        ],
        "name": "borrow",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "uint8",
                "name": "id",
                "type": "uint8"
            },
            {
                "components": [
                    {
                        "internalType": "uint16",
                        "name": "ltv",
                        "type": "uint16"
                    },
                    {
                        "internalType": "uint16",
                        "name": "liquidationThreshold",
                        "type": "uint16"
                    },
                    {
                        "internalType": "uint16",
                        "name": "liquidationBonus",
                        "type": "uint16"
                    },
                    {
                        "internalType": "address",
                        "name": "priceSource",
                        "type": "address"
                    },
                    {
                        "internalType": "string",
                        "name": "label",
                        "type": "string"
                    }
                ],
                "internalType": "struct DataTypes.EModeCategory",
                "name": "category",
                "type": "tuple"
            }
        ],
        "name": "configureEModeCategory",
        "outputs": [],
        "stateMutability": "nonpayable",
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
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            },
            {
                "internalType": "address",
                "name": "onBehalfOf",
                "type": "address"
            },
            {
                "internalType": "uint16",
                "name": "referralCode",
                "type": "uint16"
            }
        ],
        "name": "deposit",
        "outputs": [],
        "stateMutability": "nonpayable",
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
        "name": "dropReserve",
        "outputs": [],
        "stateMutability": "nonpayable",
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
                "name": "from",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "to",
                "type": "address"
            },
            {
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "balanceFromBefore",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "balanceToBefore",
                "type": "uint256"
            }
        ],
        "name": "finalizeTransfer",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "receiverAddress",
                "type": "address"
            },
            {
                "internalType": "address[]",
                "name": "assets",
                "type": "address[]"
            },
            {
                "internalType": "uint256[]",
                "name": "amounts",
                "type": "uint256[]"
            },
            {
                "internalType": "uint256[]",
                "name": "interestRateModes",
                "type": "uint256[]"
            },
            {
                "internalType": "address",
                "name": "onBehalfOf",
                "type": "address"
            },
            {
                "internalType": "bytes",
                "name": "params",
                "type": "bytes"
            },
            {
                "internalType": "uint16",
                "name": "referralCode",
                "type": "uint16"
            }
        ],
        "name": "flashLoan",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "receiverAddress",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "asset",
                "type": "address"
            },
            {
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            },
            {
                "internalType": "bytes",
                "name": "params",
                "type": "bytes"
            },
            {
                "internalType": "uint16",
                "name": "referralCode",
                "type": "uint16"
            }
        ],
        "name": "flashLoanSimple",
        "outputs": [],
        "stateMutability": "nonpayable",
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
        "name": "getConfiguration",
        "outputs": [
            {
                "components": [
                    {
                        "internalType": "uint256",
                        "name": "data",
                        "type": "uint256"
                    }
                ],
                "internalType": "struct DataTypes.ReserveConfigurationMap",
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
                "internalType": "uint8",
                "name": "id",
                "type": "uint8"
            }
        ],
        "name": "getEModeCategoryData",
        "outputs": [
            {
                "components": [
                    {
                        "internalType": "uint16",
                        "name": "ltv",
                        "type": "uint16"
                    },
                    {
                        "internalType": "uint16",
                        "name": "liquidationThreshold",
                        "type": "uint16"
                    },
                    {
                        "internalType": "uint16",
                        "name": "liquidationBonus",
                        "type": "uint16"
                    },
                    {
                        "internalType": "address",
                        "name": "priceSource",
                        "type": "address"
                    },
                    {
                        "internalType": "string",
                        "name": "label",
                        "type": "string"
                    }
                ],
                "internalType": "struct DataTypes.EModeCategory",
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
                "internalType": "uint16",
                "name": "id",
                "type": "uint16"
            }
        ],
        "name": "getReserveAddressById",
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
                "internalType": "address",
                "name": "asset",
                "type": "address"
            }
        ],
        "name": "getReserveData",
        "outputs": [
            {
                "components": [
                    {
                        "components": [
                            {
                                "internalType": "uint256",
                                "name": "data",
                                "type": "uint256"
                            }
                        ],
                        "internalType": "struct DataTypes.ReserveConfigurationMap",
                        "name": "configuration",
                        "type": "tuple"
                    },
                    {
                        "internalType": "uint128",
                        "name": "liquidityIndex",
                        "type": "uint128"
                    },
                    {
                        "internalType": "uint128",
                        "name": "currentLiquidityRate",
                        "type": "uint128"
                    },
                    {
                        "internalType": "uint128",
                        "name": "variableBorrowIndex",
                        "type": "uint128"
                    },
                    {
                        "internalType": "uint128",
                        "name": "currentVariableBorrowRate",
                        "type": "uint128"
                    },
                    {
                        "internalType": "uint128",
                        "name": "currentStableBorrowRate",
                        "type": "uint128"
                    },
                    {
                        "internalType": "uint40",
                        "name": "lastUpdateTimestamp",
                        "type": "uint40"
                    },
                    {
                        "internalType": "uint16",
                        "name": "id",
                        "type": "uint16"
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
                    }
                ],
                "internalType": "struct DataTypes.ReserveData",
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
                "internalType": "address",
                "name": "asset",
                "type": "address"
            }
        ],
        "name": "getReserveNormalizedIncome",
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
        "name": "getReserveNormalizedVariableDebt",
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
                "internalType": "address",
                "name": "user",
                "type": "address"
            }
        ],
        "name": "getUserAccountData",
        "outputs": [
            {
                "internalType": "uint256",
                "name": "totalCollateralBase",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "totalDebtBase",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "availableBorrowsBase",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "currentLiquidationThreshold",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "ltv",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "healthFactor",
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
                "name": "user",
                "type": "address"
            }
        ],
        "name": "getUserConfiguration",
        "outputs": [
            {
                "components": [
                    {
                        "internalType": "uint256",
                        "name": "data",
                        "type": "uint256"
                    }
                ],
                "internalType": "struct DataTypes.UserConfigurationMap",
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
                "internalType": "address",
                "name": "user",
                "type": "address"
            }
        ],
        "name": "getUserEMode",
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
                "name": "aTokenAddress",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "stableDebtAddress",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "variableDebtAddress",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "interestRateStrategyAddress",
                "type": "address"
            }
        ],
        "name": "initReserve",
        "outputs": [],
        "stateMutability": "nonpayable",
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
        "name": "initialize",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "collateralAsset",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "debtAsset",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "user",
                "type": "address"
            },
            {
                "internalType": "uint256",
                "name": "debtToCover",
                "type": "uint256"
            },
            {
                "internalType": "bool",
                "name": "receiveAToken",
                "type": "bool"
            }
        ],
        "name": "liquidationCall",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address[]",
                "name": "assets",
                "type": "address[]"
            }
        ],
        "name": "mintToTreasury",
        "outputs": [],
        "stateMutability": "nonpayable",
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
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            },
            {
                "internalType": "address",
                "name": "onBehalfOf",
                "type": "address"
            },
            {
                "internalType": "uint16",
                "name": "referralCode",
                "type": "uint16"
            }
        ],
        "name": "mintUnbacked",
        "outputs": [],
        "stateMutability": "nonpayable",
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
        "name": "rebalanceStableBorrowRate",
        "outputs": [],
        "stateMutability": "nonpayable",
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
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "interestRateMode",
                "type": "uint256"
            },
            {
                "internalType": "address",
                "name": "onBehalfOf",
                "type": "address"
            }
        ],
        "name": "repay",
        "outputs": [
            {
                "internalType": "uint256",
                "name": "",
                "type": "uint256"
            }
        ],
        "stateMutability": "nonpayable",
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
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "interestRateMode",
                "type": "uint256"
            }
        ],
        "name": "repayWithATokens",
        "outputs": [
            {
                "internalType": "uint256",
                "name": "",
                "type": "uint256"
            }
        ],
        "stateMutability": "nonpayable",
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
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "interestRateMode",
                "type": "uint256"
            },
            {
                "internalType": "address",
                "name": "onBehalfOf",
                "type": "address"
            },
            {
                "internalType": "uint256",
                "name": "deadline",
                "type": "uint256"
            },
            {
                "internalType": "uint8",
                "name": "permitV",
                "type": "uint8"
            },
            {
                "internalType": "bytes32",
                "name": "permitR",
                "type": "bytes32"
            },
            {
                "internalType": "bytes32",
                "name": "permitS",
                "type": "bytes32"
            }
        ],
        "name": "repayWithPermit",
        "outputs": [
            {
                "internalType": "uint256",
                "name": "",
                "type": "uint256"
            }
        ],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "token",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "to",
                "type": "address"
            },
            {
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            }
        ],
        "name": "rescueTokens",
        "outputs": [],
        "stateMutability": "nonpayable",
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
        "name": "resetIsolationModeTotalDebt",
        "outputs": [],
        "stateMutability": "nonpayable",
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
                "components": [
                    {
                        "internalType": "uint256",
                        "name": "data",
                        "type": "uint256"
                    }
                ],
                "internalType": "struct DataTypes.ReserveConfigurationMap",
                "name": "configuration",
                "type": "tuple"
            }
        ],
        "name": "setConfiguration",
        "outputs": [],
        "stateMutability": "nonpayable",
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
                "name": "rateStrategyAddress",
                "type": "address"
            }
        ],
        "name": "setReserveInterestRateStrategyAddress",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "uint8",
                "name": "categoryId",
                "type": "uint8"
            }
        ],
        "name": "setUserEMode",
        "outputs": [],
        "stateMutability": "nonpayable",
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
                "internalType": "bool",
                "name": "useAsCollateral",
                "type": "bool"
            }
        ],
        "name": "setUserUseReserveAsCollateral",
        "outputs": [],
        "stateMutability": "nonpayable",
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
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            },
            {
                "internalType": "address",
                "name": "onBehalfOf",
                "type": "address"
            },
            {
                "internalType": "uint16",
                "name": "referralCode",
                "type": "uint16"
            }
        ],
        "name": "supply",
        "outputs": [],
        "stateMutability": "nonpayable",
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
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            },
            {
                "internalType": "address",
                "name": "onBehalfOf",
                "type": "address"
            },
            {
                "internalType": "uint16",
                "name": "referralCode",
                "type": "uint16"
            },
            {
                "internalType": "uint256",
                "name": "deadline",
                "type": "uint256"
            },
            {
                "internalType": "uint8",
                "name": "permitV",
                "type": "uint8"
            },
            {
                "internalType": "bytes32",
                "name": "permitR",
                "type": "bytes32"
            },
            {
                "internalType": "bytes32",
                "name": "permitS",
                "type": "bytes32"
            }
        ],
        "name": "supplyWithPermit",
        "outputs": [],
        "stateMutability": "nonpayable",
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
                "internalType": "uint256",
                "name": "interestRateMode",
                "type": "uint256"
            }
        ],
        "name": "swapBorrowRateMode",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "uint256",
                "name": "protocolFee",
                "type": "uint256"
            }
        ],
        "name": "updateBridgeProtocolFee",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "uint128",
                "name": "flashLoanPremiumTotal",
                "type": "uint128"
            },
            {
                "internalType": "uint128",
                "name": "flashLoanPremiumToProtocol",
                "type": "uint128"
            }
        ],
        "name": "updateFlashloanPremiums",
        "outputs": [],
        "stateMutability": "nonpayable",
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
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            },
            {
                "internalType": "address",
                "name": "to",
                "type": "address"
            }
        ],
        "name": "withdraw",
        "outputs": [
            {
                "internalType": "uint256",
                "name": "",
                "type": "uint256"
            }
        ],
        "stateMutability": "nonpayable",
        "type": "function"
    }
]
```
</details>