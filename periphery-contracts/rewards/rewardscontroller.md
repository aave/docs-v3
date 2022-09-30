# RewardsController

Abstract contract template to build Distributors contracts for ERC20 rewards to protocol participants. All rewards type enabled in Aave V3 are managed by [RewardsDistributor.sol](https://github.com/aave/aave-v3-periphery/blob/master/contracts/rewards/RewardsDistributor.sol). This is the contract used to check rewards data, user’s rewards balance and for claiming the rewards.

## Write Methods

### initialize

```solidity
function initialize(address emissionManager) external initializer
```

Initialize for RewardsController.

#### Input Parameters:

| Name            | Type      | Description                        |
| :-------------- | :-------- | :--------------------------------- |
| emissionManager | `address` | The address of the EmissionManager |

### configureAssets

```solidity
function configureAssets(RewardsDataTypes.RewardsConfigInput[] memory config) external override onlyEmissionManager
```

Configure assets to incentivize with an emission of rewards per second until the end of distribution.

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
function setTransferStrategy(address reward, ITransferStrategyBase transferStrategy) external onlyEmissionManager
```

Sets a TransferStrategy logic contract that determines the logic of the rewards transfer.

#### Input Parameters:

| Name              | Type                  | Description                                        |
| :---------------- | :-------------------- | :------------------------------------------------- |
| reward            | `address`             | The address of the reward token                    |
| transferStrategy  | `ITransferStrategy`   | The address of the TransferStrategy logic contract |

### setRewardOracle

```solidity
function setRewardOracle(address reward, IEACAggregatorProxy rewardOracle) external onlyEmissionManager
```

Sets an Aave Oracle contract to enforce rewards with a source of value. At the moment of reward configuration, the Incentives Controller performs a check to see if the reward asset oracle is compatible with IEACAggregator proxy. This check is enforced for integrators to be able to show incentives at the current Aave UI without the need to setup an external price registry.

{% hint style="info" %}
The `msg.sender` must be an authorised claimer set using `setClaimer()` method, via Governance Vote.
{% endhint %}

#### Input Parameters:

| Name         | Type                  | Description                                                                |
| :----------- | :-------------------- | :------------------------------------------------------------------------- |
| reward       | `address`             | The address of the reward to set the price aggregator                      |
| rewardOracle | `IEACAggregatorProxy` | The address of price aggregator that follows IEACAggregatorProxy interface |

### handleAction

```solidity 
function handleAction(address user, uint256 totalSupply, uint256 userBalance) external override
```

Called by the corresponding asset on any update that affects the rewards distribution.

#### Input Parameters:

| Name        | Type      | Description                   |
| :---------- | :-------- | :---------------------------- |
| user        | `address` | The address of the user       |
| totalSupply | `uint256` | The user balance of the asset |
| userBalance | `uint256` | The total supply of the asset |

### claimRewards

```solidity
function claimRewards(
    address[] calldata assets,
    uint256 amount,
    address to,
    address reward
) external override returns (uint256)
```

Claims reward for a user to the desired address, on all the assets of the pool, accumulating the pending rewards. Rewards are received by `to` address.

#### Input Parameters:

| Name   | Type        | Description                                                                                                |
| :----- | :---------- | :--------------------------------------------------------------------------------------------------------- |
| assets | `address[]` | List of assets to check eligible distributions before claiming rewards. Pass a/s/vToken addresses          |
| amount | `uint256`   | The amount of rewards to claim, expressed in wei. Pass `MAX_UINT` to claim entire unclaimed reward balance |
| to     | `address`   | The address that will be receiving the rewards                                                             |
| reward | `address`   | The address of the reward token eg. stkAaave                                                               |


#### Return Values:

| Type      | Description                   |
| :-------- | :---------------------------- |
| `uint256` | The amount of rewards claimed |


### claimRewardsOnBehalf

```solidity
function claimRewardsOnBehalf(
    address[] calldata assets,
    uint256 amount,
    address user,
    address to,
    address reward
) external override onlyAuthorizedClaimers(msg.sender, user) returns (uint256)
```

Claims reward for a user on behalf, on all the assets of the pool, accumulating the pending rewards. The  caller must be whitelisted via "allowClaimOnBehalf" function by the RewardsAdmin role manager Rewards are received by `to` address.

{% hint style="info" %}
The `msg.sender` must be an authorised claimer set using `setClaimer()` method, via Governance Vote.
{% endhint %}

#### Input Parameters:

| Name   | Type        | Description                                                                                                |
| :----- | :---------- | :--------------------------------------------------------------------------------------------------------- |
| assets | `address[]` | The list of assets to check eligible distributions before claiming rewards. Pass a/s/vToken addresses      |
| amount | `uint256`   | The amount of rewards to claim, expressed in wei. Pass `MAX_UINT` to claim entire unclaimed reward balance |
| user   | `address`   | The address to check and claim rewards                                                                     |
| to     | `address`   | The address that will be receiving the rewards                                                             |
| reward | `address`   | The address of the reward token being claimed. eg. stkAaave                                                |

#### Return Values:

| Type      | Description                   |
| :-------- | :---------------------------- |
| `uint256` | The amount of rewards claimed |

### claimRewardsToSelf

```solidity
function claimRewardsToSelf(address[] calldata assets, uint256 amount, address reward) external override returns (uint256)
```

Claims reward for `msg.sender`, on all the assets of the pool, accumulating the pending rewards. Rewards are received by `msg.sender` .

#### Input Parameters:

| Name   | Type        | Description                                                                                               |
| :----- | :---------- | :-------------------------------------------------------------------------------------------------------- |
| assets | `address[]` | The list of assets to check eligible distributions before claiming rewards. Pass a/s/vToken addresses     |
| amount | `uint256`   | The amount of rewards to claim, expressed in wei. Pass `MAX_UINT` to claim entire unclaimed reward balance |
| reward | `address`   | The address of the reward token                                                                           |

#### Return Values:

| Type      | Description                   |
| :-------- | :---------------------------- |
| `uint256` | The amount of rewards claimed |

### claimAllRewards

```solidity
function claimAllRewards(address[] calldata assets, address to) external override returns (address[] memory rewardsList, uint256[] memory claimedAmounts)
```

Claims all rewards for a user to the desired address, on all the assets of the pool, accumulating the pending rewards. Rewards are received by `to` address.

#### Input Parameters:

| Name   | Type        | Description                                                                                           |
| :----- | :---------- | :---------------------------------------------------------------------------------------------------- |
| assets | `address[]` | The list of assets to check eligible distributions before claiming rewards. Pass a/s/vToken addresses |
| to     | `address`   | The address that will be receiving the rewards                                                        |

#### Return Values:

| Name           | Type        | Description                                                                                    |
| :------------- | :---------- | :--------------------------------------------------------------------------------------------- |
| rewardsList    | `address[]` | The list of addresses of the reward tokens. Pass a/s/vToken addresses                          |
| claimedAmounts | `uint256[]` | The list that contains the claimed amount per reward, following the same order as "rewardList" |

### claimAllRewardsOnBehalf

```solidity
function claimAllRewardsOnBehalf(
    address[] calldata assets,
    address user,
    address to
) external override onlyAuthorizedClaimers(msg.sender, user) returns (address[] memory rewardsList, uint256[] memory claimedAmounts)
```

Claims all rewards for a user on behalf, on all the assets of the pool, accumulating the pending rewards. The caller must be whitelisted via "allowClaimOnBehalf" function by the RewardsAdmin role manager. Rewards are received by `to` address.

{% hint style="info" %}
The `msg.sender` must be an authorised claimer set using `setClaimer()` method, via Governance Vote.
{% endhint %}

#### Input Parameters:

| Name   | Type        | Description                                                                                           |
| :----- | :---------- | :---------------------------------------------------------------------------------------------------- |
| assets | `address[]` | The list of assets to check eligible distributions before claiming rewards. Pass a/s/vToken addresses |
| user   | `address`   | The address to check and claim rewards                                                                |
| to     | `address`   | The address that will be receiving the rewards. eg. stkAaave                                          |

#### Return Values:

| Name           | Type        | Description                                                                                    |
| :------------- | :---------- | :--------------------------------------------------------------------------------------------- |
| rewardsList    | `address[]` | The list of addresses of the reward tokens. Pass a/s/vToken addresses                          |
| claimedAmounts | `uint256[]` | The list that contains the claimed amount per reward, following the same order as "rewardList" |

### claimAllRewardsToSelf

```solidity
function claimAllRewardsToSelf(address[] calldata assets) external override returns (address[] memory rewardsList, uint256[] memory claimedAmounts)
```

Claims all rewards accrued by `msg.sender`, on all assets of the pool, accumulating the pending rewards. Rewards are received by `msg.sender`.

#### Input Parameters:

| Name   | Type        | Description                                                                                           |
| :----- | :---------- | :---------------------------------------------------------------------------------------------------- |
| assets | `address[]` | The list of assets to check eligible distributions before claiming rewards. Pass a/s/vToken addresses |

#### Return Values:

| Name           | Type        | Description                                                                                    |
| :------------- | :---------- | :--------------------------------------------------------------------------------------------- |
| rewardsList    | `address[]` | The list of addresses of the reward tokens. Pass a/s/vToken addresses                          |
| claimedAmounts | `uint256[]` | The list that contains the claimed amount per reward, following the same order as "rewardList" |

### setClaimer

```solidity
function setClaimer(address user, address caller) external override onlyEmissionManager
```

Whitelists an address to claim the rewards on behalf of another address.

#### Input Parameters:

| Name   | Type      | Description                                           |
| :----- | :-------- | :---------------------------------------------------- |
| user   | `address` | The address of the user. Pass a/s/vToken addresses    | 
| caller | `address` | The address of the claimer. Pass a/s/vToken addresses |

## View Methods

### getClaimer

```solidity
function getClaimer(address user) external view override returns (address)
```

Returns the whitelisted claimer for a certain address (0x0 if not set).

#### Input Parameters:

| Name | Type      | Description             |
| :--- | :-------- | :---------------------- |
| user | `address` | The address of the user | 

#### Return Values:

| Type      | Description         |
| :-------- | :------------------ |
| `address` | The claimer address |

### getRewardOracle

```solidity
function getRewardOracle(address reward) external view override returns (address)
```

Get the price aggregator oracle address.

| Name | Type      | Description               |
| :--- | :-------- | :------------------------ |
| user | `address` | The address of the reward | 

#### Return Values:

| Type      | Description                    |
| :-------- | :----------------------------- |
| `address` | The price oracle of the reward |

### getTransferStrategy

```solidity
function getTransferStrategy(address reward) external view override returns (address)
```

Returns the Transfer Strategy implementation contract address being used for a reward address.

| Name | Type      | Description                 |
| :--- | :-------- | :-------------------------- |
| reward | `address` | The address of the reward | 

#### Return Values:

| Type      | Description                                  |
| :-------- | :------------------------------------------- |
| `address` | The address of the TransferStrategy contract |

## Pure Methods

```solidity
function getRevision() internal pure override returns (uint256)
```

Returns the revision of the implementation contract.

#### Return Values:

| Type      | Description                  |
| :-------- | :--------------------------- |
| `uint256` | The current revision version |

## ABI
<details>
<summary>RewardsController ABI</summary>

```
[
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "emissionManager",
                "type": "address"
            }
        ],
        "stateMutability": "nonpayable",
        "type": "constructor"
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
                "name": "reward",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "user",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "assetIndex",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "userIndex",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "rewardsAccrued",
                "type": "uint256"
            }
        ],
        "name": "Accrued",
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
                "name": "reward",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "oldEmission",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "newEmission",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "oldDistributionEnd",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "newDistributionEnd",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "assetIndex",
                "type": "uint256"
            }
        ],
        "name": "AssetConfigUpdated",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "user",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "claimer",
                "type": "address"
            }
        ],
        "name": "ClaimerSet",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "oldEmissionManager",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "newEmissionManager",
                "type": "address"
            }
        ],
        "name": "EmissionManagerUpdated",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "reward",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "rewardOracle",
                "type": "address"
            }
        ],
        "name": "RewardOracleUpdated",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "user",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "reward",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "to",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "address",
                "name": "claimer",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            }
        ],
        "name": "RewardsClaimed",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "reward",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "transferStrategy",
                "type": "address"
            }
        ],
        "name": "TransferStrategyInstalled",
        "type": "event"
    },
    {
        "inputs": [],
        "name": "REVISION",
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
                "internalType": "address[]",
                "name": "assets",
                "type": "address[]"
            },
            {
                "internalType": "address",
                "name": "to",
                "type": "address"
            }
        ],
        "name": "claimAllRewards",
        "outputs": [
            {
                "internalType": "address[]",
                "name": "rewardsList",
                "type": "address[]"
            },
            {
                "internalType": "uint256[]",
                "name": "claimedAmounts",
                "type": "uint256[]"
            }
        ],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address[]",
                "name": "assets",
                "type": "address[]"
            },
            {
                "internalType": "address",
                "name": "user",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "to",
                "type": "address"
            }
        ],
        "name": "claimAllRewardsOnBehalf",
        "outputs": [
            {
                "internalType": "address[]",
                "name": "rewardsList",
                "type": "address[]"
            },
            {
                "internalType": "uint256[]",
                "name": "claimedAmounts",
                "type": "uint256[]"
            }
        ],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address[]",
                "name": "assets",
                "type": "address[]"
            }
        ],
        "name": "claimAllRewardsToSelf",
        "outputs": [
            {
                "internalType": "address[]",
                "name": "rewardsList",
                "type": "address[]"
            },
            {
                "internalType": "uint256[]",
                "name": "claimedAmounts",
                "type": "uint256[]"
            }
        ],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address[]",
                "name": "assets",
                "type": "address[]"
            },
            {
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            },
            {
                "internalType": "address",
                "name": "to",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "reward",
                "type": "address"
            }
        ],
        "name": "claimRewards",
        "outputs": [
            {
                "internalType": "uint256",
                "name": "",
                "type": "uint256"
            }
        ],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address[]",
                "name": "assets",
                "type": "address[]"
            },
            {
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            },
            {
                "internalType": "address",
                "name": "user",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "to",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "reward",
                "type": "address"
            }
        ],
        "name": "claimRewardsOnBehalf",
        "outputs": [
            {
                "internalType": "uint256",
                "name": "",
                "type": "uint256"
            }
        ],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address[]",
                "name": "assets",
                "type": "address[]"
            },
            {
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            },
            {
                "internalType": "address",
                "name": "reward",
                "type": "address"
            }
        ],
        "name": "claimRewardsToSelf",
        "outputs": [
            {
                "internalType": "uint256",
                "name": "",
                "type": "uint256"
            }
        ],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "components": [
                    {
                        "internalType": "uint88",
                        "name": "emissionPerSecond",
                        "type": "uint88"
                    },
                    {
                        "internalType": "uint256",
                        "name": "totalSupply",
                        "type": "uint256"
                    },
                    {
                        "internalType": "uint32",
                        "name": "distributionEnd",
                        "type": "uint32"
                    },
                    {
                        "internalType": "address",
                        "name": "asset",
                        "type": "address"
                    },
                    {
                        "internalType": "address",
                        "name": "reward",
                        "type": "address"
                    },
                    {
                        "internalType": "contract ITransferStrategyBase",
                        "name": "transferStrategy",
                        "type": "address"
                    },
                    {
                        "internalType": "contract IEACAggregatorProxy",
                        "name": "rewardOracle",
                        "type": "address"
                    }
                ],
                "internalType": "struct RewardsDataTypes.RewardsConfigInput[]",
                "name": "config",
                "type": "tuple[]"
            }
        ],
        "name": "configureAssets",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address[]",
                "name": "assets",
                "type": "address[]"
            },
            {
                "internalType": "address",
                "name": "user",
                "type": "address"
            }
        ],
        "name": "getAllUserRewards",
        "outputs": [
            {
                "internalType": "address[]",
                "name": "rewardsList",
                "type": "address[]"
            },
            {
                "internalType": "uint256[]",
                "name": "unclaimedAmounts",
                "type": "uint256[]"
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
            }
        ],
        "name": "getAssetDecimals",
        "outputs": [
            {
                "internalType": "uint8",
                "name": "",
                "type": "uint8"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "user",
                "type": "address"
            }
        ],
        "name": "getClaimer",
        "outputs": [
            {
                "internalType": "address",
                "name": "",
                "type": "address"
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
                "internalType": "address",
                "name": "reward",
                "type": "address"
            }
        ],
        "name": "getDistributionEnd",
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
        "inputs": [],
        "name": "getEmissionManager",
        "outputs": [
            {
                "internalType": "address",
                "name": "",
                "type": "address"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "reward",
                "type": "address"
            }
        ],
        "name": "getRewardOracle",
        "outputs": [
            {
                "internalType": "address",
                "name": "",
                "type": "address"
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
            }
        ],
        "name": "getRewardsByAsset",
        "outputs": [
            {
                "internalType": "address[]",
                "name": "",
                "type": "address[]"
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
                "internalType": "address",
                "name": "reward",
                "type": "address"
            }
        ],
        "name": "getRewardsData",
        "outputs": [
            {
                "internalType": "uint256",
                "name": "",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "",
                "type": "uint256"
            },
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
        "inputs": [],
        "name": "getRewardsList",
        "outputs": [
            {
                "internalType": "address[]",
                "name": "",
                "type": "address[]"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "reward",
                "type": "address"
            }
        ],
        "name": "getTransferStrategy",
        "outputs": [
            {
                "internalType": "address",
                "name": "",
                "type": "address"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "user",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "reward",
                "type": "address"
            }
        ],
        "name": "getUserAccruedRewards",
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
                "name": "user",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "asset",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "reward",
                "type": "address"
            }
        ],
        "name": "getUserAssetIndex",
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
                "internalType": "address[]",
                "name": "assets",
                "type": "address[]"
            },
            {
                "internalType": "address",
                "name": "user",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "reward",
                "type": "address"
            }
        ],
        "name": "getUserRewards",
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
                "name": "user",
                "type": "address"
            },
            {
                "internalType": "uint256",
                "name": "totalSupply",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "userBalance",
                "type": "uint256"
            }
        ],
        "name": "handleAction",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "emissionManager",
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
                "name": "user",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "caller",
                "type": "address"
            }
        ],
        "name": "setClaimer",
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
                "name": "reward",
                "type": "address"
            },
            {
                "internalType": "uint32",
                "name": "newDistributionEnd",
                "type": "uint32"
            }
        ],
        "name": "setDistributionEnd",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "emissionManager",
                "type": "address"
            }
        ],
        "name": "setEmissionManager",
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
                "internalType": "address[]",
                "name": "rewards",
                "type": "address[]"
            },
            {
                "internalType": "uint88[]",
                "name": "newEmissionsPerSecond",
                "type": "uint88[]"
            }
        ],
        "name": "setEmissionPerSecond",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "reward",
                "type": "address"
            },
            {
                "internalType": "contract IEACAggregatorProxy",
                "name": "rewardOracle",
                "type": "address"
            }
        ],
        "name": "setRewardOracle",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "reward",
                "type": "address"
            },
            {
                "internalType": "contract ITransferStrategyBase",
                "name": "transferStrategy",
                "type": "address"
            }
        ],
        "name": "setTransferStrategy",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    }
]
```
</details>