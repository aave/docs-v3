# PoolConfigurator

This contract implements the configuration methods for the Aave protocol. The 'write methods' below are grouped by permissioned system roles that are managed by ACLManager.

The source code can be found [here](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/pool/PoolConfigurator.sol).

## Methods

## Only Asset Listing Or Pool Admins Methods

### initReserves

```solidity
function initReserves(ConfiguratorInputTypes.InitReserveInput[] calldata input) external override onlyAssetListingOrPoolAdmins
```

Initializes multiple reserves using the array of initialization parameters as input.

#### Input Parameters:

| Name                        | Type      | Description                                                         |
| :-------------------------- | :-------- | :------------------------------------------------------------------ |
| aTokenImpl                  | `address` | The address of the aToken contract implementation                   |
| stableDebtTokenImpl         | `address` | The address of the stable debt token contract                       |
| variableDebtTokenImpl       | `address` | The address of the variable debt token contract                     |
| underlyingAssetDecimals     | `uint8`   | The decimals of the reserve underlying asset                        |
| interestRateStrategyAddress | `address` | The address of the interest rate strategy contract for this reserve |
| underlyingAsset             | `address` | The address of the underlying asset                                 |
| treasury                    | `address` | The address of the treasury                                         |
| incentivesController        | `address` | The address of the incentives controller for this aToken            |
| aTokenName                  | `string`  | The name of the aToken                                              |
| aTokenSymbol                | `string`  | The symbol of the aToken                                            |
| variableDebtTokenName       | `string`  | The name of the variable debt token                                 |
| variableDebtTokenSymbol     | `string`  | The symbol of the variable debt token                               |
| stableDebtTokenName         | `string`  | The name of the stable debt token                                   |
| stableDebtTokenSymbol       | `string`  | The symbol of the stable debt token                                 |
| params                      | `bytes`   | A set of encoded parameters for additional initialization           |

## Only Emergency Admin Methods

### setPoolPause

```solidity
function setPoolPause(bool paused) external override onlyEmergencyAdmin 
```

Pauses or unpauses all the protocol reserves. In the paused state all the protocol interactions are suspended.

#### Input Parameters:

| Name   | Type   | Description                                              |
| :----- | :----- | :------------------------------------------------------- |
| paused | `bool` | True if the protocol needs to be paused, otherwise False |

## Only Emergency Or Pool Admin Methods

### setReservePause

```solidity
function setReservePause(address asset, bool paused) public override onlyEmergencyOrPoolAdmin 
```

Pauses a reserve. A paused reserve does not allow any interaction (supply, borrow, repay, swap interest rate, liquidate, atoken transfers).

#### Input Parameters:

| Name   | Type      | Description                                                 |
| :----- | :-------- | :---------------------------------------------------------- |
| asset  | `address` | The address of the underlying asset of the reserve          |
| paused | `bool`    | True if pausing the reserve, False if unpausing the reserve |

## Only Pool Admin Methods

### dropReserve

```solidity
function dropReserve(address asset) external override onlyPoolAdmin
```

Drops a reserve entirely.

#### Input Parameters:

| Name   | Type      | Description                        |
| :----- | :-------- | :--------------------------------- |
| asset  | `address` | The address of the reserve to drop |

### updateAToken

```solidity
function updateAToken(ConfiguratorInputTypes.UpdateATokenInput calldata input) external override onlyPoolAdmin
```

Updates the aToken implementation for the reserve. Takes the aToken update parameters as input.

#### Input Parameters:

| Name                 | Type      | Description                                               |
| :------------------- | :-------- | :-------------------------------------------------------- |
| asset                | `address` | The address of the underlying asset of the reserve        |
| treasury             | `address` | The address of the treasury                               |
| incentivesController | `address` | The address of the incentives controller for this aToken  |
| name                 | `string`  | The name of the aToken                                    |
| symbol               | `string`  | The symbol of the aToken                                  |
| implementation       | `address` | The new aToken implementation                             |
| params               | `bytes`   | A set of encoded parameters for additional initialization |

### updateStableDebtToken

```solidity
function updateStableDebtToken(ConfiguratorInputTypes.UpdateDebtTokenInput calldata input) external override onlyPoolAdmin
```
Updates the stableDebtToken implementation for the reserve. Takes the stableDebtToken update parameters as input.

