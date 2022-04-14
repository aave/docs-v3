# PoolConfigurator

## Methods

### Risk or Pool Admins

### setSiloedBorrowing

`function setSiloedBorrowing(address asset, bool newSiloed) external`

Call Params

| Name      | Type      | Description                                      |
| --------- | --------- | ------------------------------------------------ |
| asset     | `address` | address of reserve's underlying asset.           |
| newSiloed | `bool`    | Enable/Disable SIloed Borrowing for the reserve. |

* enableBorrowingOnReserve
* disableBorrowingOnReserve
* configureReserveAsCollateral
* enableReserveStableRate
* disableReserveStableRate
* freezeReserve
* unfreezeReserve
* setBorrowableInIsolation
* setReserveFactor
* setDebtCeiling
* setSiloedBorrowing
*   setBorrowCap

    `setBorrowCap (asset, newBorrowCap)`

    Allows `RISK_ADMIN` and `POOL_ADMIN` to add/update cap on the total borrow that can be borrowed from the reserve. Once the borrow cap is reached, no more borrow (variable or stable) for the given reserve asset can be initiated.

    | Param Name   | Type    | Description                                                      |
    | ------------ | ------- | ---------------------------------------------------------------- |
    | asset        | address | Address of the underlying asset.                                 |
    | newBorrowCap | uint256 | <p>Borrow cap in whole tokens.<br>borrowCap == 0 => no cap |</p> |


*   setSupplyCap

    `setSupplyCap (asset, newSupplyCap)`

    Allows `RISK_ADMIN` and `POOL_ADMIN` to add/update liquidity supply cap on the reserve. Once the supply cap is reached, no more liquidity for the given reserve asset can be supplied to the pool.

    | Param Name   | Type    | Description                                                      |
    | ------------ | ------- | ---------------------------------------------------------------- |
    | asset        | address | Address of the underlying asset.                                 |
    | newSupplyCap | uint256 | <p>Supply cap in whole tokens.<br>supplyCap == 0 => no cap |</p> |


* setLiquidationProtocolFee
*   setEModeCategory

    `setEModeCategory (categoryId, ltv, liquidationThreshold, liquidationBonus, oracle, label)`

    Allows `RISK_ADMIN` and `POOL_ADMINS` to configure existing or add new `eModeCategory`

    | Param                | Type    | Description                                                                                |
    | -------------------- | ------- | ------------------------------------------------------------------------------------------ |
    | categoryId           | uint8   | <p>Category id ≠ 0<br>NOTE: category 0 is reserved for default category i.e. non-eMode</p> |
    | ltv                  | uint16  | <p>Loan to value for given eMode categoryId.<br>Must be ≤ liquidationThreshold</p>         |
    | liquidationThreshold | uint16  | Liquidation threshold for given eMode categoryId.                                          |
    | liquidationBonus     | uint16  | Liquidation bonus for given eMode categoryId                                               |
    | oracle               | address | Address of custom price oracle for category                                                |
    | label                | string  | Custom label for the category                                                              |

    &#x20;
*   setAssetEModeCategory

    `setAssetEModeCategory(address asset, uint8 categoryId)`

    Allows `RISK_ADMIN` and `POOL_ADMINS` to configure `eModeCategory` of an asset.

    | Param Name | Type    | Description                                                                                   |
    | ---------- | ------- | --------------------------------------------------------------------------------------------- |
    | asset      | address | Address of the reserve asset being configured                                                 |
    | categoryId | uint8   | <p>≠ 0 one of the already defined eModeCategory<br>= 0 for default aka non-eMode category</p> |
* setUnbackedMintCap
* setReserveInterestRateStrategyAddress

### Asset Listing or Pool Admins

* initReserves

### Only Pool Admin

* dropReserve
* updateAToken
* updateStableDebtToken
* updateVariableDebtToken
* activateReserve
* deactivateReserve
* updateBridgeProtocolFee
* updateFlashloanPremiumTotal
* updateFlashloanPremiumToProtocol
