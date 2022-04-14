# UiIncentiveDataProviderV3

## UiIncentiveDataProviderV3

Contract used by Aave UI to collect and pre-process incentives data.

## Data Structures

#### AggregatedReserveIncentiveData

| Name            | Type          | Description                                                                                               |
| --------------- | ------------- | --------------------------------------------------------------------------------------------------------- |
| underlyingAsset | address       | Address of the asset supplied/borrowed in Pool                                                            |
| aIncentiveData  | IncentiveData | Details of rewards distributed for supplying to Aave Pool i.e. rewards for aToken holders.                |
| vIncentiveData  | IncentiveData | Details of rewards distributed for variable debt borrowed from Aave Pool i.e. rewards for vToken holders. |
| sIncentiveData  | IncentiveData | Details of rewards distributed for stable debt borrowed from Aave Pool i.e. rewards for sToken holders.   |

#### IncentiveData

| Name                       | Type          | Description                                                                    |
| -------------------------- | ------------- | ------------------------------------------------------------------------------ |
| tokenAddress               | address       | Address of corresponding a/s/vToken.                                           |
| incentiveControllerAddress | address       | Address of Rewards Controller                                                  |
| rewardsTokenInformation    | RewardInfo\[] | Array of details for all reward tokens that are available for given a/s/vToken |

#### RewardInfo

| Name                          | Type    | Description                                                                                          |
| ----------------------------- | ------- | ---------------------------------------------------------------------------------------------------- |
| rewardTokenSymbol             | string  | Symbol of Reward Token                                                                               |
| rewardTokenAddress            | address | Address of Reward Token                                                                              |
| rewardOracleAddress           | address | Price Oracle for Reward token                                                                        |
| emissionPerSecond             | uint256 | Reward Token emitted per second                                                                      |
| incentivesLastUpdateTimestamp | uint256 | Unix timestamp of last update made on asset’s reward token.                                          |
| tokenIncentivesIndex          | uint256 | Latest distribution index of the reward token                                                        |
| emissionEndTimestamp          | uint256 | Unix timestamp of when the Incentive emission of given reward token ends for the corresponding asset |
| rewardPriceFeed               | int256  | Latest answer/price from reward token price oracle                                                   |
| rewardTokenDecimals           | uint8   | Decimals of reward token                                                                             |
| precision                     | uint8   | Decimals of asset token (a/s/vToken)                                                                 |
| priceFeedDecimals             | uint8   | Decimals of price provided by oracle                                                                 |

#### UserReserveIncentiveData

| Name                     | Type              | Description                                                                                             |
| ------------------------ | ----------------- | ------------------------------------------------------------------------------------------------------- |
| underlyingAsset          | address           | Address of the asset supplied/borrowed in Pool                                                          |
| aTokenIncentivesUserData | UserIncentiveData | Details of user rewards received for supplying to Aave Pool i.e. rewards for aToken.                    |
| vTokenIncentivesUserData | UserIncentiveData | Details of user rewards received for borrowing at variable rate from Aave Pool i.e. rewards for vToken. |
| sTokenIncentivesUserData | UserIncentiveData | Details of user rewards received for borrowing at stable rate from Aave Pool i.e. rewards for sToken.   |

#### UserIncentiveData

| Name                       | Type              | Description                                                                         |
| -------------------------- | ----------------- | ----------------------------------------------------------------------------------- |
| tokenAddress               | address           | Address of corresponding a/s/vToken.                                                |
| incentiveControllerAddress | address           | Address of Rewards Controller for reward claim tx                                   |
| userRewardsInformation     | UserRewardInfo\[] | Array of details for all reward tokens accrued/claimed by user for given a/s/vToken |

#### UserRewardInfo

| Name                     | Type    | Description                                        |
| ------------------------ | ------- | -------------------------------------------------- |
| rewardTokenSymbol        | string  | Symbol of Reward Token                             |
| rewardOracleAddress      | address | Price Oracle for Reward token                      |
| rewardTokenAddress       | address | Address of Reward Token                            |
| userUnclaimedRewards     | uint256 | User’s unclaimed rewards                           |
| tokenIncentivesUserIndex | uint256 | Latest user distribution index                     |
| rewardPriceFeed          | int256  | Latest answer/price from reward token price oracle |
| priceFeedDecimals        | uint8   | Decimals of price provided by oracle               |
| rewardTokenDecimals      | uint8   | Decimals of reward token                           |

## Methods

#### getReservesIncentivesData

`function getReservesIncentivesData(IPoolAddressesProvider provider)`

Returns `AggregatedReserveIncentiveData[]` for the pool associated with given [`provider`](../core-contracts/pooladdressesprovider.md).

#### getUserReservesIncentivesData

`function getUserReservesIncentivesData(IPoolAddressesProvider provider, address user)`

Returns `UserReserveIncentiveData[]` for the given `user` for the pool associated with given .

#### getFullReservesIncentiveData

`function getFullReservesIncentiveData(IPoolAddressesProvider provider, address user)`

Returns both `AggregatedReserveIncentiveData[]` and `UserReserveIncentiveData[]` for the given `user` for the pool associated with given [`provider`](../core-contracts/pooladdressesprovider.md).