#### Input Parameters:

| Name                 | Type      | Description                                                       |
| :------------------- | :-------- | :---------------------------------------------------------------- |
| asset                | `address` | The address of the underlying asset of the reserve                |
| incentivesController | `address` | The address of the incentives controller for this stableDebtToken |
| name                 | `string`  | The name of the stableDebtToken                                   |
| symbol               | `string`  | The symbol of the stableDebtToken                                 |
| implementation       | `address` | The new stableDebtToken implementation                            |
| params               | `bytes`   | A set of encoded parameters for additional initialization         |

### updateVariableDebtToken

```solidity
function updateVariableDebtToken(ConfiguratorInputTypes.UpdateDebtTokenInput calldata input) external override onlyPoolAdmin
```

Updates the updateVariableDebtToken implementation for the reserve. Takes the stableDebtToken update parameters as input.

#### Input Parameters:

| Name                 | Type      | Description                                                         |
| :------------------- | :-------- | :------------------------------------------------------------------ |
| asset                | `address` | The address of the underlying asset of the reserve                  |
| incentivesController | `address` | The address of the incentives controller for this variableDebtToken |
| name                 | `string`  | The name of the variableDebtToken                                   |
| symbol               | `string`  | The symbol of the variableDebtToken                                 |
| implementation       | `address` | The new variableDebtToken implementation                            |
| params               | `bytes`   | A set of encoded parameters for additional initialization           |


### setReserveActive

```solidity
function setReserveActive(address asset, bool active) external override onlyPoolAdmin
```

Activate or deactivate a reserve.

#### Input Parameters:

| Name   | Type      | Description                                             |
| :----- | :-------- | :------------------------------------------------------ |
| asset  | `address` | The address of the underlying asset of the reserve      |
| active | `bool`    | True if the reserve needs to be active, false otherwise |

### updateBridgeProtocolFee

```solidity
function updateBridgeProtocolFee(uint256 newBridgeProtocolFee) external override onlyPoolAdmin 
```

Updates the bridge fee collected by the protocol reserves.

#### Input Parameters:

| Name                 | Type      | Description                                                         |
| :------------------- | :-------- | :------------------------------------------------------------------ |
| newBridgeProtocolFee | `uint256` | The part of the fee sent to the protocol treasury, expressed in bps |

### updateFlashloanPremiumTotal

```solidity
function updateFlashloanPremiumTotal(uint128 newFlashloanPremiumTotal) external override onlyPoolAdmin
```

Updates the total flash loan premium. The premium is calculated on the total amount borrowed, and is expressed in bps.

The total flash loan premium consists of two parts:
- A part is sent to aToken holders as extra balance, and
- A part is collected by the protocol reserves.

#### Input Parameters:

| Name                     | Type      | Description                 |
| :----------------------- | :-------- | :-------------------------- |
| newFlashloanPremiumTotal | `uint128` | The total flashloan premium |

### updateFlashloanPremiumToProtocol

```solidity
function updateFlashloanPremiumToProtocol(uint128 newFlashloanPremiumToProtocol) external override onlyPoolAdmin
```

Updates the flash loan premium collected by protocol reserves. The premium to protocol is calculated on the total flashloan premium, and is expressed in bps.

#### Input Parameters:

| Name                          | Type      | Description                                                     |
| :---------------------------- | :-------- | :-------------------------------------------------------------- |
| newFlashloanPremiumToProtocol | `uint128` | The part of the flashloan premium sent to the protocol treasury |

## Only Risk Or Pool Admins Methods

### setReserveBorrowing

```solidity
function setReserveBorrowing(address asset, bool enabled) external override onlyRiskOrPoolAdmins 
```

Configures borrowing on a reserve. It can only be disabled (set to false) if stable borrowing is disabled.

#### Input Parameters:

| Name    | Type      | Description                                            |
| :------ | :-------- | :----------------------------------------------------- |
| asset   | `address` | The address of the underlying asset of the reserve     |
| enabled | `bool`    | True if borrowing needs to be enabled, false otherwise |

### configureReserveAsCollateral

```solidity
function configureReserveAsCollateral(
    address asset,
    uint256 ltv,
    uint256 liquidationThreshold,
    uint256 liquidationBonus
) external override onlyRiskOrPoolAdmins 
```

