# SupplyLogic

The SupplyLogic library implements the base logic for supply/withdraw.

Source code available on [GitHub](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/libraries/logic/SupplyLogic.sol).

## Write Methods

### executeSupply

```solidity
function executeSupply(
    mapping(address => DataTypes.ReserveData) storage reservesData,
    mapping(uint256 => address) storage reservesList,
    DataTypes.UserConfigurationMap storage userConfig,
    DataTypes.ExecuteSupplyParams memory params
) external
```

Implements the supply feature. Through `supply()`, users supply assets to the Aave protocol.

Emits the `Supply()` event.

In the first supply action, `ReserveUsedAsCollateralEnabled()` is emitted, if the asset can be enabled as collateral.

#### Input Parameters:

| Name         | Type                                        | Description                                                             |
| :----------- | :------------------------------------------ | :---------------------------------------------------------------------- |
| reservesData | `mapping(address => DataTypes.ReserveData)` | Mapping of the state of all the reserves                                |
| reservesList | `mapping(uint256 => address)`               | Mapping of the addresses of all the active reserves                     |
| userConfig   | `DataTypes.UserConfigurationMap`            | The user configuration mapping that tracks the supplied/borrowed assets |
| params       | `DataTypes.ExecuteSupplyParams`             | The additional parameters needed to execute the supply function         |

### executeWithdraw

```solidity
function executeWithdraw(
    mapping(address => DataTypes.ReserveData) storage reservesData,
    mapping(uint256 => address) storage reservesList,
    mapping(uint8 => DataTypes.EModeCategory) storage eModeCategories,
    DataTypes.UserConfigurationMap storage userConfig,
    DataTypes.ExecuteWithdrawParams memory params
) external returns (uint256) 
```

Implements the withdraw feature. Through `withdraw()`, users redeem their aTokens for the underlying asset previously supplied in the Aave protocol.

Emits the `Withdraw()` event.

If the user withdraws everything, `ReserveUsedAsCollateralDisabled()` is emitted.

#### Input Parameters:

| Name            | Type                                        | Description                                                             |
| :-------------- | :------------------------------------------ | :---------------------------------------------------------------------- |
| reservesData    | `mapping(address => DataTypes.ReserveData)` | Mapping of the state of all the reserves                                |
| reservesList    | `mapping(uint256 => address)`               | Mapping of the addresses of all the active reserves                     |
| eModeCategories | `mapping(uint8 => DataTypes.EModeCategory)` | Mapping of the configuration of all the efficiency mode categories      |
| userConfig      | `DataTypes.UserConfigurationMap`            | The user configuration mapping that tracks the supplied/borrowed assets |
| params          | `DataTypes.ExecuteWithdrawParams`           | The additional parameters needed to execute the withdraw function       |

#### Return Values:

| Type      | Description                 |
| :-------- | :-------------------------- |
| `uint256` | The actual amount withdrawn |

### executeFinalizeTransfer

```solidity
function executeFinalizeTransfer(
    mapping(address => DataTypes.ReserveData) storage reservesData,
    mapping(uint256 => address) storage reservesList,
    mapping(uint8 => DataTypes.EModeCategory) storage eModeCategories,
    mapping(address => DataTypes.UserConfigurationMap) storage usersConfig,
    DataTypes.FinalizeTransferParams memory params
) external
```

Validates a transfer of aTokens. The sender is subjected to health factor validation to avoid collateralization constraints violation.

Emits the `ReserveUsedAsCollateralEnabled()` event for the `to` account, if the asset is being activated as collateral.

In case the `from` user transfers everything, `ReserveUsedAsCollateralDisabled()` is emitted for `from`.

#### Input Parameters:

| Name            | Type                                        | Description                                                               |
| :-------------- | :------------------------------------------ | :------------------------------------------------------------------------ |
| reservesData    | `mapping(address => DataTypes.ReserveData)` | Mapping of the state of all the reserves                                  |
| reservesList    | `mapping(uint256 => address)`               | Mapping of the addresses of all the active reserves                       |
| eModeCategories | `mapping(uint8 => DataTypes.EModeCategory)` | Mapping of the configuration of all the efficiency mode categories        |
| usersConfig     | `DataTypes.UserConfigurationMap`            | The users configuration mapping that track the supplied/borrowed assets   |
| params          | `DataTypes.FinalizeTransferParams`          | The additional parameters needed to execute the finalizeTransfer function |

### executeUseReserveAsCollateral

```solidity
function executeUseReserveAsCollateral(
    mapping(address => DataTypes.ReserveData) storage reservesData,
    mapping(uint256 => address) storage reservesList,
    mapping(uint8 => DataTypes.EModeCategory) storage eModeCategories,
    DataTypes.UserConfigurationMap storage userConfig,
    address asset,
    bool useAsCollateral,
    uint256 reservesCount,
    address priceOracle,
    uint8 userEModeCategory
) external
```

Executes the 'set as collateral' feature. A user can choose to activate or deactivate an asset as collateral at any point in time. Deactivating an asset as collateral is subjected to the usual health factor checks to ensure collateralization.

Emits the `ReserveUsedAsCollateralEnabled()` event if the asset can be activated as collateral.

In case the asset is being deactivated as collateral, `ReserveUsedAsCollateralDisabled()` is emitted.

#### Input Parameters:

| Name              | Type                                        | Description                                                                |
| :---------------- | :------------------------------------------ | :------------------------------------------------------------------------- |
| reservesData      | `mapping(address => DataTypes.ReserveData)` | Mapping of the state of all the reserves                                   |
| reservesList      | `mapping(uint256 => address)`               | Mapping of the addresses of all the active reserves                        |
| eModeCategories   | `mapping(uint8 => DataTypes.EModeCategory)` | Mapping of the configuration of all the efficiency mode categories         |
| usersConfig       | `DataTypes.UserConfigurationMap`            | The users configuration mapping that track the supplied/borrowed assets    |
| asset             | `address`                                   | The address of the asset being configured as collateral                    |
| useAsCollateral   | `bool`                                      | `true` if the user wants to set the asset as collateral, `false` otherwise |
| reservesCount     | `uint256`                                   | The number of initialized reserves                                         |
| priceOracle       | `address`                                   | The address of the price oracle                                            |
| userEModeCategory | `uint8`                                     | The eMode category chosen by the user                                      |

## Data Types

The data types below are from the [DataTypes library](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/libraries/types/DataTypes.sol). They are used throughout the SupplyLogic library.

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

### ExecuteSupplyParams

``` solidity
struct ExecuteSupplyParams {
    address asset;
    uint256 amount;
    address onBehalfOf;
    uint16 referralCode;
}
```

### ExecuteWithdrawParams

```solidity
struct ExecuteWithdrawParams {
    address asset;
    uint256 amount;
    address to;
    uint256 reservesCount;
    address oracle;
    uint8 userEModeCategory;
}
```

### FinalizeTransferParams

```solidity
struct FinalizeTransferParams {
    address asset;
    address from;
    address to;
    uint256 amount;
    uint256 balanceFromBefore;
    uint256 balanceToBefore;
    uint256 reservesCount;
    address oracle;
    uint8 fromEModeCategory;
}