# EModeLogic

The EModeLogic library implements the base logic for all the actions related to the eMode.

Source code available on [GitHub](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/libraries/logic/EModeLogic.sol).

## Write Methods

### executeSetUserEMode

```solidity
function executeSetUserEMode(
    mapping(address => DataTypes.ReserveData) storage reservesData,
    mapping(uint256 => address) storage reservesList,
    mapping(uint8 => DataTypes.EModeCategory) storage eModeCategories,
    mapping(address => uint8) storage usersEModeCategory,
    DataTypes.UserConfigurationMap storage userConfig,
    DataTypes.ExecuteSetUserEModeParams memory params
) external
```

Updates the user efficiency mode category.

Will revert if user is borrowing non-compatible asset or change will drop `HF < HEALTH_FACTOR_LIQUIDATION_THRESHOLD`.

Emits the `UserEModeSet` event.

#### Input Parameters:

| Name               | Type                                        | Description                                                             |
| :----------------- | :------------------------------------------ | :---------------------------------------------------------------------- |
| reservesData       | `mapping(address => DataTypes.ReserveData)` | Mapping of the state of all the reserves                                |
| reservesList       | `mapping(uint256 => address)`               | Mapping of the addresses of all the active reserves                     |
| eModeCategories    | `mapping(uint8 => DataTypes.EModeCategory)` | Mapping of the configuration of all the efficiency mode categories      |
| usersEModeCategory | `mapping(address => uint8)`                 | Mapping of the state of all users efficiency mode category              |
| userConfig         | `DataTypes.UserConfigurationMap`            | The user configuration mapping that tracks the supplied/borrowed assets |
| params             | `DataTypes.ExecuteSetUserEModeParams`       | The additional parameters needed to execute the setUserEMode function   |

## Data Types

The data types below are from the [DataTypes library](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/libraries/types/DataTypes.sol). They are used throughout the EModeLogic library.

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

### ExecuteSetUserEModeParams

```solidity
struct ExecuteSetUserEModeParams {
    uint256 reservesCount;
    address oracle;
    uint8 categoryId;
}
```