Configures the reserve collateralization parameters. All the values are expressed in bps. A value of 10000, results in 100.00%. The `liquidationBonus` is always above 100%. A value of 105% means the liquidator will receive a 5% bonus

#### Input Parameters:

| Name                 | Type      | Description                                                                                        |
| :------------------- | :-------- | :------------------------------------------------------------------------------------------------- |
| asset                | `address` | The address of the underlying asset of the reserve                                                 |
| ltv                  | `uint256` | The loan to value of the asset when used as collateral                                             |
| liquidationThreshold | `uint256` | The threshold at which loans using this asset as collateral will be considered undercollateralized |
| liquidationBonus     | `uint256` | The bonus liquidators receive to liquidate this asset                                              |

### setReserveStableRateBorrowing

```solidity
function setReserveStableRateBorrowing(address asset, bool enabled) external override onlyRiskOrPoolAdmins
```

Enable or disable stable rate borrowing on a reserve. Can only be enabled (set to true) if borrowing is enabled.

#### Input Parameters:

| Name    | Type      | Description                                                        |
| :------ | :-------- | :----------------------------------------------------------------- |
| asset   | `address` | The address of the underlying asset of the reserve                 |
| enabled | `bool`    | True if stable rate borrowing needs to be enabled, false otherwise |

### setReserveFreeze

```solidity
function setReserveFreeze(address asset, bool freeze) external override onlyRiskOrPoolAdmins
```

Freeze or unfreeze a reserve. A frozen reserve doesn't allow any new supply, borrow or rate swap but allows repayments, liquidations, rate rebalances and withdrawals.

#### Input Parameters:

| Name   | Type      | Description                                             |
| :----- | :-------- | :------------------------------------------------------ |
| asset  | `address` | The address of the underlying asset of the reserve      |
| freeze | `bool`    | True if the reserve needs to be frozen, false otherwise |

### setBorrowableInIsolation

```solidity
function setBorrowableInIsolation(address asset, bool borrowable) external override onlyRiskOrPoolAdmins
```

Sets the borrowable in isolation flag for the reserve. When this flag is set to true, the asset will be borrowable against isolated collaterals and the borrowed amount will be accumulated in the isolated collateral's total debt exposure. Only assets of the same family (e.g. USD stablecoins) should be borrowable in isolation mode to keep consistency in the debt ceiling calculations.

#### Input Parameters:

| Name       | Type      | Description                                                          |
| :--------- | :-------- | :------------------------------------------------------------------- |
| asset      | `address` | The address of the underlying asset of the reserve                   |
| borrowable | `bool`    | True if the asset should be borrowable in isolation, false otherwise |

### setReserveFactor

```solidity
function setReserveFactor(address asset, uint256 newReserveFactor) external override onlyRiskOrPoolAdmins
```

Updates the reserve factor of a reserve.

#### Input Parameters:

| Name             | Type      | Description                                        |
| :--------------- | :-------- | :------------------------------------------------- |
| asset            | `address` | The address of the underlying asset of the reserve |
| newReserveFactor | `uint256` | The new reserve factor of the reserve              |

### setDebtCeiling

```solidity
function setDebtCeiling(address asset, uint256 newDebtCeiling) external override onlyRiskOrPoolAdmins
```

Sets the debt ceiling for an asset.

#### Input Parameters:

| Name           | Type      | Description                                        |
| :------------- | :-------- | :------------------------------------------------- |
| asset          | `address` | The address of the underlying asset of the reserve |
| newDebtCeiling | `uint256` | The new debt ceiling                               |

### setSiloedBorrowing

```solidity
function setSiloedBorrowing(address asset, bool newSiloed) external override onlyRiskOrPoolAdmins
```

Sets siloed borrowing for an asset

#### Input Parameters:

| Name   | Type      | Description                                                                         |
| :----- | :-------- | :---------------------------------------------------------------------------------- |
| asset  | `address` | The address of the underlying asset of the reserve                                  |
| siloed | `bool`    | The new siloed borrowing state - enable or disable siloed borrowing for the reserve |

### setBorrowCap

```solidity
function setBorrowCap(address asset, uint256 newBorrowCap) external override onlyRiskOrPoolAdmins
```

