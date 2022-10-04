# FlashLoanLogic

The FlashLoanLogic library implements the logic for the flash loans.

Source code available on [GitHub](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/libraries/logic/FlashLoanLogic.sol).

## Write Methods

### executeFlashLoan

```solidity
function executeFlashLoan(
    mapping(address => DataTypes.ReserveData) storage reservesData,
    mapping(uint256 => address) storage reservesList,
    mapping(uint8 => DataTypes.EModeCategory) storage eModeCategories,
    DataTypes.UserConfigurationMap storage userConfig,
    DataTypes.FlashloanParams memory params
) external
```

Implements the flashloan feature that allow users to access liquidity of the pool for one transaction as long as the amount taken plus fee is returned or debt is opened.

For authorized flashborrowers the fee is waived. 

At the end of the transaction the pool will pull amount borrowed + fee from the receiver, if the receiver have not approved the pool the transaction will revert.

Emits the `FlashLoan()` event.

#### Input Parameters:

| Name            | Type                                        | Description                                                             |
| :-------------- | :------------------------------------------ | :---------------------------------------------------------------------- |
| reservesData    | `mapping(address => DataTypes.ReserveData)` | Mapping of the state of all the reserves                                |
| reservesList    | `mapping(uint256 => address)`               | Mapping of the addresses of all the active reserves                     |
| eModeCategories | `mapping(uint8 => DataTypes.EModeCategory)` | Mapping of the configuration of all the efficiency mode categories      |
| userConfig      | `DataTypes.UserConfigurationMap`            | The user configuration mapping that tracks the supplied/borrowed assets |
| params          | `DataTypes.FlashloanParams`                 | The additional parameters needed to execute the flashloan function      |

### executeFlashLoan

```solidity
function executeFlashLoanSimple(
    DataTypes.ReserveData storage reserve,
    DataTypes.FlashloanSimpleParams memory params
) external 
```

Implements the simple flashloan feature that allow users to access liquidity of ONE reserve for one transaction as long as the amount taken plus fee is returned.

Does not waive fee for approved flashborrowers nor allow taking on debt instead of repaying to save gas.

At the end of the transaction the pool will pull amount borrowed + fee from the receiver, if the receiver have not approved the pool the transaction will revert. Emits the `FlashLoan()` event.

The usual action flow (cache -> updateState -> validation -> changeState -> updateRates) is altered to (validation -> user payload -> cache -> updateState -> changeState -> updateRates) for flashloans. This is done to protect against reentrance and rate manipulation within the user specified payload.

#### Input Parameters:

| Name    | Type                              | Description                                                               |
| :------ | :-------------------------------- | :------------------------------------------------------------------------ |
| reserve | `DataTypes.ReserveData`           | The state of the flashloaned reserve                                      |
| params  | `DataTypes.FlashloanSimpleParams` | The additional parameters needed to execute the simple flashloan function |

## Data Types

The data types below are from the [DataTypes library](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/libraries/types/DataTypes.sol). They are used throughout the FlashLoanLogic library.

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

### FlashloanParams

```solidity
struct FlashloanParams {
    address receiverAddress;
    address[] assets;
    uint256[] amounts;
    uint256[] interestRateModes;
    address onBehalfOf;
    bytes params;
    uint16 referralCode;
    uint256 flashLoanPremiumToProtocol;
    uint256 flashLoanPremiumTotal;
    uint256 maxStableRateBorrowSizePercent;
    uint256 reservesCount;
    address addressesProvider;
    uint8 userEModeCategory;
    bool isAuthorizedFlashBorrower;
}
```

### FlashloanSimpleParams

```solidity
struct FlashloanSimpleParams {
    address receiverAddress;
    address asset;
    uint256 amount;
    bytes params;
    uint16 referralCode;
    uint256 flashLoanPremiumToProtocol;
    uint256 flashLoanPremiumTotal;
}