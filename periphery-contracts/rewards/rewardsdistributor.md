# RewardsDistributor

An accounting contract to manage multiple staking distributions with multiple rewards.

## Write Methods

### setDistributionEnd

``` solidity
function setDistributionEnd(address asset, address reward, uint32 newDistributionEnd) external override onlyEmissionManager
```

Sets the end date for the distribution.

#### Input Parameters:

| Name               | Type      | Description                                              |
| :----------------- | :-------- | :------------------------------------------------------- |
| asset              | `address` | The asset to incentivize                                 |
| reward             | `address` | The reward token that incentives the asset               |
| newDistributionEnd | `uint32`  | The end date of the incentivization, in unix time format |

### setEmissionPerSecond

``` solidity
function setEmissionPerSecond(
    address asset,
    address[] calldata rewards,
    uint88[] calldata newEmissionsPerSecond
) external override onlyEmissionManager
```
Sets the emission per second of a set of reward distributions.

#### Input Parameters:

| Name                  | Type        | Description                                             |
| :-------------------- | :---------- | :------------------------------------------------------ |
| asset                 | `address`   | The asset that is being incentivized                    |
| rewards               | `address[]` | The list of reward addresses that are being distributed |
| newEmissionsPerSecond | `uint88[]`  | The list of new reward emissions per second             |

## View Methods

### getRewardsData

```solidity
function getRewardsData(address asset, address reward) public view override returns (uint256, uint256, uint256, uint256)
```

Get the data of the reward emitted for the asset. Returns the configuration of the distribution reward for a certain asset.

#### Input Parameters:

| Name   | Type      | Description                                |
| :----- | :-------- | :----------------------------------------- |
| asset  | `address` | The incentivized asset                     |
| reward | `address` | The reward token of the incentivized asset |

#### Return Values:

| Type      | Description                                        |
| :-------- | :------------------------------------------------- |
| `uint256` | The index of the asset distribution                |
| `uint256` | The emission per second of the reward distribution |
| `uint256` | The timestamp of the last update of the index      |
| `uint256` | The timestamp of the distribution end              |

### getDistributionEnd
```solidity
function getDistributionEnd(address asset, address reward) external view override returns (uint256)
```

Gets the end date for the distribution.

#### Input Parameters:

| Name   | Type      | Description                                |
| :----- | :-------- | :----------------------------------------- |
| asset  | `address` | The incentivized asset                     |
| reward | `address` | The reward token of the incentivized asset |

#### Return Values:

| Type      | Description                                                         |
| :-------- | :------------------------------------------------------------------ |
| `uint256` | The timestamp with the end of the distribution, in unix time format |

### getRewardsByAsset

```solidity
function getRewardsByAsset(address asset) external view override returns (address[] memory)
```

Returns the list of available reward token addresses of an incentivized asset.

#### Input Parameters:

| Name  | Type      | Description                                  |
| :---- | :-------- | :------------------------------------------- |
| asset | `address` | List of rewards addresses of the input asset |

#### Return Values:

| Type        | Description                                                         |
| :---------- | :------------------------------------------------------------------ |
| `address[]` | The timestamp with the end of the distribution, in unix time format |

### getRewardsList

```solidity
function getRewardsList() external view override returns (address[] memory)
```

Returns the list of available reward addresses.

#### Return Values:

| Type        | Description                                    |
| :---------- | :--------------------------------------------- |
| `address[]` | The list of rewards supported in this contract |

### getUserAssetIndex

```solidity
function getUserAssetIndex(address user, address asset, address reward) public view override returns (uint256)
```

Returns the index of a user on a reward distribution.

#### Input Parameters:

| Name   | Type      | Description                                |
| :----- | :-------- | :----------------------------------------- |
| user   | `address` | The address of the user                    |
| asser  | `address` | The incentivized asset                     |
| reward | `address` | The reward token of the incentivized asset |

#### Return Values:

| Type      | Description                                                   |
| :-------- | :------------------------------------------------------------ |
| `uint256` | The current user asset index, not including new distributions |

### getUserAccruedRewards

```solidity
function getUserAccruedRewards(address user, address reward) external view override returns (uint256)
```

Returns the accrued rewards balance of a user, not including virtually accrued rewards since last distribution.

#### Input Parameters:

| Name   | Type      | Description                     |
| :----- | :-------- | :------------------------------ |
| user   | `address` | The address of the user         |
| reward | `address` | The address of the reward token |

#### Return Values:

| Type      | Description                                        |
| :-------- | :------------------------------------------------- |
| `uint256` | Unclaimed rewards, not including new distributions |

### getUserRewards

```solidity
function getUserRewards(
    address[] calldata assets,
    address user,
    address reward
) external view override returns (uint256)
```

Returns a single rewards balance of a user, including virtually accrued and unrealized claimable rewards.

#### Input Parameters:

| Name   | Type        | Description                                                     |
| :----- | :---------- | :-------------------------------------------------------------- |
| assets | `address[]` | The list of incentivized assets to check eligible distributions |
| user   | `address`   | The address of the user                                         |
| reward | `address`   | The address of the reward token                                 |

#### Return Values:

| Type      | Description        |
| :-------- | :----------------- |
| `uint256` | The rewards amount |

### getAllUserRewards

```solidity
function getAllUserRewards(address[] calldata assets, address user)
    external
    view
    override
    returns (address[] memory rewardsList, uint256[] memory unclaimedAmounts)
```

Returns a list all rewards of a user, including already accrued and unrealized claimable rewards.

#### Input Parameters:

| Name   | Type        | Description                                                     |
| :----- | :---------- | :-------------------------------------------------------------- |
| assets | `address[]` | The list of incentivized assets to check eligible distributions |
| user   | `address`   | The address of the user                                         |

#### Return Values:

| Name             | Type        | Description                             |
| :--------------- | :---------- | :-------------------------------------- |
| rewardsList      | `address[]` | The list of reward addresses            |
| unclaimedAmounts | `uint256`   | The list of unclaimed amount of rewards |

### getAssetDecimals

```solidity
function getAssetDecimals(address asset) external view returns (uint8)
```

Returns the decimals of an asset to calculate the distribution delta.

#### Input Parameters:

| Name   | Type      | Description                      |
| :----- | :-------- | :------------------------------- |
| assets | `address` | The address to retrieve decimals |

#### Return Values:

| Type    | Description                         |
| :------ | :---------------------------------- |
| `uint8` | The decimals of an underlying asset |

### getEmissionManager

```solidity
function getEmissionManager() external view returns (address)
```

Returns the address of the emission manager.

#### Return Values:

| Type      | Description                        |
| :-------- | :--------------------------------- |
| `address` | The address of the EmissionManager |

### setEmissionManager

```solidity
function setEmissionManager(address emissionManager) external onlyEmissionManager
```

Updates the address of the emission manager.

#### Input Parameters:

| Name            | Type      | Description                             |
| :-------------- | :-------- | :-------------------------------------- |
| emissionManager | `address` |  The address of the new EmissionManager |

## ABI
<details>
<summary>RewardsDistributor ABI</summary>

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
    }
]
```
</details>