# LiquidationLogic

The LiquidationLogic library iimplements actions involving management of collateral in the protocol, the main one being the liquidations.

Source code available on [GitHub](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/libraries/logic/LiquidationLogic.sol).

## Write Methods

### executeLiquidationCall

```solidity
function executeLiquidationCall(
    mapping(address => DataTypes.ReserveData) storage reservesData,
    mapping(uint256 => address) storage reservesList,
    mapping(address => DataTypes.UserConfigurationMap) storage usersConfig,
    mapping(uint8 => DataTypes.EModeCategory) storage eModeCategories,
    DataTypes.ExecuteLiquidationCallParams memory params
) external
```

Function to liquidate a position if its Health Factor drops below 1. The caller (liquidator) covers `debtToCover` amount of debt of the user getting liquidated, and receives proportional amount of the `collateralAsset` plus a bonus to cover market risk.

Emits the `LiquidationCall()` event.

#### Input Parameters:

| Name            | Type                                        | Description                                                             |
| :-------------- | :------------------------------------------ | :---------------------------------------------------------------------- |
| reservesData    | `mapping(address => DataTypes.ReserveData)` | Mapping of the state of all the reserves                                |
| reservesList    | `mapping(uint256 => address)`               | Mapping of the addresses of all the active reserves                     |
| usersConfig     | `DataTypes.UserConfigurationMap`            | The users configuration mapping that track the supplied/borrowed assets |
| eModeCategories | `mapping(uint8 => DataTypes.EModeCategory)` | Mapping of the configuration of all the efficiency mode categories      |
| params          | `DataTypes.ExecuteLiquidationCallParams`    | The additional parameters needed to execute the liquidation function    |

## Data Types

The data types below are from the [DataTypes library](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/libraries/types/DataTypes.sol). They are used throughout the LiquidationLogic library.

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

### ExecuteLiquidationCallParams

```solidity
struct ExecuteLiquidationCallParams {
    uint256 reservesCount;
    uint256 debtToCover;
    address collateralAsset;
    address debtAsset;
    address user;
    bool receiveAToken;
    address priceOracle;
    uint8 userEModeCategory;
    address priceOracleSentinel;
}
```