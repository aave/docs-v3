# BorrowLogic

The BorrowLogic library implements the base logic for all the actions related to borrowing. 

Source code available on [GitHub](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/libraries/logic/BorrowLogic.sol).

## Write Methods

### executeBorrow

```solidity
function executeBorrow(
    mapping(address => DataTypes.ReserveData) storage reservesData,
    mapping(uint256 => address) storage reservesList,
    mapping(uint8 => DataTypes.EModeCategory) storage eModeCategories,
    DataTypes.UserConfigurationMap storage userConfig,
    DataTypes.ExecuteBorrowParams memory params
) public 
```

Implements the borrow feature. Borrowing allows users that provided collateral to draw liquidity from the Aave protocol proportionally to their collateralization power. For isolated positions, it also increases the isolated debt.
 
Emits the `Borrow()` event.

#### Input Parameters:

| Name            | Type                                        | Description                                                             |
| :-------------- | :------------------------------------------ | :---------------------------------------------------------------------- |
| reservesData    | `mapping(address => DataTypes.ReserveData)` | Mapping of the state of all the reserves                                |
| reservesList    | `mapping(uint256 => address)`               | Mapping of the addresses of all the active reserves                     |
| eModeCategories | `mapping(uint8 => DataTypes.EModeCategory)` | Mapping of the configuration of all the efficiency mode categories      |
| userConfig      | `DataTypes.UserConfigurationMap`            | The user configuration mapping that tracks the supplied/borrowed assets |
| params          | `DataTypes.ExecuteBorrowParams`             | The additional parameters needed to execute the borrow function         |

### executeRepay

```solidity
function executeRepay(
    mapping(address => DataTypes.ReserveData) storage reservesData,
    mapping(uint256 => address) storage reservesList,
    DataTypes.UserConfigurationMap storage userConfig,
    DataTypes.ExecuteRepayParams memory params
) external returns (uint256)
```

Implements the repay feature. Repaying transfers the underlying back to the aToken and clears the equivalent amount of debt for the user by burning the corresponding debt token. For isolated positions, it also reduces the isolated debt.

Emits the `Repay()` event.

#### Input Parameters:

| Name         | Type                                        | Description                                                             |
| :----------- | :------------------------------------------ | :---------------------------------------------------------------------- |
| reservesData | `mapping(address => DataTypes.ReserveData)` | Mapping of the state of all the reserves                                |
| reservesList | `mapping(uint256 => address)`               | Mapping of the addresses of all the active reserves                     |
| userConfig   | `DataTypes.UserConfigurationMap`            | The user configuration mapping that tracks the supplied/borrowed assets |
| params       | `DataTypes.ExecuteBorrowParams`             | The additional parameters needed to execute the repay function          |

#### Return Values:

| Type      | Description                    |
| :-------- | :----------------------------- |
| `uint256` | The actual amount being repaid |

### executeRebalanceStableBorrowRate

```solidity
function executeRebalanceStableBorrowRate(
    DataTypes.ReserveData storage reserve,
    address asset,
    address user
) external
```

Implements the rebalance stable borrow rate feature. In case of liquidity crunches on the protocol, stable rate borrows might need to be rebalanced to bring back equilibrium between the borrow and supply APYs.

The rules that define if a position can be rebalanced are implemented in `ValidationLogic.validateRebalanceStableBorrowRate()`. Emits the `RebalanceStableBorrowRate()` event.

#### Input Parameters:

| Name    | Type                    | Description                                        |
| :------ | :---------------------- | :------------------------------------------------- |
| reserve | `DataTypes.ReserveData` | The state of the reserve of the asset being repaid |
| asset   | `address`               | The asset of the position being rebalanced         |
| user    | `address`               | The user being rebalanced                          |

### executeSwapBorrowRateMode

```solidity
function executeSwapBorrowRateMode(
    DataTypes.ReserveData storage reserve,
    DataTypes.UserConfigurationMap storage userConfig,
    address asset,
    DataTypes.InterestRateMode interestRateMode
) external
```

Implements the swap borrow rate feature. Borrowers can swap from variable to stable positions at any time.

Emits the `Swap()` event.

#### Input Parameters:

| Name             | Type                             | Description                                                             |
| :--------------- | :------------------------------- | :---------------------------------------------------------------------- |
| reserve          | `DataTypes.ReserveData`          | The state of the reserve of the asset being repaid                      |
| userConfig       | `DataTypes.UserConfigurationMap` | The user configuration mapping that tracks the supplied/borrowed assets |
| asset            | `address`                        | The asset of the position being swapped                                 |
| interestRateMode | `DataTypes.InterestRateMode`     | The current interest rate mode of the position being swapped            |

## Data Types

The data types below are from the [DataTypes library](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/libraries/types/DataTypes.sol). They are used throughout the BorrowLogic library.

### ReserveData

```solidity
struct ReserveData {
    //stores the reserve configuration
    ReserveConfigurationMap configuration;
    //the liquidity index. Expressed in ray
    uint128 liquidityIndex;
    //the current supply rate. Expressed in ray
    uint128 currentLiquidityRate;
    //variable borrow index. Expressed in ray
    uint128 variableBorrowIndex;
    //the current variable borrow rate. Expressed in ray
    uint128 currentVariableBorrowRate;
    //the current stable borrow rate. Expressed in ray
    uint128 currentStableBorrowRate;
    //timestamp of last update
    uint40 lastUpdateTimestamp;
    //the id of the reserve. Represents the position in the list of the active reserves
    uint16 id;
    //aToken address
    address aTokenAddress;
    //stableDebtToken address
    address stableDebtTokenAddress;
    //variableDebtToken address
    address variableDebtTokenAddress;
    //address of the interest rate strategy
    address interestRateStrategyAddress;
    //the current treasury balance, scaled
    uint128 accruedToTreasury;
    //the outstanding unbacked aTokens minted through the bridging feature
    uint128 unbacked;
    //the outstanding debt borrowed against this asset in isolation mode
    uint128 isolationModeTotalDebt;
}
```

### UserConfigurationMap

```solidity
struct UserConfigurationMap {
    uint256 data;
}
```

Bitmap of the users collaterals and borrows. It is divided in pairs of bits, one pair per asset.

The first bit indicates if an asset is used as collateral by the user, the second whether an asset is borrowed by the user.

### EModeCategory

```solidity
struct EModeCategory {
    // each eMode category has a custom ltv and liquidation threshold
    uint16 ltv;
    uint16 liquidationThreshold;
    uint16 liquidationBonus;
    // each eMode category may or may not have a custom oracle to override the individual assets price oracles
    address priceSource;
    string label;
}
```

### InterestRateMode

```solidity
enum InterestRateMode {
    NONE,
    STABLE,
    VARIABLE
}
```

### ExecuteBorrowParams

```solidity
struct ExecuteBorrowParams {
    address asset;
    address user;
    address onBehalfOf;
    uint256 amount;
    InterestRateMode interestRateMode;
    uint16 referralCode;
    bool releaseUnderlying;
    uint256 maxStableRateBorrowSizePercent;
    uint256 reservesCount;
    address oracle;
    uint8 userEModeCategory;
    address priceOracleSentinel;
}
```

### ExecuteRepayParams

```solidity
struct ExecuteRepayParams {
    address asset;
    uint256 amount;
    InterestRateMode interestRateMode;
    address onBehalfOf;
    bool useATokens;
}
```