# Arbitrum

## Core & Periphery contracts

{% hint style="warning" %}
_Pool_, _PoolConfigurator_, _Incentives_ and _Treasury_ addresses mentioned below are of Upgradeable Proxy contract. While interacting please use _abi_ of implementation contracts or generate abi from the github source code linked.&#x20;
{% endhint %}

| Contract                      | Github                                                                                                                                | Address                                    |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------ |
| ACLManager                    | [Github](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/configuration/ACLManager.sol)                            | 0xa72636CbcAa8F5FF95B2cc47F3CDEe83F3294a0B |
| Pool                          | [Github](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/pool/L2Pool.sol)                                         | 0x794a61358D6845594F94dc1DB02A252b5b4814aD |
| PoolConfigurator              | [Github](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/pool/PoolConfigurator.sol)                               | 0x8145eddDf43f50276641b55bd3AD95944510021E |
| Incentives                    | [Github](https://github.com/aave/aave-v3-periphery/blob/master/contracts/rewards/RewardsController.sol)                               | 0x929EC64c34a17401F460460D4B9390518E5B473e |
| PullRewardsTransferStrategy   | [Github](https://github.com/aave/aave-v3-periphery/blob/master/contracts/rewards/transfer-strategies/PullRewardsTransferStrategy.sol) | 0x2bB52F7779fA2A77Be64E199c18BD6437801caac |
| PoolAddressesProvider         | [Github](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/configuration/PoolAddressesProvider.sol)                 | 0xa97684ead0e402dC232d5A977953DF7ECBaB3CDb |
| PoolAddressesProviderRegistry | [Github](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/configuration/PoolAddressesProviderRegistry.sol)         | 0x770ef9f4fe897e59daCc474EF11238303F9552b6 |
| PoolDataProvider              | [Github](https://github.com/aave/aave-v3-core/blob/master/contracts/misc/AaveProtocolDataProvider.sol)                                | 0x69FA688f1Dc47d4B5d8029D5a35FB7a548310654 |
| L2Encoder                     | [Github](https://github.com/aave/aave-v3-core/blob/master/contracts/misc/L2Encoder.sol)                                               | 0x9abADECD08572e0eA5aF4d47A9C7984a5AA503dC |
| UiIncentiveDataProviderV3     | [Github](https://github.com/aave/aave-v3-periphery/blob/master/contracts/misc/UiIncentiveDataProviderV3.sol)                          | 0xEFdd7374551897B11a23Ec7b5694C713DFDa76f1 |
| UiPoolDataProviderV3          | [Github](https://github.com/aave/aave-v3-periphery/blob/master/contracts/misc/UiPoolDataProviderV3.sol)                               | 0x3f960bB91e85Ae2dB561BDd01B515C5A5c65802b |
| WETHGateway                   | [Github](https://github.com/aave/aave-v3-periphery/blob/master/contracts/misc/WETHGateway.sol)                                        | 0xC09e69E79106861dF5d289dA88349f10e2dc6b5C |
| WalletBalanceProvider         | [Github](https://github.com/aave/aave-v3-periphery/blob/master/contracts/misc/WalletBalanceProvider.sol)                              | 0xBc790382B3686abffE4be14A030A96aC6154023a |
| AaveOracle                    | [Github](https://github.com/aave/aave-v3-core/blob/master/contracts/misc/AaveOracle.sol)                                              | 0xb56c2F0B653B2e0b10C9b928C8580Ac5Df02C7C7 |
| Treasury                      | [Github](https://github.com/aave/aave-v3-periphery/blob/master/contracts/treasury/Collector.sol)                                      | 0x053D55f9B5AF8694c503EB288a1B7E552f590710 |
| TreasuryController            | [Github](https://github.com/aave/aave-v3-periphery/blob/master/contracts/treasury/CollectorController.sol)                            | 0xC3301b30f4EcBfd59dE0d74e89690C1a70C6f21B |
| FallbackOracle                | [Github](https://github.com/aave/aave-v3-core/blob/master/contracts/mocks/oracle/PriceOracle.sol)                                     | 0x8aA68Ca9e25aAb6f9f41bF341d12Ab407AE099E2 |

## Tokens

| Token                                 | Address                                    |
| ------------------------------------- | ------------------------------------------ |
| DAI-AToken-Arbitrum (aDAI)            | 0x82E64f49Ed5EC1bC6e43DAD4FC8Af9bb3A2312EE |
| DAI-StableDebtToken-Arbitrum (sDAI)   | 0xd94112B5B62d53C9402e7A60289c6810dEF1dC9B |
| DAI-VariableDebtToken-Arbitrum (vDAI) | 0x8619d80FB0141ba7F184CbF22fd724116D9f7ffC |
| LINK-AToken-Arbitrum (aLINK)          | 0x191c10Aa4AF7C30e871E70C95dB0E4eb77237530 |
| LINK-StableDebtToken-Arbitrum         | 0x89D976629b7055ff1ca02b927BA3e020F22A44e4 |
| LINK-VariableDebtToken-Arbitrum       | 0x953A573793604aF8d41F306FEb8274190dB4aE0e |
| USDC-AToken-Arbitrum                  | 0x625E7708f30cA75bfd92586e17077590C60eb4cD |
| USDC-StableDebtToken-Arbitrum         | 0x307ffe186F84a3bc2613D1eA417A5737D69A7007 |
| USDC-VariableDebtToken-Arbitrum       | 0xFCCf3cAbbe80101232d343252614b6A3eE81C989 |
| WBTC-AToken-Arbitrum                  | 0x078f358208685046a11C85e8ad32895DED33A249 |
| WBTC-StableDebtToken-Arbitrum         | 0x633b207Dd676331c413D4C013a6294B0FE47cD0e |
| WBTC-VariableDebtToken-Arbitrum       | 0x92b42c66840C7AD907b4BF74879FF3eF7c529473 |
| WETH-AToken-Arbitrum                  | 0xe50fA9b3c56FfB159cB0FCA61F5c9D750e8128c8 |
| WETH-StableDebtToken-Arbitrum         | 0xD8Ad37849950903571df17049516a5CD4cbE55F6 |
| WETH-VariableDebtToken-Arbitrum       | 0x0c84331e39d6658Cd6e6b9ba04736cC4c4734351 |
| USDT-AToken-Arbitrum                  | 0x6ab707Aca953eDAeFBc4fD23bA73294241490620 |
| USDT-StableDebtToken-Arbitrum         | 0x70eFfc565DB6EEf7B927610155602d31b670e802 |
| USDT-VariableDebtToken-Arbitrum       | 0xfb00AC187a8Eb5AFAE4eACE434F493Eb62672df7 |
| AAVE-AToken-Arbitrum                  | 0xf329e36C7bF6E5E86ce2150875a84Ce77f477375 |
| AAVE-StableDebtToken-Arbitrum         | 0xfAeF6A702D15428E588d4C0614AEFb4348D83D48 |
| AAVE-VariableDebtToken-Arbitrum       | 0xE80761Ea617F66F96274eA5e8c37f03960ecC679 |
| EURS-AToken-Arbitrum                  | 0x6d80113e533a2C0fe82EaBD35f1875DcEA89Ea97 |
| EURS-StableDebtToken-Arbitrum         | 0xF15F26710c827DDe8ACBA678682F3Ce24f2Fb56E |
| EURS-VariableDebtToken-Arbitrum       | 0x4a1c3aD6Ed28a636ee1751C69071f6be75DEb8B8 |
