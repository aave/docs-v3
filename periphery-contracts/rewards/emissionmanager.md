# EmissionManager

Manages the list of admins of reward emissions and provides functions to control reward emissions.

## Write Methods

### configureAssets

```solidity
function configureAssets(RewardsDataTypes.RewardsConfigInput[] memory config) external override 
```

Configure assets to incentivize with an emission of rewards per second until the end of distribution.  The assets configurations input contains list of structs with the fields in Input Parameters.

{% hint style="info" %}
It is only callable by the emission admin of the given rewards.
{% endhint %}

#### Input Parameters:

| Name              | Type                  | Description                                                                                                                                                  |
| :---------------- | :-------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| emissionPerSecond | `uint104`             | The emission per second following rewards unit decimals                                                                                                      |
| totalSupply       | `uint256`             | The total supply of the asset to incentivize                                                                                                                 |
| distributionEnd   | `uint40`              | The end of the distribution of the incentives for an asset                                                                                                   |
| asset             | `address`             | The asset address to incentivize                                                                                                                             |
| reward            | `address`             | The reward token address                                                                                                                                     |
| transferStrategy  | `ITransferStrategy`   | The TransferStrategy address with the install hook and claim logic                                                                                           |
| rewardOracle      | `IEACAggregatorProxy` | The Price Oracle of a reward to visualize the incentives at the UI Frontend. Must follow Chainlink Aggregator IEACAggregatorProxy interface to be compatible |

### setTransferStrategy

```solidity
function setTransferStrategy(address reward, ITransferStrategyBase transferStrategy) external override onlyEmissionAdmin(reward)
```

Sets a TransferStrategy logic contract that determines the logic of the rewards transfer. 

{% hint style="info" %}
It is only callable by the emission admin of the given reward.
{% endhint %}

#### Input Parameters:

| Name             | Type                    | Description                                        |
| :--------------- | :---------------------- | :------------------------------------------------- |
| reward           | `address`               | The address of the reward token                    |
| transferStrategy | `ITransferStrategyBase` | The address of the TransferStrategy logic contract |

### setRewardOracle

```solidity
function setRewardOracle(address reward, IEACAggregatorProxy rewardOracle) external override onlyEmissionAdmin(reward)
```
At the moment of reward configuration, the Incentives Controller performs a check to see if the reward asset oracle is compatible with IEACAggregator proxy. This check is enforced for integrators to be able to show incentives at the current Aave UI without the need to setup an external price registry.

Sets an Aave Oracle contract to enforce rewards with a source of value. 

{% hint style="info" %}
It is only callable by the emission admin of the given reward.
{% endhint %}

#### Input Parameters:

| Name         | Type                  | Description                                                                |
| :----------- | :-------------------- | :------------------------------------------------------------------------- |
| reward       | `address`             | The address of the reward to set the price aggregator                      |
| rewardOracle | `IEACAggregatorProxy` | The address of price aggregator that follows IEACAggregatorProxy interface |

### setDistributionEnd

``` solidity
function setDistributionEnd(address asset, address reward, uint32 newDistributionEnd) external override onlyEmissionAdmin(reward)
```

Sets the end date for the distribution. It is only callable by the emission admin of the given reward.

{% hint style="info" %}
It is only callable by the emission admin of the given reward.
{% endhint %}

#### Input Parameters:

| Name               | Type      | Description                                              |
| :----------------- | :-------- | :------------------------------------------------------- |
| asset              | `address` | The asset to incentivize                                 |
| reward             | `address` | The reward token that incentives the asset               |
| newDistributionEnd | `uint32`  | The end date of the incentivization, in unix time format |

### setEmissionPerSecond

```solidity
function setEmissionPerSecond(address asset, address[] calldata rewards, uint88[] calldata newEmissionsPerSecond) external override
```
Sets the emission per second of a set of reward distributions.

#### Input Parameters:

| Name                  | Type        | Description                                        |
| :-------------------- | :---------- | :------------------------------------------------- |
| asset                 | `address`   | The asset to incentivize                           |
| rewards               | `address[]` | The list of reward addresses are being distributed |
| newEmissionsPerSecond | `uint88[]`  | The list of new reward emissions per second        |


### setClaimer

```solidity
function setClaimer(address user, address claimer) external override onlyOwner
```

Whitelists an address to claim the rewards on behalf of another address.

{% hint style="info" %}
Only callable by the owner of the EmissionManager.
{% endhint %}

#### Input Parameters:

| Name    | Type      | Description                |
| :------ | :-------- | :------------------------- |
| user    | `address` | The address of the user    |
| claimer | `address` | The address of the claimer |

### setEmissionManager

```solidity
function setEmissionManager(address emissionManager) external override onlyOwner
```

Updates the address of the emission manager.

{% hint style="info" %}
Only callable by the owner of the EmissionManager.
{% endhint %}

#### Input Parameters:

| Name            | Type      | Description                            |
| :-------------- | :-------- | :------------------------------------- |
| emissionManager | `address` | The address of the new EmissionManager |

### setEmissionAdmin

```solidity
function setEmissionAdmin(address reward, address admin) external override onlyOwner
```

Updates the admin of the reward emission.

{% hint style="info" %}
Only callable by the owner of the EmissionManager.
{% endhint %}

#### Input Parameters:

| Name   | Type      | Description                                  |
| :----- | :-------- | :------------------------------------------- |
| reward | `address` | The address of the reward token              |
| admin  | `address` | The address of the new admin of the emission |

### setRewardsController

```solidity
function setRewardsController(address controller) external override onlyOwner
```

Updates the address of the rewards controller.

{% hint style="info" %}
Only callable by the owner of the EmissionManager.
{% endhint %}

#### Input Parameters:

| Name       | Type      | Description                                   |
| :--------- | :-------- | :-------------------------------------------- |
| controller | `address` | The address of the RewardsController contract |

## View Methods

### getRewardsController

```solidity
function getRewardsController() external view override returns (IRewardsController)
```

Returns the rewards controller address.

#### Return Values:

| Type                 | Description                                   |
| :------------------- | :-------------------------------------------- |
| `IRewardsController` | The address of the RewardsController contract |

### getEmissionAdmin

```solidity
function getEmissionAdmin(address reward) external view override returns (address)
```

Returns the admin of the given reward emission.

#### Input Parameters:

| Name   | Type      | Description                     |
| :----- | :-------- | :------------------------------ |
| reward | `address` | The address of the reward token |

#### Return Values:

| Type      | Description                       |
| :-------- | :-------------------------------- |
| `address` | The address of the emission admin |
