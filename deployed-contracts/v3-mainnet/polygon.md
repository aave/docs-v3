# Polygon

## Core & Periphery contracts

{% hint style="warning" %}
_Pool_, _PoolConfigurator_, _Incentives_ and _Treasury_ addresses mentioned below are of Upgradeable Proxy contract. While interacting please use _abi_ of implementation contracts or generate abi from the github source code linked.&#x20;
{% endhint %}

| Contract                      | Github                                                                                                                                | Address                                    |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------ |
| ACLManager                    | [Github](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/configuration/ACLManager.sol)                            | 0xa72636CbcAa8F5FF95B2cc47F3CDEe83F3294a0B |
| Pool                          | [Github](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/pool/Pool.sol)                                           | 0x794a61358D6845594F94dc1DB02A252b5b4814aD |
| PoolConfigurator              | [Github](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/pool/PoolConfigurator.sol)                               | 0x8145eddDf43f50276641b55bd3AD95944510021E |
| Incentives                    | [Github](https://github.com/aave/aave-v3-periphery/blob/master/contracts/rewards/RewardsController.sol)                               | 0x929EC64c34a17401F460460D4B9390518E5B473e |
| PullRewardsTransferStrategy   | [Github](https://github.com/aave/aave-v3-periphery/blob/master/contracts/rewards/transfer-strategies/PullRewardsTransferStrategy.sol) | 0xA1e255c216e0F7f1a54AeeA60BACB93CdE0FcF0D |
| PoolAddressesProvider         | [Github](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/configuration/PoolAddressesProvider.sol)                 | [0xa97684ead0e402dC232d5A977953DF7ECBaB3CDb](https://polygonscan.com/address/0xa97684ead0e402dC232d5A977953DF7ECBaB3CDb#code) |
| PoolAddressesProviderRegistry | [Github](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/configuration/PoolAddressesProviderRegistry.sol)         | 0x770ef9f4fe897e59daCc474EF11238303F9552b6 |
| PoolDataProvider              | [Github](https://github.com/aave/aave-v3-core/blob/master/contracts/misc/AaveProtocolDataProvider.sol)                                | 0x69FA688f1Dc47d4B5d8029D5a35FB7a548310654 |
| UiIncentiveDataProviderV3     | [Github](https://github.com/aave/aave-v3-periphery/blob/master/contracts/misc/UiIncentiveDataProviderV3.sol)                          | 0x05E309C97317d8abc0f7e78185FC966FfbD2CEC0 |
| UiPoolDataProviderV3          | [Github](https://github.com/aave/aave-v3-periphery/blob/master/contracts/misc/UiPoolDataProviderV3.sol)                               | 0x8F1AD487C9413d7e81aB5B4E88B024Ae3b5637D0 |
| WETHGateway                   | [Github](https://github.com/aave/aave-v3-periphery/blob/master/contracts/misc/WETHGateway.sol)                                        | 0x9BdB5fcc80A49640c7872ac089Cc0e00A98451B6 |
| WalletBalanceProvider         | [Github](https://github.com/aave/aave-v3-periphery/blob/master/contracts/misc/WalletBalanceProvider.sol)                              | 0xBc790382B3686abffE4be14A030A96aC6154023a |
| AaveOracle                    | [Github](https://github.com/aave/aave-v3-core/blob/master/contracts/misc/AaveOracle.sol)                                              | 0xb023e699F5a33916Ea823A16485e259257cA8Bd1 |
| Treasury                      | [Github](https://github.com/aave/aave-v3-periphery/blob/master/contracts/treasury/Collector.sol)                                      | 0xe8599F3cc5D38a9aD6F3684cd5CEa72f10Dbc383 |
| TreasuryController            | [Github](https://github.com/aave/aave-v3-periphery/blob/master/contracts/treasury/CollectorController.sol)                            | 0x73D435AFc15e35A9aC63B2a81B5AA54f974eadFe |
| ParaSwapLiquiditySwapAdapter  | [Github](https://github.com/aave/aave-v3-periphery/blob/master/contracts/adapters/paraswap/ParaSwapLiquiditySwapAdapter.sol)          | 0x00d48554f570B6f1c474EBe56116159c3B1D625f |
| ParaSwapRepayAdapter          | [Github](https://github.com/aave/aave-v3-periphery/blob/master/contracts/adapters/paraswap/ParaSwapRepayAdapter.sol)                  | 0xD0E8f168d297DfA0f3EE1711c538BcC0663320aF |
| FallbackOracle                | [Github](https://github.com/aave/aave-v3-core/blob/master/contracts/mocks/oracle/PriceOracle.sol)                                     | 0xaA5890362f36FeaAe91aF248e84e287cE6eCD1A9 |

## Tokens

| Token                                | Address                                    |
| ------------------------------------ | ------------------------------------------ |
| DAI-AToken-Polygon (aDAI)            | 0x82E64f49Ed5EC1bC6e43DAD4FC8Af9bb3A2312EE |
| DAI-StableDebtToken-Polygon (sDAI)   | 0xd94112B5B62d53C9402e7A60289c6810dEF1dC9B |
| DAI-VariableDebtToken-Polygon (vDAI) | 0x8619d80FB0141ba7F184CbF22fd724116D9f7ffC |
| LINK-AToken-Polygon (aLINK)          | 0x191c10Aa4AF7C30e871E70C95dB0E4eb77237530 |
| LINK-StableDebtToken-Polygon         | 0x89D976629b7055ff1ca02b927BA3e020F22A44e4 |
| LINK-VariableDebtToken-Polygon       | 0x953A573793604aF8d41F306FEb8274190dB4aE0e |
| USDC-AToken-Polygon                  | 0x625E7708f30cA75bfd92586e17077590C60eb4cD |
| USDC-StableDebtToken-Polygon         | 0x307ffe186F84a3bc2613D1eA417A5737D69A7007 |
| USDC-VariableDebtToken-Polygon       | 0xFCCf3cAbbe80101232d343252614b6A3eE81C989 |
| WBTC-AToken-Polygon                  | 0x078f358208685046a11C85e8ad32895DED33A249 |
| WBTC-StableDebtToken-Polygon         | 0x633b207Dd676331c413D4C013a6294B0FE47cD0e |
| WBTC-VariableDebtToken-Polygon       | 0x92b42c66840C7AD907b4BF74879FF3eF7c529473 |
| WETH-AToken-Polygon                  | 0xe50fA9b3c56FfB159cB0FCA61F5c9D750e8128c8 |
| WETH-StableDebtToken-Polygon         | 0xD8Ad37849950903571df17049516a5CD4cbE55F6 |
| WETH-VariableDebtToken-Polygon       | 0x0c84331e39d6658Cd6e6b9ba04736cC4c4734351 |
| USDT-AToken-Polygon                  | 0x6ab707Aca953eDAeFBc4fD23bA73294241490620 |
| USDT-StableDebtToken-Polygon         | 0x70eFfc565DB6EEf7B927610155602d31b670e802 |
| USDT-VariableDebtToken-Polygon       | 0xfb00AC187a8Eb5AFAE4eACE434F493Eb62672df7 |
| AAVE-AToken-Polygon                  | 0xf329e36C7bF6E5E86ce2150875a84Ce77f477375 |
| AAVE-StableDebtToken-Polygon         | 0xfAeF6A702D15428E588d4C0614AEFb4348D83D48 |
| AAVE-VariableDebtToken-Polygon       | 0xE80761Ea617F66F96274eA5e8c37f03960ecC679 |
| EURS-AToken-Polygon                  | 0x38d693cE1dF5AaDF7bC62595A37D667aD57922e5 |
| EURS-StableDebtToken-Polygon         | 0x8a9FdE6925a839F6B1932d16B36aC026F8d3FbdB |
| EURS-VariableDebtToken-Polygon       | 0x5D557B07776D12967914379C71a1310e917C7555 |
| WMATIC-AToken-Polygon                | 0x6d80113e533a2C0fe82EaBD35f1875DcEA89Ea97 |
| WMATIC-StableDebtToken-Polygon       | 0xF15F26710c827DDe8ACBA678682F3Ce24f2Fb56E |
| WMATIC-VariableDebtToken-Polygon     | 0x4a1c3aD6Ed28a636ee1751C69071f6be75DEb8B8 |
| GHST-AToken-Polygon                  | 0x8Eb270e296023E9D92081fdF967dDd7878724424 |
| GHST-StableDebtToken-Polygon         | 0x3EF10DFf4928279c004308EbADc4Db8B7620d6fc |
| GHST-VariableDebtToken-Polygon       | 0xCE186F6Cccb0c955445bb9d10C59caE488Fea559 |
| BAL-AToken-Polygon                   | 0x8ffDf2DE812095b1D19CB146E4c004587C0A0692 |
| BAL-StableDebtToken-Polygon          | 0xa5e408678469d23efDB7694b1B0A85BB0669e8bd |
| BAL-VariableDebtToken-Polygon        | 0xA8669021776Bc142DfcA87c21b4A52595bCbB40a |
| CRV-AToken-Polygon                   | 0x513c7E3a9c69cA3e22550eF58AC1C0088e918FFf |
| CRV-StableDebtToken-Polygon          | 0x08Cb71192985E936C7Cd166A8b268035e400c3c3 |
| CRV-VariableDebtToken-Polygon        | 0x77CA01483f379E58174739308945f044e1a764dc |
| DPI-AToken-Polygon                   | 0x724dc807b04555b71ed48a6896b6F41593b8C637 |
| DPI-StableDebtToken-Polygon          | 0xDC1fad70953Bb3918592b6fCc374fe05F5811B6a |
| DPI-VariableDebtToken-Polygon        | 0xf611aEb5013fD2c0511c9CD55c7dc5C1140741A6 |
| SUSHI-AToken-Polygon                 | 0xc45A479877e1e9Dfe9FcD4056c699575a1045dAA |
| SUSHI-StableDebtToken-Polygon        | 0x78246294a4c6fBf614Ed73CcC9F8b875ca8eE841 |
| SUSHI-VariableDebtToken-Polygon      | 0x34e2eD44EF7466D5f9E0b782B5c08b57475e7907 |
| JEUR-AToken-Polygon                  | 0x6533afac2E7BCCB20dca161449A13A32D391fb00 |
| JEUR-StableDebtToken-Polygon         | 0x6B4b37618D85Db2a7b469983C888040F7F05Ea3D |
| JEUR-VariableDebtToken-Polygon       | 0x44705f578135cC5d703b4c9c122528C73Eb87145 |
| AGEUR-AToken-Polygon                 | 0x8437d7C167dFB82ED4Cb79CD44B7a32A1dd95c77 |
| AGEUR-StableDebtToken-Polygon        | 0x40B4BAEcc69B882e8804f9286b12228C27F8c9BF |
| AGEUR-VariableDebtToken-Polygon      | 0x3ca5FA07689F266e907439aFd1fBB59c44fe12f6 |
