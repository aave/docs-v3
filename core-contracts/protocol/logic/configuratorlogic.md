# ConfiguratorLogic

The ConfiguratorLogic implements the functions to initialize reserves and update aTokens and debtTokens.

Source code available on [GitHub](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/libraries/logic/ConfiguratorLogic.sol).

## Write Methods

### executeInitReserve

```solidity
function executeInitReserve(IPool pool, ConfiguratorInputTypes.InitReserveInput calldata input) public
```

Initialize a reserve by creating and initializing aToken, stable debt token and variable debt token.

Emits the `ReserveInitialized` event.

#### Input Parameters:

| Name  | Type                                      | Description                                       |
| :---- | :---------------------------------------- | :------------------------------------------------ |
| pool  | `IPool`                                   | The Pool in which the reserve will be initialized |
| input | `ConfiguratorInputTypes.InitReserveInput` | The needed parameters for the initialization      |

### executeUpdateAToken

```solidity
function executeUpdateAToken(
    IPool cachedPool,
    ConfiguratorInputTypes.UpdateATokenInput calldata input
) public 
```

Updates the aToken implementation and initializes it.

Emits the `ATokenUpgraded` event.

#### Input Parameters:

| Name       | Type                                       | Description                                     |
| :--------- | :----------------------------------------- | :---------------------------------------------- |
| cachedPool | `IPool`                                    | The Pool containing the reserve with the aToken |
| input      | `ConfiguratorInputTypes.UpdateATokenInput` | The parameters needed for the initialize call   |

### executeUpdateStableDebtToken

```solidity
function executeUpdateStableDebtToken(
    IPool cachedPool,
    ConfiguratorInputTypes.UpdateDebtTokenInput calldata input
) public 
```
Updates the stable debt token implementation and initializes it.

Emits the `StableDebtTokenUpgraded` event.

#### Input Parameters:

| Name       | Type                                          | Description                                                |
| :--------- | :-------------------------------------------- | :--------------------------------------------------------- |
| cachedPool | `IPool`                                       | The Pool containing the reserve with the stable debt token |
| input      | `ConfiguratorInputTypes.UpdateDebtTokenInput` | The parameters needed for the initialize call              |

### executeUpdateVariableDebtToken 

```solidity
function executeUpdateVariableDebtToken(
    IPool cachedPool,
    ConfiguratorInputTypes.UpdateDebtTokenInput calldata input
) public 
```

Updates the variable debt token implementation and initializes it.

Emits the `VariableDebtTokenUpgraded` event.

#### Input Parameters:

| Name       | Type                                          | Description                                                  |
| :--------- | :-------------------------------------------- | :----------------------------------------------------------- |
| cachedPool | `IPool`                                       | The Pool containing the reserve with the variable debt token |
| input      | `ConfiguratorInputTypes.UpdateDebtTokenInput` | The parameters needed for the initialize call                |

## Configurator Input Types

The data types below are from the [ConfiguratorInputTypes library](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/libraries/types/ConfiguratorInputTypes.sol). They are used throughout the ConfiguratorLogic library.

### InitReserveInput

```solidity
struct InitReserveInput {
    address aTokenImpl;
    address stableDebtTokenImpl;
    address variableDebtTokenImpl;
    uint8 underlyingAssetDecimals;
    address interestRateStrategyAddress;
    address underlyingAsset;
    address treasury;
    address incentivesController;
    string aTokenName;
    string aTokenSymbol;
    string variableDebtTokenName;
    string variableDebtTokenSymbol;
    string stableDebtTokenName;
    string stableDebtTokenSymbol;
    bytes params;
}
```

### UpdateATokenInput

```solidity
struct UpdateATokenInput {
    address asset;
    address treasury;
    address incentivesController;
    string name;
    string symbol;
    address implementation;
    bytes params;
}
```

### UpdateDebtTokenInput

```solidity
struct UpdateDebtTokenInput {
    address asset;
    address incentivesController;
    string name;
    string symbol;
    address implementation;
    bytes params;
}
```