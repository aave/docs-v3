# RewardsController

## RewardsController

All rewards type enabled in Aave V3 are managed by [RewardsDistributor.sol](https://github.com/aave/aave-v3-periphery/blob/master/contracts/rewards/RewardsDistributor.sol). This is the contract used to check rewards data, user’s rewards balance and for claiming the rewards.

## Structs

### AssetData

| Name             | Type                          |
| ---------------- | ----------------------------- |
| rewards          | mapping(address ⇒ RewardData) |
| availableRewards | address\[]                    |
| decimals         | uint8                         |

### RewardsData

| Name                | Type                        |
| ------------------- | --------------------------- |
| emissionPerSecond   | uint88                      |
| index               | uint104                     |
| lastUpdateTimestamp | uint32                      |
| distributionEnd     | uint32                      |
| usersIndex          | mapping(address => uint256) |

## View Methods

### getRewardsData

`getRewardsData (asset, reward)`

Get the data of the reward emitted for the asset.

**Call Params**

| Name   | Type    | Description                                                              |
| ------ | ------- | ------------------------------------------------------------------------ |
| asset  | address | address of the a/s/v Tokens for which incentive information is requested |
| reward | address | address of the reward token                                              |

**Return Value**

| Type    | Description                                                     |
| ------- | --------------------------------------------------------------- |
| uint104 | index of the reward token                                       |
| uint88  | total reward tokens awarded per second for the given asset pool |
| uint32  | unix timestamp of last time the emissions were updated          |
| uint32  | unix timestamp of when the emissions will end                   |

### getRewardsByAsset

`getRewardsByAsset (asset)`

Get list of rewards activated for the asset

Call Params

| Name  | Type    | Description                                                              |
| ----- | ------- | ------------------------------------------------------------------------ |
| asset | address | address of the a/s/vTokens for which incentive rewards list is requested |

Return Value

| Type       | Description                                                  |
| ---------- | ------------------------------------------------------------ |
| address\[] | list of reward token addresses activated for the given asset |

## Write Methods

### claimRewards

`claimRewards (assets, amount, to, reward)`

Claims single reward type specified by `reward` for the list of assets. Rewards are received by `to` address.

Call Params

| Name   | Type       | Description                                                                                |
| ------ | ---------- | ------------------------------------------------------------------------------------------ |
| assets | address\[] | address list of assets for which rewards are being claimed. Pass a/s/vToken addresses      |
| amount | uint256    | amount to claim, expressed in wei. Pass MAX\_UINT to claim entire unclaimed reward balance |
| to     | address    | address which will receive the reward tokens                                               |
| reward | address    | address of the reward token being claimed. eg. stkAaave                                    |

### claimRewardsOnBehalf

`claimRewardsOnBehalfOf (assets, amount, user, to, reward)`

Claims single reward type specified by `reward` for the given list of assets on behalf of the `user`. Rewards are received by `to` address.

{% hint style="info" %}
The `msg.sender` must be an authorised claimer set using `setClaimer()` method, via Governance Vote.
{% endhint %}

Call Params

| Name   | Type       | Description                                                                                |
| ------ | ---------- | ------------------------------------------------------------------------------------------ |
| assets | address\[] | address list of assets for which rewards are being claimed. Pass a/s/vToken addresses      |
| amount | uint256    | amount to claim, expressed in wei. Pass MAX\_UINT to claim entire unclaimed reward balance |
| user   | address    | address of user who’s rewards are being claimed                                            |
| to     | address    | address which will receive the reward tokens                                               |
| reward | address    | address of the reward token being claimed. eg. stkAaave                                    |

### claimRewardsToSelf

`claimRewardsToSelf (assets, amount, reward)`

Claims single reward type accrued by the `msg.sender` specified by `reward` for the given list of assets. Rewards are received by `msg.sender` .

Call Params

| Name   | Type       | Description                                                                                |
| ------ | ---------- | ------------------------------------------------------------------------------------------ |
| assets | address\[] | address list of assets for which rewards are being claimed. Pass a/s/vToken addresses      |
| amount | uint256    | amount to claim, expressed in wei. Pass MAX\_UINT to claim entire unclaimed reward balance |
| reward | address    | address of the reward token being claimed. eg. stkAaave                                    |

### claimAllRewards

`claimAllRewards (assets, to)`

Claims all rewards for the list of assets. Rewards are received by `to` address.

Call Params

| Name   | Type       | Description                                                                           |
| ------ | ---------- | ------------------------------------------------------------------------------------- |
| assets | address\[] | address list of assets for which rewards are being claimed. Pass a/s/vToken addresses |
| to     | address    | address which will receive the reward tokens                                          |

### claimAllRewardsOnBehalf

`claimAllRewardsOnBehalfOf (assets, user, to)`

Claims all rewards for the given list of assets on behalf of the `user`. Rewards are received by `to` address.

{% hint style="info" %}
The `msg.sender` must be an authorised claimer set using `setClaimer()` method, via Governance Vote.
{% endhint %}

Call Params

| Name   | Type       | Description                                                                           |
| ------ | ---------- | ------------------------------------------------------------------------------------- |
| assets | address\[] | address list of assets for which rewards are being claimed. Pass a/s/vToken addresses |
| user   | address    | address of user who’s rewards are being claimed                                       |
| reward | address    | address of the reward token being claimed. eg. stkAaave                               |

### claimAllRewardsToSelf

`claimAllRewardsToSelf (assets)`

Claims all rewards accrued by `msg.sender` for the given list of assets. Rewards are received by `msg.sender` .

Call Params

| Name   | Type       | Description                                                                           |
| ------ | ---------- | ------------------------------------------------------------------------------------- |
| assets | address\[] | address list of assets for which rewards are being claimed. Pass a/s/vToken addresses |