Updates the borrow cap of a reserve. Allows `RISK_ADMIN` and `POOL_ADMIN` to add/update cap on the total borrow that can be borrowed from the reserve. Once the borrow cap is reached, no more borrow (variable or stable) for the given reserve asset can be initiated.

#### Input Parameters:

| Name         | Type      | Description                                                                                         |
| :----------- | :-------- | :-------------------------------------------------------------------------------------------------- |
| asset        | `address` | The address of the underlying asset of the reserve                                                  |
| newBorrowCap | `uint256` | The new borrow cap of the reserve in whole tokens. A borrow cap of 0 signifies that there is no cap |

### setSupplyCap

```solidity
function setSupplyCap(address asset, uint256 newSupplyCap) external override onlyRiskOrPoolAdmins
```

Updates the supply cap of a reserve. Allows `RISK_ADMIN` and `POOL_ADMIN` to add/update liquidity supply cap on the reserve. Once the supply cap is reached, no more liquidity for the given reserve asset can be supplied to the pool.

#### Input Parameters:

| Name         | Type      | Description                                                                                         |
| :----------- | :-------- | :-------------------------------------------------------------------------------------------------- |
| asset        | `address` | The address of the underlying asset of the reserve                                                  |
| newSupplyCap | `uint256` | The new supply cap of the reserve in whole tokens. A supply cap of 0 signifies that there is no cap |

### setLiquidationProtocolFee

```solidity
function setLiquidationProtocolFee(address asset, uint256 newFee) external override onlyRiskOrPoolAdmins
```

Updates the liquidation protocol fee of reserve.

#### Input Parameters:

| Name   | Type      | Description                                                       |
| :----- | :-------- | :---------------------------------------------------------------- |
| asset  | `address` | The address of the underlying asset of the reserve                |
| newFee | `uint256` | The new liquidation protocol fee of the reserve, expressed in bps |

### setEModeCategory

```solidity
function setEModeCategory(
    uint8 categoryId,
    uint16 ltv,
    uint16 liquidationThreshold,
    uint16 liquidationBonus,
    address oracle,
    string calldata label
) external override onlyRiskOrPoolAdmins 
```

Adds a new efficiency mode (eMode) category. Allows `RISK_ADMIN` and `POOL_ADMINS` to configure existing or add new `eModeCategory` 

If zero is provided as oracle address, the default asset oracles will be used to compute the overall debt and overcollateralization of the users using this category. The new ltv and liquidation threshold must be greater than the base ltvs and liquidation thresholds of all assets within the eMode category.

#### Input Parameters:

| Name                 | Type      | Description                                                                                                                   |
| :------------------- | :-------- | :---------------------------------------------------------------------------------------------------------------------------- |
| categoryId           | `uint8`   | The id of the category to be configured. categoryId ≠ 0. NOTE: category 0 is reserved for the default category i.e. non-eMode |
| ltv                  | `uint16`  | The loan to value for the associated eMode category. It must be less than or equal to the liquidationThreshold                |
| liquidationThreshold | `uint16`  | The liquidation threshold associated with the category                                                                        |
| liquidationBonus     | `uint16`  | The liquidation bonus associated with the category                                                                            |
| oracle               | `address` | The address of the custom price oracle associated with the category                                                           |
| label                | `string`  | A custom label identifying the category                                                                                       |

### setAssetEModeCategory

```solidity
function setAssetEModeCategory(address asset, uint8 newCategoryId) external override onlyRiskOrPoolAdmins
```

Assign an efficiency mode (eMode) category to asset. Allows `RISK_ADMIN` and `POOL_ADMINS` to configure `eModeCategory` of an asset.

#### Input Parameters:

| Name          | Type      | Description                                                                                                                          |
| :------------ | :-------- | :----------------------------------------------------------------------------------------------------------------------------------- |
| asset         | `address` | The address of the underlying asset of the reserve                                                                                   |
| newCategoryId | `uint8`   | If newCategoryId is not 0, it is one of the already defined `eModeCategory`. newCategoryId is 0 for default i.e. non-eMode category  |

### setUnbackedMintCap

```solidity
function setUnbackedMintCap(address asset, uint256 newUnbackedMintCap) external override onlyRiskOrPoolAdmins
```

Updates the unbacked mint cap of reserve.

#### Input Parameters:

