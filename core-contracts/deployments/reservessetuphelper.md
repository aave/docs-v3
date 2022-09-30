# ReservesSetupHelper

Deployment helper to setup the assets risk parameters at PoolConfigurator in batch.

The ReservesSetupHelper is an Ownable contract, so only the deployer or future owners can call this contract.

## Write Methods

```solidity
function configureReserves(
    PoolConfigurator configurator,
    ConfigureReserveInput[] calldata inputParams
) external onlyOwner
```

External function called by the owner account to setup the assets risk parameters in batch.

The Pool or Risk admin must transfer the ownership to ReservesSetupHelper before calling this function.

#### Input Parameters:

| Name | Type    | Description            |
| :--- | :------ | :--------------------- |
| configurator | `PoolConfigurator` | The address of PoolConfigurator contract |
| inputParams | `ConfigureReserveInput[]` | An array of ConfigureReserveInput struct that contains the assets and their risk parameters |