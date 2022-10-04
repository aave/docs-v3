# BridgeLogic

The BridgeLogic library implements the base logic for all the actions related to bridges. 

Source code available on [GitHub](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/libraries/logic/BridgeLogic.sol).

## Write Methods

### executeMintUnbacked

```solidity
function executeMintUnbacked(
    mapping(address => DataTypes.ReserveData) storage reservesData,
    mapping(uint256 => address) storage reservesList,
    DataTypes.UserConfigurationMap storage userConfig,
    address asset,
    uint256 amount,
    address onBehalfOf,
    uint16 referralCode
) external
```

Mint unbacked aTokens to a user and updates the unbacked for the reserve.

Essentially a supply without transferring the underlying.

Emits the `MintUnbacked` event. Emits the `ReserveUsedAsCollateralEnabled` if asset is set as collateral.

#### Input Parameters:

| Name         | Type                                        | Description                                                                                                                                                     |
| :----------- | :------------------------------------------ | :-------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| reservesData | `mapping(address => DataTypes.ReserveData)` | Mapping of the state of all the reserves                                                                                                                        |
| reservesList | `mapping(uint256 => address)`               | Mapping of the addresses of all the active reserves                                                                                                             |
| userConfig   | `DataTypes.UserConfigurationMap`            | The user configuration mapping that tracks the supplied/borrowed assets                                                                                         |
| asset        | `address`                                   | The address of the underlying asset to mint aTokens                                                                                                             |
| amount       | `uint256`                                   | The amount to mint                                                                                                                                              |
| onBehalfOf   | `address`                                   | The address that will receive the aTokens                                                                                                                       |
| referralCode | `uint26`                                    | Code used to register the integrator originating the operation, for potential rewards. 0 if the action is executed directly by the user, without any middle-man |

### executeBackUnbacked

```solidity
function executeBackUnbacked(
    DataTypes.ReserveData storage reserve,
    address asset,
    uint256 amount,
    uint256 fee,
    uint256 protocolFeeBps
) external
```

Back the current unbacked with `amount` and pay `fee`.

Emits the `BackUnbacked` event.

#### Input Parameters:

| Name           | Type                    | Description                                               |
| :------------- | :---------------------- | :-------------------------------------------------------- |
| reserve        | `DataTypes.ReserveData` | The reserve to back unbacked for                          |
| asset          | `address`               | The address of the underlying asset to repay              |
| amount         | `uint256`               | The amount to back                                        |
| fee            | `uint256`               | The amount paid in fees                                   |
| protocolFeeBps | `uint256`               | The fraction of fees in basis points paid to the protocol |

## Data Types

The data types below are from the [DataTypes library](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/libraries/types/DataTypes.sol). They are used throughout the BridgeLogic library.

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