| Name               | Type      | Description                                        |
| :----------------- | :-------- | :------------------------------------------------- |
| asset              | `address` | The address of the underlying asset of the reserve |
| newUnbackedMintCap | `uint256` | The new unbacked mint cap of the reserve           |

### setReserveInterestRateStrategyAddress

```solidity
function setReserveInterestRateStrategyAddress(address asset, address newRateStrategyAddress) external override onlyRiskOrPoolAdmins
```

Sets the interest rate strategy of a reserve.

#### Input Parameters:

| Name                   | Type      | Description                                        |
| :--------------------- | :-------- | :------------------------------------------------- |
| asset                  | `address` | The address of the underlying asset of the reserve |
| newRateStrategyAddress | `address` | The address of the new interest strategy contract  |

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
<summary>PoolConfigurator ABI</summary>

```
[
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
                "indexed": true,
                "internalType": "address",
                "name": "proxy",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "implementation",
                "type": "address"
            }
        ],
        "name": "ATokenUpgraded",
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
                "name": "oldBorrowCap",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "newBorrowCap",
                "type": "uint256"
            }
        ],
        "name": "BorrowCapChanged",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": false,
                "internalType": "address",
                "name": "asset",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "bool",
                "name": "borrowable",
                "type": "bool"
            }
        ],
        "name": "BorrowableInIsolationChanged",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "oldBridgeProtocolFee",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "newBridgeProtocolFee",
                "type": "uint256"
            }
        ],
        "name": "BridgeProtocolFeeUpdated",
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
                "name": "ltv",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "liquidationThreshold",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "liquidationBonus",
                "type": "uint256"
            }
        ],
        "name": "CollateralConfigurationChanged",
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
                "name": "oldDebtCeiling",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "newDebtCeiling",
                "type": "uint256"
            }
        ],
        "name": "DebtCeilingChanged",
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
                "internalType": "uint8",
                "name": "oldCategoryId",
                "type": "uint8"
            },
            {
                "indexed": false,
                "internalType": "uint8",
                "name": "newCategoryId",
                "type": "uint8"
            }
        ],
        "name": "EModeAssetCategoryChanged",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "uint8",
                "name": "categoryId",
                "type": "uint8"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "ltv",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "liquidationThreshold",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "liquidationBonus",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "address",
                "name": "oracle",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "string",
                "name": "label",
                "type": "string"
            }
        ],
        "name": "EModeCategoryAdded",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": false,
                "internalType": "uint128",
                "name": "oldFlashloanPremiumToProtocol",
                "type": "uint128"
            },
            {
                "indexed": false,
                "internalType": "uint128",
                "name": "newFlashloanPremiumToProtocol",
                "type": "uint128"
            }
        ],
        "name": "FlashloanPremiumToProtocolUpdated",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": false,
                "internalType": "uint128",
                "name": "oldFlashloanPremiumTotal",
                "type": "uint128"
            },
            {
                "indexed": false,
                "internalType": "uint128",
                "name": "newFlashloanPremiumTotal",
                "type": "uint128"
            }
        ],
        "name": "FlashloanPremiumTotalUpdated",
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
                "name": "oldFee",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "newFee",
                "type": "uint256"
            }
        ],
        "name": "LiquidationProtocolFeeChanged",
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
                "internalType": "bool",
                "name": "active",
                "type": "bool"
            }
        ],
        "name": "ReserveActive",
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
                "internalType": "bool",
                "name": "enabled",
                "type": "bool"
            }
        ],
        "name": "ReserveBorrowing",
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
            }
        ],
        "name": "ReserveDropped",
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
                "name": "oldReserveFactor",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "newReserveFactor",
                "type": "uint256"
            }
        ],
        "name": "ReserveFactorChanged",
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
                "internalType": "bool",
                "name": "frozen",
                "type": "bool"
            }
        ],
        "name": "ReserveFrozen",
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
                "indexed": true,
                "internalType": "address",
                "name": "aToken",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "address",
                "name": "stableDebtToken",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "address",
                "name": "variableDebtToken",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "address",
                "name": "interestRateStrategyAddress",
                "type": "address"
            }
        ],
        "name": "ReserveInitialized",
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
                "internalType": "address",
                "name": "oldStrategy",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "address",
                "name": "newStrategy",
                "type": "address"
            }
        ],
        "name": "ReserveInterestRateStrategyChanged",
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
                "internalType": "bool",
                "name": "paused",
                "type": "bool"
            }
        ],
        "name": "ReservePaused",
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
                "internalType": "bool",
                "name": "enabled",
                "type": "bool"
            }
        ],
        "name": "ReserveStableRateBorrowing",
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
                "internalType": "bool",
                "name": "oldState",
                "type": "bool"
            },
            {
                "indexed": false,
                "internalType": "bool",
                "name": "newState",
                "type": "bool"
            }
        ],
        "name": "SiloedBorrowingChanged",
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
                "indexed": true,
                "internalType": "address",
                "name": "proxy",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "implementation",
                "type": "address"
            }
        ],
        "name": "StableDebtTokenUpgraded",
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
                "name": "oldSupplyCap",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "newSupplyCap",
                "type": "uint256"
            }
        ],
        "name": "SupplyCapChanged",
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
                "name": "oldUnbackedMintCap",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "newUnbackedMintCap",
                "type": "uint256"
            }
        ],
        "name": "UnbackedMintCapChanged",
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
                "indexed": true,
                "internalType": "address",
                "name": "proxy",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "implementation",
                "type": "address"
            }
        ],
        "name": "VariableDebtTokenUpgraded",
        "type": "event"
    },
    {
        "inputs": [],
        "name": "CONFIGURATOR_REVISION",
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
                "name": "ltv",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "liquidationThreshold",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "liquidationBonus",
                "type": "uint256"
            }
        ],
        "name": "configureReserveAsCollateral",
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
                "components": [
                    {
                        "internalType": "address",
                        "name": "aTokenImpl",
                        "type": "address"
                    },
                    {
                        "internalType": "address",
                        "name": "stableDebtTokenImpl",
                        "type": "address"
                    },
                    {
                        "internalType": "address",
                        "name": "variableDebtTokenImpl",
                        "type": "address"
                    },
                    {
                        "internalType": "uint8",
                        "name": "underlyingAssetDecimals",
                        "type": "uint8"
                    },
                    {
                        "internalType": "address",
                        "name": "interestRateStrategyAddress",
                        "type": "address"
                    },
                    {
                        "internalType": "address",
                        "name": "underlyingAsset",
                        "type": "address"
                    },
                    {
                        "internalType": "address",
                        "name": "treasury",
                        "type": "address"
                    },
                    {
                        "internalType": "address",
                        "name": "incentivesController",
                        "type": "address"
                    },
                    {
                        "internalType": "string",
                        "name": "aTokenName",
                        "type": "string"
                    },
                    {
                        "internalType": "string",
                        "name": "aTokenSymbol",
                        "type": "string"
                    },
                    {
                        "internalType": "string",
                        "name": "variableDebtTokenName",
                        "type": "string"
                    },
                    {
                        "internalType": "string",
                        "name": "variableDebtTokenSymbol",
                        "type": "string"
                    },
                    {
                        "internalType": "string",
                        "name": "stableDebtTokenName",
                        "type": "string"
                    },
                    {
                        "internalType": "string",
                        "name": "stableDebtTokenSymbol",
                        "type": "string"
                    },
                    {
                        "internalType": "bytes",
                        "name": "params",
                        "type": "bytes"
                    }
                ],
                "internalType": "struct ConfiguratorInputTypes.InitReserveInput[]",
                "name": "input",
                "type": "tuple[]"
            }
        ],
        "name": "initReserves",
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
                "name": "asset",
                "type": "address"
            },
            {
                "internalType": "uint8",
                "name": "newCategoryId",
                "type": "uint8"
            }
        ],
        "name": "setAssetEModeCategory",
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
                "name": "newBorrowCap",
                "type": "uint256"
            }
        ],
        "name": "setBorrowCap",
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
                "name": "borrowable",
                "type": "bool"
            }
        ],
        "name": "setBorrowableInIsolation",
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
                "name": "newDebtCeiling",
                "type": "uint256"
            }
        ],
        "name": "setDebtCeiling",
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
            },
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
                "name": "oracle",
                "type": "address"
            },
            {
                "internalType": "string",
                "name": "label",
                "type": "string"
            }
        ],
        "name": "setEModeCategory",
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
                "name": "newFee",
                "type": "uint256"
            }
        ],
        "name": "setLiquidationProtocolFee",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "bool",
                "name": "paused",
                "type": "bool"
            }
        ],
        "name": "setPoolPause",
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
                "name": "active",
                "type": "bool"
            }
        ],
        "name": "setReserveActive",
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
                "name": "enabled",
                "type": "bool"
            }
        ],
        "name": "setReserveBorrowing",
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
                "name": "newReserveFactor",
                "type": "uint256"
            }
        ],
        "name": "setReserveFactor",
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
                "name": "freeze",
                "type": "bool"
            }
        ],
        "name": "setReserveFreeze",
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
                "name": "newRateStrategyAddress",
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
                "internalType": "address",
                "name": "asset",
                "type": "address"
            },
            {
                "internalType": "bool",
                "name": "paused",
                "type": "bool"
            }
        ],
        "name": "setReservePause",
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
                "name": "enabled",
                "type": "bool"
            }
        ],
        "name": "setReserveStableRateBorrowing",
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
                "name": "newSiloed",
                "type": "bool"
            }
        ],
        "name": "setSiloedBorrowing",
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
                "name": "newSupplyCap",
                "type": "uint256"
            }
        ],
        "name": "setSupplyCap",
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
                "name": "newUnbackedMintCap",
                "type": "uint256"
            }
        ],
        "name": "setUnbackedMintCap",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "components": [
                    {
                        "internalType": "address",
                        "name": "asset",
                        "type": "address"
                    },
                    {
                        "internalType": "address",
                        "name": "treasury",
                        "type": "address"
                    },
                    {
                        "internalType": "address",
                        "name": "incentivesController",
                        "type": "address"
                    },
                    {
                        "internalType": "string",
                        "name": "name",
                        "type": "string"
                    },
                    {
                        "internalType": "string",
                        "name": "symbol",
                        "type": "string"
                    },
                    {
                        "internalType": "address",
                        "name": "implementation",
                        "type": "address"
                    },
                    {
                        "internalType": "bytes",
                        "name": "params",
                        "type": "bytes"
                    }
                ],
                "internalType": "struct ConfiguratorInputTypes.UpdateATokenInput",
                "name": "input",
                "type": "tuple"
            }
        ],
        "name": "updateAToken",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "uint256",
                "name": "newBridgeProtocolFee",
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
                "name": "newFlashloanPremiumToProtocol",
                "type": "uint128"
            }
        ],
        "name": "updateFlashloanPremiumToProtocol",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "uint128",
                "name": "newFlashloanPremiumTotal",
                "type": "uint128"
            }
        ],
        "name": "updateFlashloanPremiumTotal",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "components": [
                    {
                        "internalType": "address",
                        "name": "asset",
                        "type": "address"
                    },
                    {
                        "internalType": "address",
                        "name": "incentivesController",
                        "type": "address"
                    },
                    {
                        "internalType": "string",
                        "name": "name",
                        "type": "string"
                    },
                    {
                        "internalType": "string",
                        "name": "symbol",
                        "type": "string"
                    },
                    {
                        "internalType": "address",
                        "name": "implementation",
                        "type": "address"
                    },
                    {
                        "internalType": "bytes",
                        "name": "params",
                        "type": "bytes"
                    }
                ],
                "internalType": "struct ConfiguratorInputTypes.UpdateDebtTokenInput",
                "name": "input",
                "type": "tuple"
            }
        ],
        "name": "updateStableDebtToken",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "components": [
                    {
                        "internalType": "address",
                        "name": "asset",
                        "type": "address"
                    },
                    {
                        "internalType": "address",
                        "name": "incentivesController",
                        "type": "address"
                    },
                    {
                        "internalType": "string",
                        "name": "name",
                        "type": "string"
                    },
                    {
                        "internalType": "string",
                        "name": "symbol",
                        "type": "string"
                    },
                    {
                        "internalType": "address",
                        "name": "implementation",
                        "type": "address"
                    },
                    {
                        "internalType": "bytes",
                        "name": "params",
                        "type": "bytes"
                    }
                ],
                "internalType": "struct ConfiguratorInputTypes.UpdateDebtTokenInput",
                "name": "input",
                "type": "tuple"
            }
        ],
        "name": "updateVariableDebtToken",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    }
]
```
</details>