# PoolLogic

The PoolLogic library implements the logic for Pool specific functions.

Source code available on [GitHub](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/libraries/logic/PoolLogic.sol).

## Write Methods

### executeInitReserve

```solidity
function executeInitReserve(
    mapping(address => DataTypes.ReserveData) storage reservesData,
    mapping(uint256 => address) storage reservesList,
    DataTypes.InitReserveParams memory params
) external returns (bool) 
```

Initialize an asset reserve and add the reserve to the list of reserves.

#### Input Parameters:

| Name         | Type                                        | Description                                         |
| :----------- | :------------------------------------------ | :-------------------------------------------------- |
| reservesData | `mapping(address => DataTypes.ReserveData)` | Mapping of the state of all the reserves            |
| reservesList | `mapping(uint256 => address)`               | Mapping of the addresses of all the active reserves |
| params       | `DataTypes.InitReserveParams`               | Additional parameters needed for initiation         |

#### Return Values:

| Type   | Description                                                    |
| :----- | :------------------------------------------------------------- |
| `bool` | `true` if appended, `false` if inserted at existing empty spot |

### executeRescueTokens

```solidity
function executeRescueTokens(
    address token,
    address to,
    uint256 amount
) external
```

Rescue and transfer tokens locked in this contract.

#### Input Parameters:

| Name   | Type      | Description                     |
| :----- | :-------- | :------------------------------ |
| token  | `address` | The address of the token        |
| to     | `address` | The address of the recipient    |
| amount | `uint256` | The amount of token to transfer |

### executeMintToTreasury

```solidity
function executeMintToTreasury(
    mapping(address => DataTypes.ReserveData) storage reservesData,
    address[] calldata assets
) external
```

Mints the assets accrued through the reserve factor to the treasury in the form of aTokens.

#### Input Parameters:

| Name         | Type                                        | Description                                                     |
| :----------- | :------------------------------------------ | :-------------------------------------------------------------- |
| reservesData | `mapping(address => DataTypes.ReserveData)` | Mapping of the state of all the reserves                        |
| assets       | `address[]`                                 | The list of reserves for which the minting needs to be executed |

### executeResetIsolationModeTotalDebt

```solidity
function executeResetIsolationModeTotalDebt(
    mapping(address => DataTypes.ReserveData) storage reservesData,
    address asset
) external 
```

Resets the isolation mode total debt of the given asset to zero.

It requires the given asset has zero debt ceiling.

#### Input Parameters:

| Name         | Type                                        | Description                                                             |
| :----------- | :------------------------------------------ | :---------------------------------------------------------------------- |
| reservesData | `mapping(address => DataTypes.ReserveData)` | Mapping of the state of all the reserves                                |
| asset        | `address`                                   | The address of the underlying asset to reset the isolationModeTotalDebt |

### executeDropReserve

```solidity
function executeDropReserve(
    mapping(address => DataTypes.ReserveData) storage reservesData,
    mapping(uint256 => address) storage reservesList,
    address asset
) external
```

Drop a reserve.

#### Input Parameters:

| Name         | Type                                        | Description                                         |
| :----------- | :------------------------------------------ | :-------------------------------------------------- |
| reservesData | `mapping(address => DataTypes.ReserveData)` | Mapping of the state of all the reserves            |
| reservesList | `mapping(uint256 => address)`               | Mapping of the addresses of all the active reserves |
| asset        | `address`                                   | The address of the underlying asset of the reserve  |

## View Methods

### executeGetUserAccountData

```solidity
function executeGetUserAccountData(
    mapping(address => DataTypes.ReserveData) storage reservesData,
    mapping(uint256 => address) storage reservesList,
    mapping(uint8 => DataTypes.EModeCategory) storage eModeCategories,
    DataTypes.CalculateUserAccountDataParams memory params
  )
    external
    view
    returns (
      uint256 totalCollateralBase,
      uint256 totalDebtBase,
      uint256 availableBorrowsBase,
      uint256 currentLiquidationThreshold,
      uint256 ltv,
      uint256 healthFactor
)
```

Returns the user account data across all the reserves.

#### Input Parameters:

| Name            | Type                                        | Description                                                        |
| :-------------- | :------------------------------------------ | :----------------------------------------------------------------- |
| reservesData    | `mapping(address => DataTypes.ReserveData)` | Mapping of the state of all the reserves                           |
| reservesList    | `mapping(uint256 => address)`               | Mapping of the addresses of all the active reserves                |
| eModeCategories | `mapping(uint8 => DataTypes.EModeCategory)` | Mapping of the configuration of all the efficiency mode categories |
| params          | `DataTypes.CalculateUserAccountDataParams`  | Additional params needed for the calculation                       |

#### Return Values:

| Name                        | Type      | Description                                                                      |
| :-------------------------- | :-------- | :------------------------------------------------------------------------------- |
| totalCollateralBase         | `uint256` | The total collateral of the user in the base currency used by the price feed     |
| totalDebtBase               | `uint256` | The total debt of the user in the base currency used by the price feed           |
| availableBorrowsBase        | `uint256` | The borrowing power left of the user in the base currency used by the price feed |
| currentLiquidationThreshold | `uint256` | The liquidation threshold of the user                                            |
| ltv                         | `uint256` | The loan to value of The user                                                    |
| healthFactor                | `uint256` | The current health factor of the user                                            |


## Data Types

The data types below are from the [DataTypes library](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/libraries/types/DataTypes.sol). They are used throughout the PoolLogic library.

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

### CalculateUserAccountDataParams

```solidity
struct CalculateUserAccountDataParams {
    UserConfigurationMap userConfig;
    uint256 reservesCount;
    address user;
    address oracle;
    uint8 userEModeCategory;
}
```

### InitReserveParams

```solidity
struct InitReserveParams {
    address asset;
    address aTokenAddress;
    address stableDebtAddress;
    address variableDebtAddress;
    address interestRateStrategyAddress;
    uint16 reservesCount;
    uint16 maxNumberReserves;
}
```