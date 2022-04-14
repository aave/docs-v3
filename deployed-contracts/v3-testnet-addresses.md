# V3 Testnet Addresses

Official Aave V3 Testnet Release is on following chains:

* Ethereum - Rinkeby, Kovan
* Arbitrum
* Optimism
* Avalanche
* Fantom
* Polygon
* Harmony

Contract name changes from V2 -> V3:

* LendingPool -> Pool
* LendingPoolAddressesProvider -> PoolAddressesProvider
* ProtocolDataProvider -> PoolDataProvider

{% hint style="warning" %}
For assets on testnets, we use different versions of the token (e.g. testnet Dai) which are mintableERC20. This is to ensure enough liquidity for our reserves and to easily mint more tokens when needed.
{% endhint %}

\
💡  Click through the tabs to get addresses of the deployed contracts on various chains.

{% tabs %}
{% tab title="Rinkeby" %}
```
========
┌─────────┬──────────────────────────────────┬──────────────────────────────────────────────┬────────────────────────┐
│ (index) │               name               │                   account                    │        balance         │
├─────────┼──────────────────────────────────┼──────────────────────────────────────────────┼────────────────────────┤
│    0    │            'deployer'            │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '9.53205127725099007'  │
│    1    │            'aclAdmin'            │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '9.53205127725099007'  │
│    2    │         'emergencyAdmin'         │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '9.53205127725099007'  │
│    3    │           'poolAdmin'            │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '9.53205127725099007'  │
│    4    │ 'addressesProviderRegistryOwner' │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '9.53205127725099007'  │
│    5    │       'treasuryProxyAdmin'       │ '0x04c94825C3e3539e0f2bB21d435302d08B2Dbd77' │ '0.098622566595694224' │
│    6    │      'incentivesProxyAdmin'      │ '0x04c94825C3e3539e0f2bB21d435302d08B2Dbd77' │ '0.098622566595694224' │
│    7    │   'incentivesEmissionManager'    │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '9.53205127725099007'  │
│    8    │     'incentivesRewardsVault'     │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '9.53205127725099007'  │
└─────────┴──────────────────────────────────┴──────────────────────────────────────────────┴────────────────────────┘

Deployments
===========
┌─────────────────────────────────────────┬──────────────────────────────────────────────┐
│                 (index)                 │                   address                    │
├─────────────────────────────────────────┼──────────────────────────────────────────────┤
│               BorrowLogic               │ '0xd52699A7a785c758AA6e4dEB89AE8Ca0245aAD0A' │
│               BridgeLogic               │ '0x507EdfcB669a30BbEC4b2f5930FB992a120DF306' │
│            ConfiguratorLogic            │ '0x1fCB8CdeD02E3e294663EB913B3C8FcB75033f6A' │
│               EModeLogic                │ '0x6bEda4d300945b461C78fEE9771C5cB8B9cfF00e' │
│            LiquidationLogic             │ '0xBF2b3c17d297B30F2A20599F056Fc905E46A5a91' │
│      PoolAddressesProviderRegistry      │ '0xF2038a65f68a94d1CFD0166f087A795341e2eac8' │
│               SupplyLogic               │ '0xca2413028D0c91f5F88821A13d4A82690945F678' │
│             FlashLoanLogic              │ '0xd237aDd251323Cd757fFCB4Ae3e36d31a603163A' │
│                PoolLogic                │ '0xB0782094001F15Fd0EA8e573D72b488b06574649' │
│              TreasuryProxy              │ '0xD1D7347DC861A86Cb5ba39fdD7f8335aCa7bD81A' │
│           Treasury-Controller           │ '0x45F1e6D6F206bBe33d495Ef94F64C1CF6e50D1BE' │
│         Treasury-Implementation         │ '0x3eF99001FB402A8853ca1a06C37554255B4Ce5d9' │
│               WETHGateway               │ '0xD1DECc6502cc690Bc85fAf618Da487d886E54Abe' │
│          WalletBalanceProvider          │ '0x116674C3Efe4e31F192d855284619DEd6fE2a1b9' │
│        UiIncentiveDataProviderV3        │ '0x2c9f31b1F9838Bb8781bb61a0d0a4615f6530207' │
│          UiPoolDataProviderV3           │ '0x550f9764d56291B5B793b6dD1623af3346128BD2' │
│            ERC20Faucet-Aave             │ '0x88138CA1e9E485A1E688b030F85Bb79d63f156BA' │
│       PoolAddressesProvider-Aave        │ '0xBA6378f1c1D046e9EB0F538560BA7558546edF3C' │
│          PoolDataProvider-Aave          │ '0xBAB2E7afF5acea53a43aEeBa2BA6298D8056DcE5' │
│    WETH-TestnetPriceAggregator-Aave     │ '0xa9731A16797a9d406E3F3EA31078061444a2CcF3' │
│     DAI-TestnetPriceAggregator-Aave     │ '0x69Cf8fF6A12D2817Ec5b296556E37D816a86EABD' │
│    LINK-TestnetPriceAggregator-Aave     │ '0x6aEc12D18b9702778227F7EC09f2e640BEEBed3d' │
│    USDC-TestnetPriceAggregator-Aave     │ '0xC7fEC323B14aA86597933ada3583b118a893B786' │
│    WBTC-TestnetPriceAggregator-Aave     │ '0x687cC036dB24eAC4Cc3C39Da94D0ca241b1055fc' │
│    USDT-TestnetPriceAggregator-Aave     │ '0x842cC57d976cE198EE537e2E247F88eD8e3dC8AE' │
│    AAVE-TestnetPriceAggregator-Aave     │ '0xd4a65cBe3A924769dE047a7115Ca859E888FddC4' │
│    EURS-TestnetPriceAggregator-Aave     │ '0x03422c68e48790B4465f2d856596d808bE3b75Ea' │
│           Pool-Implementation           │ '0x87530ED4bd0ee0e79661D65f8Dd37538F693afD5' │
│     PoolConfigurator-Implementation     │ '0x4D7D84cd3A63f61183363F4655e64C0D1E3012d1' │
│           ReservesSetupHelper           │ '0x3Bbac251F403026ddaE95e4a2352A622C9F340C5' │
│             ACLManager-Aave             │ '0x74E3445f239f9915D57715Efb810f67b2a7E5758' │
│             AaveOracle-Aave             │ '0xA323726989db5708B19EAd4A494dDe09F3cEcc69' │
│           FallbackOracle-Aave           │ '0xb7F15f789e93D228c62e68807f3153da3aA4EdC8' │
│             Pool-Proxy-Aave             │ '0xE039BdF1d874d27338e09B55CB09879Dedca52D8' │
│       PoolConfigurator-Proxy-Aave       │ '0x11E9F019FCC15AccB472Aa49C8fc0c61949c86d5' │
│             IncentivesProxy             │ '0x17e7097C6Db59B13Da3f894A28946a3ec23502E0' │
│       IncentivesV2-Implementation       │ '0x73e5a2a7F95AD2BDF9f79B50a611B36905fA7a33' │
│       PullRewardsTransferStrategy       │ '0x1aDe1619d4448D05812e0F53E7bA6A7D88d3D5C2' │
│               AToken-Aave               │ '0xF7844Dc72031Bd9E6aCE18b09509792401D8c9c5' │
│       DelegationAwareAToken-Aave        │ '0xB59D25Bb725989272E62dbe8FccaD30B31CF7235' │
│          StableDebtToken-Aave           │ '0x252336a2eeeaC16e7c1B318bd277A28da3593a5d' │
│         VariableDebtToken-Aave          │ '0x09A1BFBDF142138259D43e469b0741C85Ba3b90d' │
│  ReserveStrategy-rateStrategyStableTwo  │ '0x1AFE601dE2FBFa18131EC57d610e0955515D9C1C' │
│ ReserveStrategy-rateStrategyVolatileOne │ '0xa5D6469689FaCD89ACFA08533d573e0Ac5830331' │
│  ReserveStrategy-rateStrategyStableOne  │ '0x63809848Fe0a049207fF84926Bf909f1333Fe546' │
│            WETH-AToken-Aave             │ '0x608D11E704baFb68CfEB154bF7Fd641120e33aD4' │
│       WETH-VariableDebtToken-Aave       │ '0x252C97371c9Ad590898fcDb0C401d9230939A78F' │
│        WETH-StableDebtToken-Aave        │ '0x7666ca6911bEcBA7d38Fa2da8278b82297EC7e6F' │
│             DAI-AToken-Aave             │ '0x49866611AA7Dc30130Ac6A0DF29217D16FD87bc0' │
│       DAI-VariableDebtToken-Aave        │ '0x37768F60EfcFF96188530B022e3DE9d168c2c8E8' │
│        DAI-StableDebtToken-Aave         │ '0x0F48c09701B6D24d6D9571637758EE06eeCb9630' │
│            LINK-AToken-Aave             │ '0xeC4752053c5A693eBE6A07deF330a9F97D07FBC3' │
│       LINK-VariableDebtToken-Aave       │ '0x34c5DAeC73aE986Bf93bCf22d41e505264A86625' │
│        LINK-StableDebtToken-Aave        │ '0x4e63D3ff7Bca937FAD4e1b0e9aF4f946f2AAaE64' │
│            USDC-AToken-Aave             │ '0x50b283C17b0Fc2a36c550A57B1a133459F4391B3' │
│       USDC-VariableDebtToken-Aave       │ '0x0EfFd205184FE944f9eF80264b144270dB15eEa7' │
│        USDC-StableDebtToken-Aave        │ '0xee3D33c0C779cAD53CAa496aa5a97D026D1218Ca' │
│            WBTC-AToken-Aave             │ '0xeC1d8303b8fa33afB59012Fc3b49458B57883326' │
│       WBTC-VariableDebtToken-Aave       │ '0x3eA8e63b6e7260C2D6cfc3877914cbB6eE687D6B' │
│        WBTC-StableDebtToken-Aave        │ '0x372C35caeED54907d694DF6229319779fbC79440' │
│            USDT-AToken-Aave             │ '0x377D3F732CBeB84D0EebF71e1a4e3546Da86C76d' │
│       USDT-VariableDebtToken-Aave       │ '0x427cd2ad9Fe0B63ec26Df3aA83D4048149B3DCB3' │
│        USDT-StableDebtToken-Aave        │ '0xCC28d19D8e8A64D2Fc1709e8FE7b6139e25Fd524' │
│            AAVE-AToken-Aave             │ '0x3fc92c5f08c361EB21ef86a31d55df4b92ab7874' │
│       AAVE-VariableDebtToken-Aave       │ '0xd2693256be8c567d26D50f4B04479bD49a3aC3B5' │
│        AAVE-StableDebtToken-Aave        │ '0x951a8575A0b18A1180D5e8DD0e2e646E235b42bb' │
│            EURS-AToken-Aave             │ '0xC6B64D19EeF69071F32b043F8e57e506A86B8612' │
│       EURS-VariableDebtToken-Aave       │ '0x31e1005A6d7e48055b3BA617E4337Fb04D2C9EE0' │
│        EURS-StableDebtToken-Aave        │ '0xEa7619f4AE50C3a6ad07e8Bd029b937B8D57A2b8' │
│          MockFlashLoanReceiver          │ '0x80258fd4326bE7C97CDc181584347D859a4c012b' │
└─────────────────────────────────────────┴──────────────────────────────────────────────┘

Mintable Reserves and Rewards
┌────────────────────────────────┬──────────────────────────────────────────────┐
│            (index)             │                   address                    │
├────────────────────────────────┼──────────────────────────────────────────────┤
│ WETH-TestnetMintableERC20-Aave │ '0xd74047010D77c5901df5b0f9ca518aED56C85e8D' │
│ DAI-TestnetMintableERC20-Aave  │ '0x4aAded56bd7c69861E8654719195fCA9C670EB45' │
│ LINK-TestnetMintableERC20-Aave │ '0x237f409fBD10E30e237d63d9050Ae302e339028E' │
│ USDC-TestnetMintableERC20-Aave │ '0xb18d016cDD2d9439A19f15633005A6b2cd6Aa774' │
│ WBTC-TestnetMintableERC20-Aave │ '0x124F70a8a3246F177b0067F435f5691Ee4e467DD' │
│ USDT-TestnetMintableERC20-Aave │ '0x326005cFdF58bfB38650396836BEBF815F5ab4dD' │
│ AAVE-TestnetMintableERC20-Aave │ '0x100aB78E5A565a94f2a191714A7a1B727268eFFb' │
│ EURS-TestnetMintableERC20-Aave │ '0x7eEB186F13538e6795a0823e2D7283FEeD2738f5' │
└────────────────────────────────┴──────────────────────────────────────────────┘
```


{% endtab %}

{% tab title="Kovan" %}
```
te | Accounts after deployment
1|kovan-te | ========
1|kovan-te | ┌─────────┬──────────────────────────────────┬──────────────────────────────────────────────┬────────────────────────┐
1|kovan-te | │ (index) │               name               │                   account                    │        balance         │
1|kovan-te | ├─────────┼──────────────────────────────────┼──────────────────────────────────────────────┼────────────────────────┤
1|kovan-te | │    0    │            'deployer'            │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '0.931178255734458184' │
1|kovan-te | │    1    │            'aclAdmin'            │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '0.931178255734458184' │
1|kovan-te | │    2    │         'emergencyAdmin'         │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '0.931178255734458184' │
1|kovan-te | │    3    │           'poolAdmin'            │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '0.931178255734458184' │
1|kovan-te | │    4    │ 'addressesProviderRegistryOwner' │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '0.931178255734458184' │
1|kovan-te | │    5    │       'treasuryProxyAdmin'       │ '0x04c94825C3e3539e0f2bB21d435302d08B2Dbd77' │      '0.09703729'      │
1|kovan-te | │    6    │      'incentivesProxyAdmin'      │ '0x04c94825C3e3539e0f2bB21d435302d08B2Dbd77' │      '0.09703729'      │
1|kovan-te | │    7    │   'incentivesEmissionManager'    │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '0.931178255734458184' │
1|kovan-te | │    8    │     'incentivesRewardsVault'     │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '0.931178255734458184' │
1|kovan-te | └─────────┴──────────────────────────────────┴──────────────────────────────────────────────┴────────────────────────┘
1|kovan-te | 
1|kovan-te | Deployments
1|kovan-te | ===========
1|kovan-te | ┌─────────────────────────────────────────┬──────────────────────────────────────────────┐
1|kovan-te | │                 (index)                 │                   address                    │
1|kovan-te | ├─────────────────────────────────────────┼──────────────────────────────────────────────┤
1|kovan-te | │    AAVE-TestnetPriceAggregator-Aave     │ '0xd237aDd251323Cd757fFCB4Ae3e36d31a603163A' │
1|kovan-te | │             ACLManager-Aave             │ '0x9D2729bC36f9E203002Bc5B5ee2E08C68Bd13794' │
1|kovan-te | │               AToken-Aave               │ '0x03422c68e48790B4465f2d856596d808bE3b75Ea' │
1|kovan-te | │             AaveOracle-Aave             │ '0x550f9764d56291B5B793b6dD1623af3346128BD2' │
1|kovan-te | │               BorrowLogic               │ '0xc16D86A31fc3a1b68a5F4d9c71A0fff562A67fe5' │
1|kovan-te | │               BridgeLogic               │ '0x71A6FDa341405281236174d4377ba81C750A73dd' │
1|kovan-te | │            ConfiguratorLogic            │ '0x623C6B904a74130B81eec2dE0d32FD9E54F3dd04' │
1|kovan-te | │     DAI-TestnetPriceAggregator-Aave     │ '0xBF2b3c17d297B30F2A20599F056Fc905E46A5a91' │
1|kovan-te | │       DelegationAwareAToken-Aave        │ '0x87530ED4bd0ee0e79661D65f8Dd37538F693afD5' │
1|kovan-te | │               EModeLogic                │ '0x14C0BF50D98287071E174d8F9E75D41C4d3e011C' │
1|kovan-te | │            ERC20Faucet-Aave             │ '0x06Db7973d6D724Eb15404a0d178624f6e07834F9' │
1|kovan-te | │    EURS-TestnetPriceAggregator-Aave     │ '0xB0782094001F15Fd0EA8e573D72b488b06574649' │
1|kovan-te | │           FallbackOracle-Aave           │ '0x4aAded56bd7c69861E8654719195fCA9C670EB45' │
1|kovan-te | │             FlashLoanLogic              │ '0xAa344F7Bed7B971df25aaf0E87A7a73d434D340a' │
1|kovan-te | │             IncentivesProxy             │ '0x6aEc12D18b9702778227F7EC09f2e640BEEBed3d' │
1|kovan-te | │       IncentivesV2-Implementation       │ '0xC7fEC323B14aA86597933ada3583b118a893B786' │
1|kovan-te | │    LINK-TestnetPriceAggregator-Aave     │ '0x6bEda4d300945b461C78fEE9771C5cB8B9cfF00e' │
1|kovan-te | │            LiquidationLogic             │ '0xAc6D153BF94aFBdC296e72163735B0f94581F736' │
1|kovan-te | │           Pool-Implementation           │ '0xD1D7347DC861A86Cb5ba39fdD7f8335aCa7bD81A' │
1|kovan-te | │             Pool-Proxy-Aave             │ '0x329462f8ed05E5FfBF6dfB84106e76B69e6B1F94' │
1|kovan-te | │       PoolAddressesProvider-Aave        │ '0x651b8A8cA545b251a8f49B57D5838Da0a8DFbEF9' │
1|kovan-te | │      PoolAddressesProviderRegistry      │ '0x3179C833fF0035D3BD42654f3aCAE4B0908af7A7' │
1|kovan-te | │     PoolConfigurator-Implementation     │ '0x45F1e6D6F206bBe33d495Ef94F64C1CF6e50D1BE' │
1|kovan-te | │       PoolConfigurator-Proxy-Aave       │ '0xc351E0C7688f813c3Aab76cE8a8963ed628D4b19' │
1|kovan-te | │          PoolDataProvider-Aave          │ '0x7d23F30DE42c67cc8016e8da8c713448364E02cF' │
1|kovan-te | │                PoolLogic                │ '0xC05FAA52459226aA19eDF47DD858Ff137D41Ce84' │
1|kovan-te | │       PullRewardsTransferStrategy       │ '0xd4a65cBe3A924769dE047a7115Ca859E888FddC4' │
1|kovan-te | │           ReservesSetupHelper           │ '0x3eF99001FB402A8853ca1a06C37554255B4Ce5d9' │
1|kovan-te | │          StableDebtToken-Aave           │ '0x4D7D84cd3A63f61183363F4655e64C0D1E3012d1' │
1|kovan-te | │               SupplyLogic               │ '0xf3f1496178823b5B829E82971B68e3A3c642f4ab' │
1|kovan-te | │           Treasury-Controller           │ '0x32701F47A7b230D87C7F4d407Aa038AA47060802' │
1|kovan-te | │         Treasury-Implementation         │ '0x002A8C1dDcEA2A3A621328ffd3bed31DDACbA46E' │
1|kovan-te | │              TreasuryProxy              │ '0x51b116B1Efb91c60D032540136f15E6989Cf1834' │
1|kovan-te | │    USDC-TestnetPriceAggregator-Aave     │ '0x507EdfcB669a30BbEC4b2f5930FB992a120DF306' │
1|kovan-te | │    USDT-TestnetPriceAggregator-Aave     │ '0x6CE18569B2A517191F342e2aD429DBE013dD12FD' │
1|kovan-te | │        UiIncentiveDataProviderV3        │ '0x335De793a66B839974aED2673b72a452c3Ee93A4' │
1|kovan-te | │          UiPoolDataProviderV3           │ '0x47E83aeB8E1940aF16fF763F2c25ba75a1F4D0c5' │
1|kovan-te | │         VariableDebtToken-Aave          │ '0x3Bbac251F403026ddaE95e4a2352A622C9F340C5' │
1|kovan-te | │    WBTC-TestnetPriceAggregator-Aave     │ '0x1fCB8CdeD02E3e294663EB913B3C8FcB75033f6A' │
1|kovan-te | │    WETH-TestnetPriceAggregator-Aave     │ '0xd52699A7a785c758AA6e4dEB89AE8Ca0245aAD0A' │
1|kovan-te | │               WETHGateway               │ '0x509B2506FbA1BD41765F6A82C7B0Dd4229191768' │
1|kovan-te | │          WalletBalanceProvider          │ '0x57dDbfeab5Dc552d33dC8cacCdB490de80431334' │
1|kovan-te | │  ReserveStrategy-rateStrategyStableTwo  │ '0x26C3249723F2b98be57F49a1a31A9243a4B2cd88' │
1|kovan-te | │ ReserveStrategy-rateStrategyVolatileOne │ '0x74E3445f239f9915D57715Efb810f67b2a7E5758' │
1|kovan-te | │  ReserveStrategy-rateStrategyStableOne  │ '0x71ABaeBCA33Dac8CbF99790DF3c72b42908b8E43' │
1|kovan-te | │            AAVE-AToken-Aave             │ '0x1D4f0D0D1129476377d057da0fA1b7E9a218ea3E' │
1|kovan-te | │       AAVE-VariableDebtToken-Aave       │ '0x66641D1e56d04a3D76A098830828e80900571069' │
1|kovan-te | │        AAVE-StableDebtToken-Aave        │ '0x354b8b5E96E9821f2a984E18Eb871EdbD5AEf139' │
1|kovan-te | │             DAI-AToken-Aave             │ '0xE101EcB2283Acf0C91e05A428DDD8833Ac66B572' │
1|kovan-te | │       DAI-VariableDebtToken-Aave        │ '0xCe26cA5B57704147103649e8d2B41d66F6148737' │
1|kovan-te | │        DAI-StableDebtToken-Aave         │ '0xB7A6a70DB9EA05E2a23283048CC467bCFD608899' │
1|kovan-te | │            EURS-AToken-Aave             │ '0xB2C04224D7692D2884a7Bd4f568D9213951CcE57' │
1|kovan-te | │       EURS-VariableDebtToken-Aave       │ '0xaF1e10d9B2121E37db746292E632ec69F5c94B98' │
1|kovan-te | │        EURS-StableDebtToken-Aave        │ '0xB0e33EB2EaCD35c5600e92e0F0E0AE8D9A496a36' │
1|kovan-te | │            LINK-AToken-Aave             │ '0xf53334d908F3A12AA82f393b599fd5ed97e80F88' │
1|kovan-te | │       LINK-VariableDebtToken-Aave       │ '0xdc283323970C371B884e9c89957b33E89e6Dfad9' │
1|kovan-te | │        LINK-StableDebtToken-Aave        │ '0x6250C0053D9280d5F60fa5006D916F5c80565e79' │
1|kovan-te | │            USDC-AToken-Aave             │ '0x36b5879749812B5f8d5Ed7a37ab465aEDBC5501f' │
1|kovan-te | │       USDC-VariableDebtToken-Aave       │ '0xc7EB21043fe41ac0bd0231Fb075a1Eb04e6f322a' │
1|kovan-te | │        USDC-StableDebtToken-Aave        │ '0xE645173296B71C83A00285299828D5C4B5A1F9e8' │
1|kovan-te | │            USDT-AToken-Aave             │ '0x27c838adB75F101886D2287a778bc35668E11d7b' │
1|kovan-te | │       USDT-VariableDebtToken-Aave       │ '0xb4Ff572ac9c27688eB7DD83F1269240E0acAf0F7' │
1|kovan-te | │        USDT-StableDebtToken-Aave        │ '0xC2e52BAcE294673Aa732a56401d0C7bF077C34bB' │
1|kovan-te | │            WBTC-AToken-Aave             │ '0x317499375dA66D1Ff9A63557A66cAE7c150f0a48' │
1|kovan-te | │       WBTC-VariableDebtToken-Aave       │ '0x2159D1c83802506E2A97b7EE1794fC827F02E72e' │
1|kovan-te | │        WBTC-StableDebtToken-Aave        │ '0x0A10e657970b2c3ceFFdEc91969ba62cf58de915' │
1|kovan-te | │            WETH-AToken-Aave             │ '0xec6E5B3Bd3e8CC74756Af812994361d8D1EF30F8' │
1|kovan-te | │       WETH-VariableDebtToken-Aave       │ '0xE16D896C946060E342EFE319928dF87202609AB7' │
1|kovan-te | │        WETH-StableDebtToken-Aave        │ '0xFB2E04F58ED47AD6E3C9F67A2C990563C8c65232' │
1|kovan-te | │          MockFlashLoanReceiver          │ '0xE72EcD59fEEfE1F7f77BF488346075b057A9012C' │
1|kovan-te | └─────────────────────────────────────────┴──────────────────────────────────────────────┘
1|kovan-te | 
1|kovan-te | Mintable Reserves and Rewards
1|kovan-te | ┌────────────────────────────────┬──────────────────────────────────────────────┐
1|kovan-te | │            (index)             │                   address                    │
1|kovan-te | ├────────────────────────────────┼──────────────────────────────────────────────┤
1|kovan-te | │ AAVE-TestnetMintableERC20-Aave │ '0xA3a8697C4C6A7D9ccF9238cb567b122d53012ac9' │
1|kovan-te | │ DAI-TestnetMintableERC20-Aave  │ '0x58Cd851c28dF05Edc7F018B533C0257DE57673f7' │
1|kovan-te | │ EURS-TestnetMintableERC20-Aave │ '0x8017B7FC5473d05e67E617072fB237D24Add550b' │
1|kovan-te | │ LINK-TestnetMintableERC20-Aave │ '0xFfaDa869df79320120dfFd6eeE8cF664Dba43146' │
1|kovan-te | │ USDC-TestnetMintableERC20-Aave │ '0xa982Aef90A37675C0E321e3e2f3aDC959fB89351' │
1|kovan-te | │ USDT-TestnetMintableERC20-Aave │ '0x8D01d567AFdE8601C6BA784CF0da7Da12b3BFd66' │
1|kovan-te | │ WBTC-TestnetMintableERC20-Aave │ '0xaE4A267987f640AE1b0Dd757854Af00651cf2EC7' │
1|kovan-te | │ WETH-TestnetMintableERC20-Aave │ '0xF1bE881Ee7034ebC0CD47E1af1bA94EC30DF3583' │
1|kovan-te | └────────────────────────────────┴──────────────────────────────────────────────┘
```
{% endtab %}

{% tab title="Arbitrum" %}
```markdown
3|arbitrum | Accounts after deployment
3|arbitrum | ========
3|arbitrum | ┌─────────┬──────────────────────────────────┬──────────────────────────────────────────────┬────────────────────────┐
3|arbitrum | │ (index) │               name               │                   account                    │        balance         │
3|arbitrum | ├─────────┼──────────────────────────────────┼──────────────────────────────────────────────┼────────────────────────┤
3|arbitrum | │    0    │            'deployer'            │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '5.077529874115090877' │
3|arbitrum | │    1    │            'aclAdmin'            │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '5.077529874115090877' │
3|arbitrum | │    2    │         'emergencyAdmin'         │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '5.077529874115090877' │
3|arbitrum | │    3    │           'poolAdmin'            │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '5.077529874115090877' │
3|arbitrum | │    4    │ 'addressesProviderRegistryOwner' │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '5.077529874115090877' │
3|arbitrum | │    5    │       'treasuryProxyAdmin'       │ '0x04c94825C3e3539e0f2bB21d435302d08B2Dbd77' │ '0.99983296768731248'  │
3|arbitrum | │    6    │      'incentivesProxyAdmin'      │ '0x04c94825C3e3539e0f2bB21d435302d08B2Dbd77' │ '0.99983296768731248'  │
3|arbitrum | │    7    │   'incentivesEmissionManager'    │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '5.077529874115090877' │
3|arbitrum | │    8    │     'incentivesRewardsVault'     │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '5.077529874115090877' │
3|arbitrum | └─────────┴──────────────────────────────────┴──────────────────────────────────────────────┴────────────────────────┘
3|arbitrum | 
3|arbitrum | Deployments
3|arbitrum | ===========
3|arbitrum | ┌─────────────────────────────────────────┬──────────────────────────────────────────────┐
3|arbitrum | │                 (index)                 │                   address                    │
3|arbitrum | ├─────────────────────────────────────────┼──────────────────────────────────────────────┤
3|arbitrum | │      PoolAddressesProviderRegistry      │ '0x4D60d51eD920d37D70Df45375A1b856CbeCda8ec' │
3|arbitrum | │               SupplyLogic               │ '0xe59aa6Ea8d9Dc67E328F1E1680025c1d75D46188' │
3|arbitrum | │               BorrowLogic               │ '0x2Da4b924b58116e942C58F2dc7Cb3175977626dd' │
3|arbitrum | │            LiquidationLogic             │ '0x73D8465eAdCaD8d8C002878D86E3e175Aa7C7A98' │
3|arbitrum | │               EModeLogic                │ '0xbAD216a412F4E4b611001F1BD65fb86908873482' │
3|arbitrum | │               BridgeLogic               │ '0x29DB8a5C745aA93eA953A1B6126de138D830BcE3' │
3|arbitrum | │            ConfiguratorLogic            │ '0xBb02a688dC1FE5f90878457A46BbE9baF2119f02' │
3|arbitrum | │             FlashLoanLogic              │ '0x0086701b24ab166aaB4A9f56AA69CA636ee771b6' │
3|arbitrum | │                PoolLogic                │ '0x2d0409275acE648D5a6A911aB46A365459d12564' │
3|arbitrum | │              TreasuryProxy              │ '0x248c15d1b0230Fe589CF032D3A0cdDfBD233d131' │
3|arbitrum | │           Treasury-Controller           │ '0x160824b12e91F09D374154B3c8AA366323111970' │
3|arbitrum | │         Treasury-Implementation         │ '0xB2c3fB5CE7BDCd6Dea68Ac26Eb27958d567F1706' │
3|arbitrum | │               WETHGateway               │ '0xF1C72f4e230289970d60046915c79c4A7A94aae5' │
3|arbitrum | │          WalletBalanceProvider          │ '0xA1B434CC7B9Cf70BE99f19B3721904919CaA5227' │
3|arbitrum | │        UiIncentiveDataProviderV3        │ '0x8E90a6524931E097DefB662B3DEa33809D410E6c' │
3|arbitrum | │          UiPoolDataProviderV3           │ '0xa3D26e300df5Aa91713fB5963A6A6C48777243Aa' │
3|arbitrum | │          ERC20Faucet-Arbitrum           │ '0x3BE25d21ee1C417462E97CEF1D53da9011149384' │
3|arbitrum | │     PoolAddressesProvider-Arbitrum      │ '0xF7158D1412Bdc8EAfc6BF97DB4e2178379c9521c' │
3|arbitrum | │        PoolDataProvider-Arbitrum        │ '0x9bf4b639C27F7aaF2eE2c1917478fd48370CE390' │
3|arbitrum | │  WETH-TestnetPriceAggregator-Arbitrum   │ '0x8837B64670f60c410866D184e527AFF053a417a6' │
3|arbitrum | │   DAI-TestnetPriceAggregator-Arbitrum   │ '0xB126357B8B403567ca3038e5854a295Be43c10eA' │
3|arbitrum | │  LINK-TestnetPriceAggregator-Arbitrum   │ '0xCc72A4a4026A80EEe6EDEa5D60593a96F22001ef' │
3|arbitrum | │  USDC-TestnetPriceAggregator-Arbitrum   │ '0xAe797919D0c52D8A4B7Cda55c208Eef99B06F451' │
3|arbitrum | │  WBTC-TestnetPriceAggregator-Arbitrum   │ '0x5B7bb8C97558F480E0B33aE69422a2Eca9Ed911e' │
3|arbitrum | │  USDT-TestnetPriceAggregator-Arbitrum   │ '0xb63350A9fD683275232b1e11FD149929BD8f5317' │
3|arbitrum | │  AAVE-TestnetPriceAggregator-Arbitrum   │ '0x2532Ea08967C83658a164c888138F797d5F7e1Cd' │
3|arbitrum | │  EURS-TestnetPriceAggregator-Arbitrum   │ '0x2E67F47d0BC562579747F5927240C888d50584Cc' │
3|arbitrum | │              CalldataLogic              │ '0x0dDF0cD3F5a92576822ACC1b34be9767d7Ff143f' │
3|arbitrum | │          L2Pool-Implementation          │ '0xfac2b648C4036AFf0104019E3f43E87135929d8f' │
3|arbitrum | │     PoolConfigurator-Implementation     │ '0x54F2C6abB776fcDC1081a79c92F6cB711c7b7936' │
3|arbitrum | │           ReservesSetupHelper           │ '0x98Ce88b74fe01C4814EaBE18874302223a0FB52a' │
3|arbitrum | │           ACLManager-Arbitrum           │ '0x371cdFcbeA8f98e045184373d83c10762E193075' │
3|arbitrum | │           AaveOracle-Arbitrum           │ '0x2560A04c24E8870bB12eE7A9E2DcC4186362F3A1' │
3|arbitrum | │         FallbackOracle-Arbitrum         │ '0xa43Bf6Ff0B831E23C0b658e778a26E0fc8a4C47a' │
3|arbitrum | │           Pool-Proxy-Arbitrum           │ '0x9C55a3C34de5fd46004Fa44a55490108f7cE388F' │
3|arbitrum | │     PoolConfigurator-Proxy-Arbitrum     │ '0xCf7e77c25e04d5F44AA20C505fbda04BFCF60c0b' │
3|arbitrum | │                L2Encoder                │ '0x3d0d309DC8f999f34c4E7296dB38F0e65D3115DF' │
3|arbitrum | │             IncentivesProxy             │ '0x4C7c962A4D3FD03c60beDd940b37A7923c5F5EA8' │
3|arbitrum | │       IncentivesV2-Implementation       │ '0xa7fFc70d21854A09f4b65D7db9487DD7d93af8D5' │
3|arbitrum | │       PullRewardsTransferStrategy       │ '0x163544d8AA15F61a33F1D7af185e9Fe0cee9e6D6' │
3|arbitrum | │             AToken-Arbitrum             │ '0xB09381C9674e034577E3af9a0B4660042d0Efc00' │
3|arbitrum | │     DelegationAwareAToken-Arbitrum      │ '0x854af3F32E7b841AdDC846B67257fd20249e02b4' │
3|arbitrum | │        StableDebtToken-Arbitrum         │ '0x295D001b288742F97a2162208a97fD4FCdA46F71' │
3|arbitrum | │       VariableDebtToken-Arbitrum        │ '0x8fD53ff646070d8FA781AA70419354651978A4E3' │
3|arbitrum | │  ReserveStrategy-rateStrategyStableTwo  │ '0x70c77Cfc188DDEa4Fd0cB5De9Bc3f988E5c31921' │
3|arbitrum | │ ReserveStrategy-rateStrategyVolatileOne │ '0xdad6790Fe4D331C07575CfcB48F608746dE2D2Ee' │
3|arbitrum | │  ReserveStrategy-rateStrategyStableOne  │ '0x5b6eb2AbfE5Ddf2cFE12eC0775c2CC5E6894cF12' │
3|arbitrum | │          WETH-AToken-Arbitrum           │ '0xD7a3657B2B395a166cD068269B4a3f42Fd2ef5D8' │
3|arbitrum | │     WETH-VariableDebtToken-Arbitrum     │ '0x38fcFDEb4A31F0C36502A91eab3585deE9F5955f' │
3|arbitrum | │      WETH-StableDebtToken-Arbitrum      │ '0x84B63b4607E47Ae1E17907200690feFBFfF804aD' │
3|arbitrum | │           DAI-AToken-Arbitrum           │ '0x38c4f078813bcAc22b4c580A870F812377615D59' │
3|arbitrum | │     DAI-VariableDebtToken-Arbitrum      │ '0x7e983CD5e2Af0Dc0519fA15F0D8D1b4EDd04e588' │
3|arbitrum | │      DAI-StableDebtToken-Arbitrum       │ '0xa626040B7Ec7febdA5c4f470d88541Fcb9e465a9' │
3|arbitrum | │          LINK-AToken-Arbitrum           │ '0x9F3399055a08549F706353BbD0796cB682337529' │
3|arbitrum | │     LINK-VariableDebtToken-Arbitrum     │ '0x1cFCee0E4B082f466735Ad4BC38F35Df87a6ad56' │
3|arbitrum | │      LINK-StableDebtToken-Arbitrum      │ '0x60399941B74464eCe33cb681d830fa4e7370D3dc' │
3|arbitrum | │          USDC-AToken-Arbitrum           │ '0x80a8F2FcC1fF2A658cd684b27227CB85eC0eebab' │
3|arbitrum | │     USDC-VariableDebtToken-Arbitrum     │ '0x23FCB713dfFd6D8D213eB16C5Eb70673A7e7A462' │
3|arbitrum | │      USDC-StableDebtToken-Arbitrum      │ '0x057A698a4fD2C486dd269E285e1c4Cbfac2D0A4B' │
3|arbitrum | │          WBTC-AToken-Arbitrum           │ '0x020Ccb5Fcbb05d7d4C6cF702c081d47EC357A68E' │
3|arbitrum | │     WBTC-VariableDebtToken-Arbitrum     │ '0xcC03498c9D8EE0c7c3e2c6032031ac563a3429f1' │
3|arbitrum | │      WBTC-StableDebtToken-Arbitrum      │ '0x314EED755BD345029Eb6A42F1648f889bD7179f0' │
3|arbitrum | │          USDT-AToken-Arbitrum           │ '0xf6dF93819BeBd3A73F4DF43327Ce0f95d148ED47' │
3|arbitrum | │     USDT-VariableDebtToken-Arbitrum     │ '0x95aa1D10444cB5E815f6afe473859177C8829c7d' │
3|arbitrum | │      USDT-StableDebtToken-Arbitrum      │ '0x29E13C2B7B35B4FFf8d3323bC73b5D462d0f22c8' │
3|arbitrum | │          AAVE-AToken-Arbitrum           │ '0xD5B608ec055675661c5425f0B92301F32A8f1aCA' │
3|arbitrum | │     AAVE-VariableDebtToken-Arbitrum     │ '0xCE2d1a5b977E42741147214a67B22Bc703B8Dcd5' │
3|arbitrum | │      AAVE-StableDebtToken-Arbitrum      │ '0x6895ACc82d5556e8289c65c1eA60D8E96D00a94B' │
3|arbitrum | │          EURS-AToken-Arbitrum           │ '0x81728cFD25eE94285322fE7fd2AC163ba24040b0' │
3|arbitrum | │     EURS-VariableDebtToken-Arbitrum     │ '0x8447f6e52731cB9cA2e39945B93297A53FA7c29f' │
3|arbitrum | │      EURS-StableDebtToken-Arbitrum      │ '0x831d5aE09d3CD39d1ac601bA1E73eA639b96B2f6' │
3|arbitrum | │          MockFlashLoanReceiver          │ '0x53CeE8e8513e19D752e9CA45B5F19691D1fe67c3' │
3|arbitrum | └─────────────────────────────────────────┴──────────────────────────────────────────────┘
3|arbitrum | 
3|arbitrum | Mintable Reserves and Rewards
3|arbitrum | ┌────────────────────────────────────┬──────────────────────────────────────────────┐
3|arbitrum | │              (index)               │                   address                    │
3|arbitrum | ├────────────────────────────────────┼──────────────────────────────────────────────┤
3|arbitrum | │ WETH-TestnetMintableERC20-Arbitrum │ '0x5eb35Fe1f1074Ae8d6D23Bf771705846Cc812c09' │
3|arbitrum | │ DAI-TestnetMintableERC20-Arbitrum  │ '0x200c2386A02cbA50563b7b64615B43Ab1874a06e' │
3|arbitrum | │ LINK-TestnetMintableERC20-Arbitrum │ '0x403052a80d33A79Bef4645c0D8Ff00FA03f424c7' │
3|arbitrum | │ USDC-TestnetMintableERC20-Arbitrum │ '0x774382EF196781400a335AF0c4219eEd684ED713' │
3|arbitrum | │ WBTC-TestnetMintableERC20-Arbitrum │ '0x1F7dC0B961950c69584d0F9cE290A918124d32CD' │
3|arbitrum | │ USDT-TestnetMintableERC20-Arbitrum │ '0x7c53810c756C636cEF076c92D5D7C04555694E76' │
3|arbitrum | │ AAVE-TestnetMintableERC20-Arbitrum │ '0x31f909C64E93f764dc90d78DCBB38a6A6D1D48dE' │
3|arbitrum | │ EURS-TestnetMintableERC20-Arbitrum │ '0xaB874B1862938704Cf44Fb81E33c59B67c6BeC07' │
3|arbitrum | └────────────────────────────────────┴──────────────────────────────────────────────┘
```
{% endtab %}

{% tab title="Optimism" %}
```
|optimism | Accounts after deployment
2|optimism | ========
2|optimism | ┌─────────┬──────────────────────────────────┬──────────────────────────────────────────────┬────────────────────────┐
2|optimism | │ (index) │               name               │                   account                    │        balance         │
2|optimism | ├─────────┼──────────────────────────────────┼──────────────────────────────────────────────┼────────────────────────┤
2|optimism | │    0    │            'deployer'            │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '0.899866034761479837' │
2|optimism | │    1    │            'aclAdmin'            │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '0.899866034761479837' │
2|optimism | │    2    │         'emergencyAdmin'         │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '0.899866034761479837' │
2|optimism | │    3    │           'poolAdmin'            │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '0.899866034761479837' │
2|optimism | │    4    │ 'addressesProviderRegistryOwner' │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '0.899866034761479837' │
2|optimism | │    5    │       'treasuryProxyAdmin'       │ '0x04c94825C3e3539e0f2bB21d435302d08B2Dbd77' │ '0.049999987636082397' │
2|optimism | │    6    │      'incentivesProxyAdmin'      │ '0x04c94825C3e3539e0f2bB21d435302d08B2Dbd77' │ '0.049999987636082397' │
2|optimism | │    7    │   'incentivesEmissionManager'    │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '0.899866034761479837' │
2|optimism | │    8    │     'incentivesRewardsVault'     │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '0.899866034761479837' │
2|optimism | └─────────┴──────────────────────────────────┴──────────────────────────────────────────────┴────────────────────────┘
2|optimism | 
2|optimism | Deployments
2|optimism | ===========
2|optimism | ┌─────────────────────────────────────────┬──────────────────────────────────────────────┐
2|optimism | │                 (index)                 │                   address                    │
2|optimism | ├─────────────────────────────────────────┼──────────────────────────────────────────────┤
2|optimism | │ AAVE-TestnetPriceAggregator-Optimistic  │ '0xaA5A5AD49CAE9C0F19F46dB76feDA55c1B52daD4' │
2|optimism | │          ACLManager-Optimistic          │ '0x552626e2E6e35566d53CE0C5Ad97d72E95bC3fc3' │
2|optimism | │          AaveOracle-Optimistic          │ '0xce87225e5A0ABFe6241C6A60158840d509a84B47' │
2|optimism | │               BorrowLogic               │ '0x69564eAb943c5990eA4F7129b030608EF048EBBb' │
2|optimism | │               BridgeLogic               │ '0x5D3ae67aE4d6917d7B858AD55D52B869A624A9b0' │
2|optimism | │              CalldataLogic              │ '0x051050569eD8A46422F3b9489c529CA5771992C9' │
2|optimism | │            ConfiguratorLogic            │ '0x87d4802e740934ca8671050b4820561242570077' │
2|optimism | │  DAI-TestnetPriceAggregator-Optimistic  │ '0x8d493C61838B3A05888bBc5B61d95C5A1edBd431' │
2|optimism | │               EModeLogic                │ '0x3937800B8EbB13E892f2BC4a7f2fA7aDe02375ca' │
2|optimism | │         ERC20Faucet-Optimistic          │ '0xed97140B58B97FaF70b70Ae26714Aa59705c74aE' │
2|optimism | │        FallbackOracle-Optimistic        │ '0x9075aaBA90d54E491EA43498A171e1908b18731c' │
2|optimism | │             FlashLoanLogic              │ '0x19036f43D29C4A7B0d6Ae1ADF18a9ded6C0635fA' │
2|optimism | │          L2Pool-Implementation          │ '0x84EEC5bC5543B7Facb1f07290dEa97fB9dC127F4' │
2|optimism | │ LINK-TestnetPriceAggregator-Optimistic  │ '0x5B3D60512c077A779597031d1C91F828D9a39372' │
2|optimism | │            LiquidationLogic             │ '0x0008aBb7592559D13185b38d3BeF46eD3ee13064' │
2|optimism | │    PoolAddressesProvider-Optimistic     │ '0xD15d36975A0200D11B8a8964F4F267982D2a1cFe' │
2|optimism | │      PoolAddressesProviderRegistry      │ '0x3179C833fF0035D3BD42654f3aCAE4B0908af7A7' │
2|optimism | │     PoolConfigurator-Implementation     │ '0xAc2F2c1C7Ff2721C5d0B29c98ED5aCd110b4022a' │
2|optimism | │       PoolDataProvider-Optimistic       │ '0x2f733c0389bfF96a3f930Deb2f6DB1d767Cd3215' │
2|optimism | │                PoolLogic                │ '0x05aAFF5DE9dc88a9DF9B2113b98730D4Ab8E2983' │
2|optimism | │           ReservesSetupHelper           │ '0x50463B271c3A3578DB88Ca18F0B2b80F8DF623eC' │
2|optimism | │ SUSD-TestnetPriceAggregator-Optimistic  │ '0xE9ABd1387226A65E2C8B68929b42E5B2A50A7DDF' │
2|optimism | │               SupplyLogic               │ '0xb38c10c43871DFe9E76c942A2a4bbb1FB274efD4' │
2|optimism | │           Treasury-Controller           │ '0x9b791f6A34B2C87c360902F050dA5e0075b7A567' │
2|optimism | │         Treasury-Implementation         │ '0x83c8bb29D4278c4551da3d5E824Cc4144548E5E0' │
2|optimism | │              TreasuryProxy              │ '0x733DC8C72B189791B28Dc8c6Fb09D9201b01eF2f' │
2|optimism | │ USDC-TestnetPriceAggregator-Optimistic  │ '0x95271D9B95fE8b888ED38440fAC50395568DfEC0' │
2|optimism | │ USDT-TestnetPriceAggregator-Optimistic  │ '0x10E5744B09Fb08b8373D6955952A8831A1285bbe' │
2|optimism | │        UiIncentiveDataProviderV3        │ '0xe2E3a30E77469397dc3CF74f1Fa35f39493207C2' │
2|optimism | │          UiPoolDataProviderV3           │ '0xBCb61ecc7997cc736E4802de2D5ce76D0908C97c' │
2|optimism | │ WBTC-TestnetPriceAggregator-Optimistic  │ '0x3cAF4f17bFBFc776545BF7FAae986523a12A6784' │
2|optimism | │ WETH-TestnetPriceAggregator-Optimistic  │ '0xdCfe7bf3f8d145a92437b7dc280de060B9Fd73Eb' │
2|optimism | │               WETHGateway               │ '0x698851Fc324Ff9572289Dd72dfC102DB778b52f1' │
2|optimism | │          WalletBalanceProvider          │ '0xA8751C0e2383cE144a95386A2E30f7E2BD78236C' │
2|optimism | │          Pool-Proxy-Optimistic          │ '0x139d8F557f70D1903787e929D7C42165c4667229' │
2|optimism | │    PoolConfigurator-Proxy-Optimistic    │ '0x12F6E19b968e34fEE34763469c7EAf902Af6914B' │
2|optimism | │                L2Encoder                │ '0xacC688c1246fb8Fb421D1DDe20dF2F59844178b5' │
2|optimism | │             IncentivesProxy             │ '0x12d8A50922f634E2c153DcD4D2c67b963644729F' │
2|optimism | │       IncentivesV2-Implementation       │ '0x36144551a6b2b193edAa2aB467916f8688166B35' │
2|optimism | │       PullRewardsTransferStrategy       │ '0xD05A0AFF4A64fAB790930156862A6dEece86F279' │
2|optimism | │            AToken-Optimistic            │ '0xD772d40fb2C14ca80f35EBcb5D1eD85eDA115212' │
2|optimism | │    DelegationAwareAToken-Optimistic     │ '0x24d5260774901392cc2763310dcCe10f735A5659' │
2|optimism | │       StableDebtToken-Optimistic        │ '0x728592068F66F935B5F53E9be4c51d21B1a30139' │
2|optimism | │      VariableDebtToken-Optimistic       │ '0x4737ff5142fA2CC962a0755B5D311753bbF3b8EF' │
2|optimism | │  ReserveStrategy-rateStrategyStableTwo  │ '0xbAAC1D3Dd2019857836C1Db0E6E28b21b1E42efB' │
2|optimism | │ ReserveStrategy-rateStrategyVolatileOne │ '0x3DD3FbDcF9D0d64d21EEd9148568363E44ddDEC6' │
2|optimism | │  ReserveStrategy-rateStrategyStableOne  │ '0x08653Dc107cC46E7429b70F85DeC05c22C2b7B26' │
2|optimism | │         AAVE-AToken-Optimistic          │ '0x5994ce8E7F595AFE3115D72854e0EAeCbD902ea7' │
2|optimism | │    AAVE-VariableDebtToken-Optimistic    │ '0xb45966470789847E7bC73E2aEdFefff96c86F821' │
2|optimism | │     AAVE-StableDebtToken-Optimistic     │ '0xBe7c6a35A2932411A379081a745bcb99d83574EC' │
2|optimism | │          DAI-AToken-Optimistic          │ '0x4cdb5D85687Fa162446c7Cf263f9be9614E6314B' │
2|optimism | │    DAI-VariableDebtToken-Optimistic     │ '0x4F02eD54a25CD9D5bc3432f4bD82f39655A9F4bD' │
2|optimism | │     DAI-StableDebtToken-Optimistic      │ '0xF7f1a6f7A614b12F2f3bcc8a2e0952B2c6bF283d' │
2|optimism | │         LINK-AToken-Optimistic          │ '0x70713F22F01f0053803F1520d526a2C7b26b318a' │
2|optimism | │    LINK-VariableDebtToken-Optimistic    │ '0x36B43B427a618cb2Dda78bEc36B7ed7d0b193071' │
2|optimism | │     LINK-StableDebtToken-Optimistic     │ '0x2074341b6880f6B7FC4f3B2B3B15ef91712182E6' │
2|optimism | │         SUSD-AToken-Optimistic          │ '0xE603E221fa3a858BdAE91FB51cE09BA6C53B19A5' │
2|optimism | │    SUSD-VariableDebtToken-Optimistic    │ '0xd3a31fD51e6F0Ca6b4a083e05893bfC6e294cb30' │
2|optimism | │     SUSD-StableDebtToken-Optimistic     │ '0xF864A79eE389859A33DA2CDec69fb1d723dB319B' │
2|optimism | │         USDC-AToken-Optimistic          │ '0x0849Cd326DC590bF313a0b1E5a04790CBb4eE387' │
2|optimism | │    USDC-VariableDebtToken-Optimistic    │ '0x3cB29D1F440d7ffADACCd57762c1332CF7Db9e6c' │
2|optimism | │     USDC-StableDebtToken-Optimistic     │ '0xE953b08a7908921e179187bAf7dFb4e36f9b40CA' │
2|optimism | │         USDT-AToken-Optimistic          │ '0x98A978662670A35cA2b4aD12319486a3F294a78b' │
2|optimism | │    USDT-VariableDebtToken-Optimistic    │ '0x163F2F60F99090E1fF7d7eC768dA0BA77Dd50547' │
2|optimism | │     USDT-StableDebtToken-Optimistic     │ '0x1b187f0e91934c94aFb324cD9cd03FBa0C7a8B71' │
2|optimism | │         WBTC-AToken-Optimistic          │ '0x2D89bE7Cfbe21ed728A5AeDdA03cACFCAf04aA08' │
2|optimism | │    WBTC-VariableDebtToken-Optimistic    │ '0x5a9BaC403F9034852Ed18613Ecac81A1FaE2AdF3' │
2|optimism | │     WBTC-StableDebtToken-Optimistic     │ '0x4c9D6192E7920b2C56400aBFa8909EC7A572a315' │
2|optimism | │         WETH-AToken-Optimistic          │ '0xCb5Df0b49BCa05B2478a606074ec39e3fa181a6f' │
2|optimism | │    WETH-VariableDebtToken-Optimistic    │ '0x90De0e1eBDBfDb421F79D26EccE37cE1Aa84bbA6' │
2|optimism | │     WETH-StableDebtToken-Optimistic     │ '0x52B61cD2CbC22A386a8F5d2Cec685e938A0379BB' │
2|optimism | │          MockFlashLoanReceiver          │ '0x5E52dEc931FFb32f609681B8438A51c675cc232d' │
2|optimism | └─────────────────────────────────────────┴──────────────────────────────────────────────┘
2|optimism | 
2|optimism | Mintable Reserves and Rewards
2|optimism | ┌──────────────────────────────────────┬──────────────────────────────────────────────┐
2|optimism | │               (index)                │                   address                    │
2|optimism | ├──────────────────────────────────────┼──────────────────────────────────────────────┤
2|optimism | │ AAVE-TestnetMintableERC20-Optimistic │ '0xb532118d86765Eb544958e47df77bb8bDDe2F096' │
2|optimism | │ DAI-TestnetMintableERC20-Optimistic  │ '0xd6B095c27bDf158C462AaB8Cb947BdA9351C0e1d' │
2|optimism | │ LINK-TestnetMintableERC20-Optimistic │ '0xFbBCcCCA95b5F676D8f044Ec75e7eA5899280efF' │
2|optimism | │ SUSD-TestnetMintableERC20-Optimistic │ '0x6883D765088f90bAE62048dE45f2202D72985B01' │
2|optimism | │ USDC-TestnetMintableERC20-Optimistic │ '0x9cCc44Aa7C301b6655ec9891BdaD20fa6eb2b552' │
2|optimism | │ USDT-TestnetMintableERC20-Optimistic │ '0xeE6b5ad81c7d88a632b24Bcdac055D6f5F469495' │
2|optimism | │ WBTC-TestnetMintableERC20-Optimistic │ '0xfF5b900f020d663719EEE1731C21778632e6C424' │
2|optimism | │ WETH-TestnetMintableERC20-Optimistic │ '0x46e213C62d4734C64986879af00eEc5128395776' │
2|optimism | └──────────────────────────────────────┴──────────────────────────────────────────────┘
```
{% endtab %}

{% tab title="Avalanche" %}
```
5|avalanch | Accounts after deployment
5|avalanch | ========
5|avalanch | ┌─────────┬──────────────────────────────────┬──────────────────────────────────────────────┬───────────────┐
5|avalanch | │ (index) │               name               │                   account                    │    balance    │
5|avalanch | ├─────────┼──────────────────────────────────┼──────────────────────────────────────────────┼───────────────┤
5|avalanch | │    0    │            'deployer'            │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '2.168418425' │
5|avalanch | │    1    │            'aclAdmin'            │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '2.168418425' │
5|avalanch | │    2    │         'emergencyAdmin'         │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '2.168418425' │
5|avalanch | │    3    │           'poolAdmin'            │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '2.168418425' │
5|avalanch | │    4    │ 'addressesProviderRegistryOwner' │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '2.168418425' │
5|avalanch | │    5    │       'treasuryProxyAdmin'       │ '0x04c94825C3e3539e0f2bB21d435302d08B2Dbd77' │ '0.042593225' │
5|avalanch | │    6    │      'incentivesProxyAdmin'      │ '0x04c94825C3e3539e0f2bB21d435302d08B2Dbd77' │ '0.042593225' │
5|avalanch | │    7    │   'incentivesEmissionManager'    │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '2.168418425' │
5|avalanch | │    8    │     'incentivesRewardsVault'     │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '2.168418425' │
5|avalanch | └─────────┴──────────────────────────────────┴──────────────────────────────────────────────┴───────────────┘
5|avalanch | 
5|avalanch | Deployments
5|avalanch | ===========
5|avalanch | ┌─────────────────────────────────────────┬──────────────────────────────────────────────┐
5|avalanch | │                 (index)                 │                   address                    │
5|avalanch | ├─────────────────────────────────────────┼──────────────────────────────────────────────┤
5|avalanch | │      PoolAddressesProviderRegistry      │ '0x85E44420b6137bbc75a85CAB5c9A3371af976FdE' │
5|avalanch | │               SupplyLogic               │ '0x21C561e551638401b937b03fE5a0a0652B99B7DD' │
5|avalanch | │               BorrowLogic               │ '0x0AB1917A0cf92cdcf7F7b637EaC3A46BBBE41409' │
5|avalanch | │            LiquidationLogic             │ '0x3e4b51076d7e9B844B92F8c6377087f9cf8C8696' │
5|avalanch | │               EModeLogic                │ '0xdDc3C9B8614092e6188A86450c8D597509893E20' │
5|avalanch | │               BridgeLogic               │ '0x8AaF462990dD5CC574c94C8266208996426A47e7' │
5|avalanch | │            ConfiguratorLogic            │ '0xE3981f4840843D67aF50026d34DA0f7e56A02D69' │
5|avalanch | │             FlashLoanLogic              │ '0x56e0507A53Ee252947a1E55D84Dc4032F914DD98' │
5|avalanch | │                PoolLogic                │ '0x302567472401C7c7B50ee7eb3418c375D8E3F728' │
5|avalanch | │              TreasuryProxy              │ '0xBaaCc99123133851Ba2D6d34952aa08CBDf5A4E4' │
5|avalanch | │           Treasury-Controller           │ '0xFCadBDefd30E11258559Ba239C8a5A8A8D28CB00' │
5|avalanch | │         Treasury-Implementation         │ '0xc1eB89DA925cc2Ae8B36818d26E12DDF8F8601b0' │
5|avalanch | │               WETHGateway               │ '0x8f57153F18b7273f9A814b93b31Cb3f9b035e7C2' │
5|avalanch | │          WalletBalanceProvider          │ '0xd19443202328A66875a51560c28276868B8C61C2' │
5|avalanch | │        UiIncentiveDataProviderV3        │ '0x036dDd300B57F6a8A6A55e2ede8b50b517A5094f' │
5|avalanch | │          UiPoolDataProviderV3           │ '0x1D01f7d8B42Ec47837966732f831E1D6321df499' │
5|avalanch | │          ERC20Faucet-Avalanche          │ '0x127277bF2F5fA186bfC6b3a0ca00baefB5472d3a' │
5|avalanch | │     PoolAddressesProvider-Avalanche     │ '0x1775ECC8362dB6CaB0c7A9C0957cF656A5276c29' │
5|avalanch | │       PoolDataProvider-Avalanche        │ '0x8e0988b28f9CdDe0134A206dfF94111578498C63' │
5|avalanch | │ WAVAX-TestnetPriceAggregator-Avalanche  │ '0x028806D92D6fC46f301b38EF1cA6d3ceFE7f3E4B' │
5|avalanch | │  DAI-TestnetPriceAggregator-Avalanche   │ '0x1758d4e6f68166C4B2d9d0F049F33dEB399Daa1F' │
5|avalanch | │  LINK-TestnetPriceAggregator-Avalanche  │ '0x823800Cc75b802457e9590Ec9AF183EA76e31778' │
5|avalanch | │  USDC-TestnetPriceAggregator-Avalanche  │ '0xa645C7F315B03BFc2E6000A4885B4EF20b045c3b' │
5|avalanch | │  WBTC-TestnetPriceAggregator-Avalanche  │ '0xfB47BA50CFADe7e16af6e7f55D91f48ADE646Dc6' │
5|avalanch | │  WETH-TestnetPriceAggregator-Avalanche  │ '0x6437b6E14D7ECa1Fa9854df92eB067253D5f683A' │
5|avalanch | │  USDT-TestnetPriceAggregator-Avalanche  │ '0x29Ff3c19C6853A0b6544b3CC241c360f422aBaD1' │
5|avalanch | │  AAVE-TestnetPriceAggregator-Avalanche  │ '0x99b97EB5948eA9aFEC4808F250f4fe245b8b3c02' │
5|avalanch | │           Pool-Implementation           │ '0x73A92E2b1Ec50bdf58aD5A2F6FAFB07d7D00E034' │
5|avalanch | │     PoolConfigurator-Implementation     │ '0x3dBB8bDcA6bF1440CCa11deEA620EB3843162Aa4' │
5|avalanch | │           ReservesSetupHelper           │ '0x520D14AE678b41067f029Ad770E2870F85E76588' │
5|avalanch | │          ACLManager-Avalanche           │ '0xAa6Fd640173bcA58e5a5CC373531F9038eF3F9e1' │
5|avalanch | │          AaveOracle-Avalanche           │ '0xAc6D153BF94aFBdC296e72163735B0f94581F736' │
5|avalanch | │        FallbackOracle-Avalanche         │ '0x14C0BF50D98287071E174d8F9E75D41C4d3e011C' │
5|avalanch | │          Pool-Proxy-Avalanche           │ '0xb47673b7a73D78743AFF1487AF69dBB5763F00cA' │
5|avalanch | │    PoolConfigurator-Proxy-Avalanche     │ '0x01743372F0F0318AaDF690f960A4c6c4eab58782' │
5|avalanch | │             IncentivesProxy             │ '0x58Cd851c28dF05Edc7F018B533C0257DE57673f7' │
5|avalanch | │       IncentivesV2-Implementation       │ '0xFfaDa869df79320120dfFd6eeE8cF664Dba43146' │
5|avalanch | │       PullRewardsTransferStrategy       │ '0x8D01d567AFdE8601C6BA784CF0da7Da12b3BFd66' │
5|avalanch | │            AToken-Avalanche             │ '0xA3a8697C4C6A7D9ccF9238cb567b122d53012ac9' │
5|avalanch | │     DelegationAwareAToken-Avalanche     │ '0x8017B7FC5473d05e67E617072fB237D24Add550b' │
5|avalanch | │        StableDebtToken-Avalanche        │ '0x06Db7973d6D724Eb15404a0d178624f6e07834F9' │
5|avalanch | │       VariableDebtToken-Avalanche       │ '0x651b8A8cA545b251a8f49B57D5838Da0a8DFbEF9' │
5|avalanch | │  ReserveStrategy-rateStrategyStableTwo  │ '0x08a917bbd0E22D496Ca9364B5D21311fe1D31637' │
5|avalanch | │ ReserveStrategy-rateStrategyVolatileOne │ '0xF2038a65f68a94d1CFD0166f087A795341e2eac8' │
5|avalanch | │  ReserveStrategy-rateStrategyStableOne  │ '0x7d23F30DE42c67cc8016e8da8c713448364E02cF' │
5|avalanch | │         WAVAX-AToken-Avalanche          │ '0xC50E6F9E8e6CAd53c42ddCB7A42d616d7420fd3e' │
5|avalanch | │    WAVAX-VariableDebtToken-Avalanche    │ '0xE21840302317b265dB7E530667ACb31188655cA2' │
5|avalanch | │     WAVAX-StableDebtToken-Avalanche     │ '0xaB73C7267347a8dc4d34f9969663E7a64B578C69' │
5|avalanch | │          DAI-AToken-Avalanche           │ '0xC42f40B7E22bcca66B3EE22F3ACb86d24C997CC2' │
5|avalanch | │     DAI-VariableDebtToken-Avalanche     │ '0xCB19d2C32cB4340C67273A5a4f5dD02BCceBbF97' │
5|avalanch | │      DAI-StableDebtToken-Avalanche      │ '0xf5934275da36A067CE00b415F0b876fA403A7198' │
5|avalanch | │          LINK-AToken-Avalanche          │ '0x210a3f864812eAF7f89eE7337EAA1FeA1830C57e' │
5|avalanch | │    LINK-VariableDebtToken-Avalanche     │ '0x1f59c8D4C97E172e42dc3cF62E75464b7e0205bf' │
5|avalanch | │     LINK-StableDebtToken-Avalanche      │ '0x0DDD3C8dfA22d4B5e5Dc086f87d94e4180dAC38D' │
5|avalanch | │          USDC-AToken-Avalanche          │ '0xA79570641bC9cbc6522aA80E2de03bF9F7fd123a' │
5|avalanch | │    USDC-VariableDebtToken-Avalanche     │ '0x796eF05488765B4DeAd23B3C7b9F295139049879' │
5|avalanch | │     USDC-StableDebtToken-Avalanche      │ '0xC168dB86f93F97652462ded450B3Ad5eA9669df2' │
5|avalanch | │          WBTC-AToken-Avalanche          │ '0x07B2C0b69c70e89C94A20A555Ab376E5a6181eE6' │
5|avalanch | │    WBTC-VariableDebtToken-Avalanche     │ '0x9731B6e01222a0772926455e4aEBa3d1ef690F24' │
5|avalanch | │     WBTC-StableDebtToken-Avalanche      │ '0xdfBa66e02c4915708e7Df3C26843D5A3492727d9' │
5|avalanch | │          WETH-AToken-Avalanche          │ '0x618922b15a1a92652818473741531eE255f68741' │
5|avalanch | │    WETH-VariableDebtToken-Avalanche     │ '0x800408b3a399d50fAbB064CB04C205910194017C' │
5|avalanch | │     WETH-StableDebtToken-Avalanche      │ '0xBA932F4F400204c7a05bDF06c6fcA8c114e39d8c' │
5|avalanch | │          USDT-AToken-Avalanche          │ '0x3a7e85a86F952CB61485e2D20BDDb6e15204744f' │
5|avalanch | │    USDT-VariableDebtToken-Avalanche     │ '0x5CC87B358742407E563A6cB665Ce28a6937eAe29' │
5|avalanch | │     USDT-StableDebtToken-Avalanche      │ '0xB66d28fd0FF446aB504dEF6C2BCd0ef5c0AADdD3' │
5|avalanch | │          AAVE-AToken-Avalanche          │ '0xE9C1731e1186362E2ba233BC16614b2a53ecb3F2' │
5|avalanch | │    AAVE-VariableDebtToken-Avalanche     │ '0x1447a3924BE947CE32b1d4045DAE8F99B894CC61' │
5|avalanch | │     AAVE-StableDebtToken-Avalanche      │ '0x118369DcFb3Dfaa36Ad424AF26247c2D91CA1262' │
5|avalanch | │          MockFlashLoanReceiver          │ '0xd237aDd251323Cd757fFCB4Ae3e36d31a603163A' │
5|avalanch | └─────────────────────────────────────────┴──────────────────────────────────────────────┘
5|avalanch | 
5|avalanch | Mintable Reserves and Rewards
5|avalanch | ┌──────────────────────────────────────┬──────────────────────────────────────────────┐
5|avalanch | │               (index)                │                   address                    │
5|avalanch | ├──────────────────────────────────────┼──────────────────────────────────────────────┤
5|avalanch | │ WAVAX-TestnetMintableERC20-Avalanche │ '0x407287b03D1167593AF113d32093942be13A535f' │
5|avalanch | │  DAI-TestnetMintableERC20-Avalanche  │ '0xFc7215C9498Fc12b22Bc0ed335871Db4315f03d3' │
5|avalanch | │ LINK-TestnetMintableERC20-Avalanche  │ '0x73b4C0C45bfB90FC44D9013FA213eF2C2d908D0A' │
5|avalanch | │ USDC-TestnetMintableERC20-Avalanche  │ '0x3E937B4881CBd500d05EeDAB7BA203f2b7B3f74f' │
5|avalanch | │ WBTC-TestnetMintableERC20-Avalanche  │ '0x09C85Ef96e93f0ae892561052B48AE9DB29F2458' │
5|avalanch | │ WETH-TestnetMintableERC20-Avalanche  │ '0x28A8E6e41F84e62284970E4bc0867cEe2AAd0DA4' │
5|avalanch | │ USDT-TestnetMintableERC20-Avalanche  │ '0xD90db1ca5A6e9873BCD9B0279AE038272b656728' │
5|avalanch | │ AAVE-TestnetMintableERC20-Avalanche  │ '0xCcbBaf8D40a5C34bf1c836e8dD33c7B7646706C5' │
5|avalanch | └──────────────────────────────────────┴──────────────────────────────────────────────┘
```
{% endtab %}

{% tab title="Fantom" %}
```
Accounts after deployment
========
┌─────────┬──────────────────────────────────┬──────────────────────────────────────────────┬───────────────────┐
│ (index) │               name               │                   account                    │      balance      │
├─────────┼──────────────────────────────────┼──────────────────────────────────────────────┼───────────────────┤
│    0    │            'deployer'            │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '2.1847195064368' │
│    1    │            'aclAdmin'            │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '2.1847195064368' │
│    2    │         'emergencyAdmin'         │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '2.1847195064368' │
│    3    │           'poolAdmin'            │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '2.1847195064368' │
│    4    │ 'addressesProviderRegistryOwner' │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '2.1847195064368' │
│    5    │       'treasuryProxyAdmin'       │ '0x04c94825C3e3539e0f2bB21d435302d08B2Dbd77' │ '9.8222133100384' │
│    6    │      'incentivesProxyAdmin'      │ '0x04c94825C3e3539e0f2bB21d435302d08B2Dbd77' │ '9.8222133100384' │
│    7    │   'incentivesEmissionManager'    │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '2.1847195064368' │
│    8    │     'incentivesRewardsVault'     │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '2.1847195064368' │
└─────────┴──────────────────────────────────┴──────────────────────────────────────────────┴───────────────────┘

Deployments
===========
┌─────────────────────────────────────────┬──────────────────────────────────────────────┐
│                 (index)                 │                   address                    │
├─────────────────────────────────────────┼──────────────────────────────────────────────┤
│   AAVE-TestnetPriceAggregator-Fantom    │ '0x5D929637faf2c27C89BaE4e36DAfAF62526b5a57' │
│            ACLManager-Fantom            │ '0x94f154aba287b3024fb32386463FC52d488bb09B' │
│              AToken-Fantom              │ '0xFc1Ab0379db4B6ad8Bf5Bc1382e108a341E2EaBb' │
│            AaveOracle-Fantom            │ '0xA840C768f7143495790eC8dc2D5f32B71B6Dc113' │
│               BorrowLogic               │ '0x20b6aC6248aFc53FcB4b332126d7f93279a0DED8' │
│               BridgeLogic               │ '0x436524442e3a5d344420bEc846bC32F0dE4B56A0' │
│    CRV-TestnetPriceAggregator-Fantom    │ '0x33FD7Ebc005e33D51590D1B88B50606da423CB17' │
│            ConfiguratorLogic            │ '0x835CA2E00F98B4c4BB8E55f070664e6443a55569' │
│    DAI-TestnetPriceAggregator-Fantom    │ '0x8726Cf3e615046636BB2c783CCEF029597a3dea3' │
│      DelegationAwareAToken-Fantom       │ '0x4e62eB262948671590b8D967BDE048557bdd03eD' │
│               EModeLogic                │ '0x4a1a1ec8a5B70bAe3996e43bD9fAa7407b6A4657' │
│           ERC20Faucet-Fantom            │ '0x02D538e56A729C535F83b2DA20Ddf9AD7281FE6c' │
│          FallbackOracle-Fantom          │ '0xAA60FA0f06bbb7a8Bd8eea037F32Fe664f46dB32' │
│             FlashLoanLogic              │ '0xed63f99f010EF092ad8173ca8E35c4b7E6e164b5' │
│             IncentivesProxy             │ '0x54Bc1D59873A5ABde98cf76B6EcF4075ff65d685' │
│       IncentivesV2-Implementation       │ '0x7337e7FF9abc45c0e43f130C136a072F4794d40b' │
│   LINK-TestnetPriceAggregator-Fantom    │ '0x4175dD315f8b23aa46B26297662B234845051112' │
│            LiquidationLogic             │ '0x0ebeBB4be785B1BEA0D02ed6b3190133E31b5057' │
│           Pool-Implementation           │ '0x2d791Ae6af4FccaD7321fE33D5D44AB9d96A452f' │
│            Pool-Proxy-Fantom            │ '0x771A45a19cE333a19356694C5fc80c76fe9bc741' │
│      PoolAddressesProvider-Fantom       │ '0xE339D30cBa24C70dCCb82B234589E3C83249e658' │
│      PoolAddressesProviderRegistry      │ '0xd79e25047CFD3e75b3Ae7D7AB4758CA315D9d0d1' │
│     PoolConfigurator-Implementation     │ '0x6111d7362965D8A8169eF7966952f752383F3210' │
│      PoolConfigurator-Proxy-Fantom      │ '0x59B84a6C943dD655D9E3B4024fC6AdC0E3f4Ff60' │
│         PoolDataProvider-Fantom         │ '0xCbAcff915f2d10727844ab0f2A4D9768954981e4' │
│                PoolLogic                │ '0x953F78ED512279b7338a744CD6C4887Ee5417c53' │
│       PullRewardsTransferStrategy       │ '0x3160F3f3B55eF85d0D27f04A2d74d426c32de842' │
│  ReserveStrategy-rateStrategyStableOne  │ '0xCCa7d1416518D095E729904aAeA087dBA749A4dC' │
│  ReserveStrategy-rateStrategyStableTwo  │ '0x65E2fe35C30eC218b46266F89847c63c2eDa7Dc7' │
│ ReserveStrategy-rateStrategyVolatileOne │ '0xf4423F4152966eBb106261740da907662A3569C5' │
│           ReservesSetupHelper           │ '0xA9904DF5C31441b5E3A4003E6a34aA1Ac0f7cF6b' │
│   SUSHI-TestnetPriceAggregator-Fantom   │ '0x82afFC26e25e7469D77D1F6922d3c669781CB232' │
│         StableDebtToken-Fantom          │ '0xc048C1b6ac47393F073dA2b3d5D1cc43b94891Fd' │
│               SupplyLogic               │ '0x068e4323d63FC63eC2C7e6FeE5F1253e0D10b70D' │
│           Treasury-Controller           │ '0x7aaB2c2CC186131851d6B1876D16eDc849846042' │
│         Treasury-Implementation         │ '0x307cEf0a7709dF6F40FCBAc76F700aAe4fa27803' │
│              TreasuryProxy              │ '0xF49dA7a22463D140f9f8dc7C91468C8721215496' │
│   USDC-TestnetPriceAggregator-Fantom    │ '0x66FfCBF6430675e16DAEF6C7DE91A78cE07A95d0' │
│   USDT-TestnetPriceAggregator-Fantom    │ '0xa65E093052D95672D5Fb37f6324c4d5dcA225B0E' │
│        UiIncentiveDataProviderV3        │ '0x7Ce8eA134935F9FED1606Ba0dfD0509fec5D3a75' │
│          UiPoolDataProviderV3           │ '0xd0B607bb9e0aA3aFF73a8E99d7EfA54C4bc3d8a9' │
│        VariableDebtToken-Fantom         │ '0x981D8AcaF6af3a46785e7741d22fBE81B25Ebf1e' │
│   WBTC-TestnetPriceAggregator-Fantom    │ '0x08Dbc45B4e520bd9686E3990d8E792507e83627F' │
│   WETH-TestnetPriceAggregator-Fantom    │ '0xfB6A6A48e81F8E0a0cC35cca4ea1946869Cc5F00' │
│               WETHGateway               │ '0x87770f04Bbece8092d777860907798138825f303' │
│   WFTM-TestnetPriceAggregator-Fantom    │ '0x7710d7681c81b2dFF7114120bbC3ddF98F1892ca' │
│          WalletBalanceProvider          │ '0xBb3F2bB6126b0709F738cbe6B50bFE69fd663e73' │
│           AAVE-AToken-Fantom            │ '0xeCbA9a45fDb849548F3e7a621fcBa4f11b3BBDcF' │
│      AAVE-VariableDebtToken-Fantom      │ '0xe90400D7D8acdCcC8c335883097A722AB653890D' │
│       AAVE-StableDebtToken-Fantom       │ '0x460d55849094CDcc8c9582Cf4B58485C08405Ae7' │
│            CRV-AToken-Fantom            │ '0x552f5C364090B954ADA025f0D7963D0a7A60d52b' │
│      CRV-VariableDebtToken-Fantom       │ '0xe4CFEa97831CB0d95CA22597e02dD793bB8f45ae' │
│       CRV-StableDebtToken-Fantom        │ '0x48Cf4cA307f321f0FC24bfAe3119f9abF6B32Ff5' │
│            DAI-AToken-Fantom            │ '0xfb08e04E9c7AfFE693290F739d11D5C3Dd2e19B5' │
│      DAI-VariableDebtToken-Fantom       │ '0x78243313999d4582cfEE48bD5B4466efF6c90fE1' │
│       DAI-StableDebtToken-Fantom        │ '0x87d62612a58a806B926a0A1276DF5C9c6DbE8a5e' │
│           LINK-AToken-Fantom            │ '0x1A7e068f35B19Ff89B7d646D83Ae15C2Db1D93c5' │
│      LINK-VariableDebtToken-Fantom      │ '0x57066BC9569260e9dEC8d224BeB9A8a56209Ff64' │
│       LINK-StableDebtToken-Fantom       │ '0x475e4C43caE948578685462F17FB7fedB85E3F79' │
│           SUSHI-AToken-Fantom           │ '0x6cC739A29b8Eb06981B8bbF22464E4F3f082bBA5' │
│     SUSHI-VariableDebtToken-Fantom      │ '0x5522dFE4b4056BA819D8e675e6999011A31BAf7a' │
│      SUSHI-StableDebtToken-Fantom       │ '0x5f933d8c8fbc9651f3E6bC0652d94fdd09EA139a' │
│           USDC-AToken-Fantom            │ '0xf1090cB4f56fDb659D24DDbC4972bE9D379A6E8c' │
│      USDC-VariableDebtToken-Fantom      │ '0x946765C86B534D8114475BFec8De8De481bA4d1F' │
│       USDC-StableDebtToken-Fantom       │ '0x7e90CE7a0463cc5656c38B5a85C33dF4C8F2523C' │
│           USDT-AToken-Fantom            │ '0x1364B761d75E348B861D7EFaEB64A5b3a37965ec' │
│      USDT-VariableDebtToken-Fantom      │ '0x81Ed0a1D00841B68C6F3956E4E210EFaaeBEBAF1' │
│       USDT-StableDebtToken-Fantom       │ '0xCcE4E4c5327870EfD280645B5a24A50dC01125a4' │
│           WBTC-AToken-Fantom            │ '0xd2ecf7aA363A9dE20088eF1a92D76D4147828B58' │
│      WBTC-VariableDebtToken-Fantom      │ '0x68C3E2eb8F2550E13328B4a9cccac65Ba6C200Be' │
│       WBTC-StableDebtToken-Fantom       │ '0x7e72682d8c90A1eeE1403730f31DCf81551C5aFA' │
│           WETH-AToken-Fantom            │ '0xd29fF48d6Fc110fe227286D5A509a4CB6503732E' │
│      WETH-VariableDebtToken-Fantom      │ '0x27dF3D6eF22A6aC1c8744Fd7A4516a4C8B22084f' │
│       WETH-StableDebtToken-Fantom       │ '0xfD7D3f98aF173B18e5A98fE3b1aE530edab1a988' │
│           WFTM-AToken-Fantom            │ '0x22FDD5F19C49fe954847A6424E4a24C2742fD9EF' │
│      WFTM-VariableDebtToken-Fantom      │ '0x812388F32346e99078B987e84f60dA68348Ac665' │
│       WFTM-StableDebtToken-Fantom       │ '0x67196249e5fE6c2f532ff456E342Abf8eE19D4E3' │
│          MockFlashLoanReceiver          │ '0xa023755537Cd2569BdDD55d0eAE0bD87B587361b' │
└─────────────────────────────────────────┴──────────────────────────────────────────────┘

Mintable Reserves and Rewards
┌───────────────────────────────────┬──────────────────────────────────────────────┐
│              (index)              │                   address                    │
├───────────────────────────────────┼──────────────────────────────────────────────┤
│ AAVE-TestnetMintableERC20-Fantom  │ '0x2a6202B83Bd2562d7460F91E9298abC27a2F0a95' │
│  CRV-TestnetMintableERC20-Fantom  │ '0xAC1a9503D1438B56BAa99939D44555FC2dC286Fc' │
│  DAI-TestnetMintableERC20-Fantom  │ '0xc469ff24046779DE9B61Be7b5DF91dbFfdF1AE02' │
│ LINK-TestnetMintableERC20-Fantom  │ '0x42Dc50EB0d35A62eac61f4E4Bc81875db9F9366e' │
│ SUSHI-TestnetMintableERC20-Fantom │ '0x484b87Aa284f51e71F15Eba1aEb06dFD202D5511' │
│ USDC-TestnetMintableERC20-Fantom  │ '0x06f0790c687A1bED6186ce3624EDD9806edf9F4E' │
│ USDT-TestnetMintableERC20-Fantom  │ '0x1b901d3C9D4ce153326BEeC60e0D4A2e8a9e3cE3' │
│ WBTC-TestnetMintableERC20-Fantom  │ '0xd0404A349A76CD2a4B7AB322B9a6C993dbC3A7E7' │
│ WETH-TestnetMintableERC20-Fantom  │ '0x2aF63215417F90bd45608115452d86D0a1bEAE5E' │
│ WFTM-TestnetMintableERC20-Fantom  │ '0xF7475b635EbE06d9C5178CC40D50856Fa98C7332' │
└───────────────────────────────────┴──────────────────────────────────────────────┘
```
{% endtab %}

{% tab title="Polygon" %}
```
Accounts after deployment
========
┌─────────┬──────────────────────────────────┬──────────────────────────────────────────────┬────────────────────────┐
│ (index) │               name               │                   account                    │        balance         │
├─────────┼──────────────────────────────────┼──────────────────────────────────────────────┼────────────────────────┤
│    0    │            'deployer'            │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '0.200515940679834592' │
│    1    │            'aclAdmin'            │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '0.200515940679834592' │
│    2    │         'emergencyAdmin'         │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '0.200515940679834592' │
│    3    │           'poolAdmin'            │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '0.200515940679834592' │
│    4    │ 'addressesProviderRegistryOwner' │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '0.200515940679834592' │
│    5    │       'treasuryProxyAdmin'       │ '0x04c94825C3e3539e0f2bB21d435302d08B2Dbd77' │     '0.997550826'      │
│    6    │      'incentivesProxyAdmin'      │ '0x04c94825C3e3539e0f2bB21d435302d08B2Dbd77' │     '0.997550826'      │
│    7    │   'incentivesEmissionManager'    │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '0.200515940679834592' │
│    8    │     'incentivesRewardsVault'     │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '0.200515940679834592' │
└─────────┴──────────────────────────────────┴──────────────────────────────────────────────┴────────────────────────┘

Deployments
===========
┌─────────────────────────────────────────┬──────────────────────────────────────────────┐
│                 (index)                 │                   address                    │
├─────────────────────────────────────────┼──────────────────────────────────────────────┤
│   AAVE-TestnetPriceAggregator-Polygon   │ '0xD90db1ca5A6e9873BCD9B0279AE038272b656728' │
│  AGEUR-TestnetPriceAggregator-Polygon   │ '0x028806D92D6fC46f301b38EF1cA6d3ceFE7f3E4B' │
│   BAL-TestnetPriceAggregator-Polygon    │ '0x99B70f90b76716D9f909AD91de7e7F44d3445da4' │
│               BorrowLogic               │ '0x03c9CfA0Fb97500af154EBE01a81D04986795993' │
│               BridgeLogic               │ '0x49461dF60a4b4b275f00fcbb006c176Ae824F655' │
│   CRV-TestnetPriceAggregator-Polygon    │ '0xCcbBaf8D40a5C34bf1c836e8dD33c7B7646706C5' │
│            ConfiguratorLogic            │ '0x9F853170Bfc6383Cf7c03d06b02529850b869Baf' │
│   DAI-TestnetPriceAggregator-Polygon    │ '0x1D01f7d8B42Ec47837966732f831E1D6321df499' │
│   DPI-TestnetPriceAggregator-Polygon    │ '0x36556E9b01BCcCF0017C4998D972614f751Adf14' │
│               EModeLogic                │ '0x82D5a84F4c9c691345C81e7e5556CA6adcA63f0f' │
│           ERC20Faucet-Polygon           │ '0xc1eB89DA925cc2Ae8B36818d26E12DDF8F8601b0' │
│   EURS-TestnetPriceAggregator-Polygon   │ '0x8e0988b28f9CdDe0134A206dfF94111578498C63' │
│             FlashLoanLogic              │ '0x00198f66D12256751Da68B492A9e2b460bF7173A' │
│   GHST-TestnetPriceAggregator-Polygon   │ '0x1775ECC8362dB6CaB0c7A9C0957cF656A5276c29' │
│   JEUR-TestnetPriceAggregator-Polygon   │ '0x55E1267C2e587b6b5E94aD4f72E3eDA725D58b8D' │
│   LINK-TestnetPriceAggregator-Polygon   │ '0xFc7215C9498Fc12b22Bc0ed335871Db4315f03d3' │
│            LiquidationLogic             │ '0x353850c99405CcC0747739FbfE624E2fAAb54595' │
│           Pool-Implementation           │ '0x1758d4e6f68166C4B2d9d0F049F33dEB399Daa1F' │
│      PoolAddressesProvider-Polygon      │ '0x5343b5bA672Ae99d627A1C87866b8E53F47Db2E6' │
│      PoolAddressesProviderRegistry      │ '0xE0987FC9EDfcdcA3CB9618510AaF1D565f4960A6' │
│        PoolDataProvider-Polygon         │ '0x8f57153F18b7273f9A814b93b31Cb3f9b035e7C2' │
│                PoolLogic                │ '0x0940ceaacBF4860d2F7BFA657121B2F26a1676B0' │
│  SUSHI-TestnetPriceAggregator-Polygon   │ '0x127277bF2F5fA186bfC6b3a0ca00baefB5472d3a' │
│               SupplyLogic               │ '0xbd1ee6F441E6702E9Ea23438360FA13995DBA071' │
│           Treasury-Controller           │ '0x810d913542D399F3680F0E806DEDf6EACf0e3383' │
│         Treasury-Implementation         │ '0x3111Aa37Dd484154A6BA4091Dfd282d9AeAfc64C' │
│              TreasuryProxy              │ '0x3B6E7a4750e478D7f7d6A5d464099A02ef164bCC' │
│   USDC-TestnetPriceAggregator-Polygon   │ '0x73b4C0C45bfB90FC44D9013FA213eF2C2d908D0A' │
│   USDT-TestnetPriceAggregator-Polygon   │ '0x28A8E6e41F84e62284970E4bc0867cEe2AAd0DA4' │
│        UiIncentiveDataProviderV3        │ '0xD4b6566313c1dCd8823226bb456d80fc85B03d8B' │
│          UiPoolDataProviderV3           │ '0x94E9E8876Fd68574f17B2cd7Fa19AA8342fFaF51' │
│   WBTC-TestnetPriceAggregator-Polygon   │ '0x3E937B4881CBd500d05EeDAB7BA203f2b7B3f74f' │
│   WETH-TestnetPriceAggregator-Polygon   │ '0x09C85Ef96e93f0ae892561052B48AE9DB29F2458' │
│               WETHGateway               │ '0x2a58E9bbb5434FdA7FF78051a4B82cb0EF669C17' │
│  WMATIC-TestnetPriceAggregator-Polygon  │ '0x036dDd300B57F6a8A6A55e2ede8b50b517A5094f' │
│          WalletBalanceProvider          │ '0x78baC31Ed73c115EB7067d1AfE75eC7B4e16Df9e' │
│     PoolConfigurator-Implementation     │ '0x823800Cc75b802457e9590Ec9AF183EA76e31778' │
│           ReservesSetupHelper           │ '0xa645C7F315B03BFc2E6000A4885B4EF20b045c3b' │
│           ACLManager-Polygon            │ '0x6437b6E14D7ECa1Fa9854df92eB067253D5f683A' │
│           AaveOracle-Polygon            │ '0x520D14AE678b41067f029Ad770E2870F85E76588' │
│         FallbackOracle-Polygon          │ '0x66404DD8A6CddD06158174a1C5817140eD724cb2' │
│           Pool-Proxy-Polygon            │ '0x6C9fB0D5bD9429eb9Cd96B85B81d872281771E6B' │
│     PoolConfigurator-Proxy-Polygon      │ '0x7b47e727eC539CB74A744ae5259ef26743294fca' │
│             IncentivesProxy             │ '0xFfaDa869df79320120dfFd6eeE8cF664Dba43146' │
│       IncentivesV2-Implementation       │ '0xa982Aef90A37675C0E321e3e2f3aDC959fB89351' │
│       PullRewardsTransferStrategy       │ '0xA3a8697C4C6A7D9ccF9238cb567b122d53012ac9' │
│             AToken-Polygon              │ '0x8017B7FC5473d05e67E617072fB237D24Add550b' │
│      DelegationAwareAToken-Polygon      │ '0x06Db7973d6D724Eb15404a0d178624f6e07834F9' │
│         StableDebtToken-Polygon         │ '0x651b8A8cA545b251a8f49B57D5838Da0a8DFbEF9' │
│        VariableDebtToken-Polygon        │ '0x08a917bbd0E22D496Ca9364B5D21311fe1D31637' │
│  ReserveStrategy-rateStrategyStableTwo  │ '0xF2038a65f68a94d1CFD0166f087A795341e2eac8' │
│ ReserveStrategy-rateStrategyVolatileOne │ '0x7d23F30DE42c67cc8016e8da8c713448364E02cF' │
│  ReserveStrategy-rateStrategyStableOne  │ '0xca2413028D0c91f5F88821A13d4A82690945F678' │
│           AAVE-AToken-Polygon           │ '0x50434C5Da807189622Db5fff66379808c58574aD' │
│     AAVE-VariableDebtToken-Polygon      │ '0xb571dcf478E2cC6c0871402fa3Dd4a3C8f6BE66E' │
│      AAVE-StableDebtToken-Polygon       │ '0x26Df87542C50326A5085764b1F650EF2514776B6' │
│          AGEUR-AToken-Polygon           │ '0xbC456dc7E6F882DBc7b11da1048eD253F5DB021D' │
│     AGEUR-VariableDebtToken-Polygon     │ '0x290F8118AAf61e129646F03791227434DFe39669' │
│      AGEUR-StableDebtToken-Polygon      │ '0x706E3AD3F2745722152acc71Da3C76330c2aa258' │
│           BAL-AToken-Polygon            │ '0x6236bfBfB3b6CDBFC311399BE346d61Ab8ab1094' │
│      BAL-VariableDebtToken-Polygon      │ '0xB70013Bde95589330F87cE9a5bD06a89Bc26e38d' │
│       BAL-StableDebtToken-Polygon       │ '0xf28E16644C6389b1B6cF03b3120726b1FfAeDC6E' │
│           CRV-AToken-Polygon            │ '0x4e752fB98b0dCC90b6772f23C52aD33b795dc758' │
│      CRV-VariableDebtToken-Polygon      │ '0xB6704e124997030cE773BB35C1Cc154CF5cE06fB' │
│       CRV-StableDebtToken-Polygon       │ '0x4a6F74A19f05529aF7E7e9f00923FFB990aeBE7B' │
│           DAI-AToken-Polygon            │ '0xDD4f3Ee61466C4158D394d57f3D4C397E91fBc51' │
│      DAI-VariableDebtToken-Polygon      │ '0xB18041Ce2439774c4c7BF611a2a635824cE99032' │
│       DAI-StableDebtToken-Polygon       │ '0x333C04243D048836d53b4ACB3c9aE64875699375' │
│           DPI-AToken-Polygon            │ '0xf815E724973ff3f5Eedc243eAE1a34D1f2a45e0C' │
│      DPI-VariableDebtToken-Polygon      │ '0x6bB285977693F47AC6799F0B3B159130018f4c9c' │
│       DPI-StableDebtToken-Polygon       │ '0x2C64B0ef18bC0616291Dc636b1738DbC675C3f0d' │
│           EURS-AToken-Polygon           │ '0xf6AeDD279Aae7361e70030515f56c22A16d81433' │
│     EURS-VariableDebtToken-Polygon      │ '0x6Fb76894E171eEDF94BB33E650Af90DfdA2c37FC' │
│      EURS-StableDebtToken-Polygon       │ '0xaB7cDf4C6053873650695352634987BbEe472c05' │
│           GHST-AToken-Polygon           │ '0x128cB3720f5d220e1E35512917c3c7fFf064A858' │
│     GHST-VariableDebtToken-Polygon      │ '0x1170823EA41B03e2258f228f617cB549C1faDf28' │
│      GHST-StableDebtToken-Polygon       │ '0x03d6be9Bc91956A0bc39f515CaA77C8C0f81c3fC' │
│           JEUR-AToken-Polygon           │ '0x04cdAA74B111b49EF4044455324C0dDb1C2aa783' │
│     JEUR-VariableDebtToken-Polygon      │ '0x97CD2BA205ff6FF09332892AB216B665793fc39E' │
│      JEUR-StableDebtToken-Polygon       │ '0xdAc793dc4A6850765F0f55224CC77425e67C2b6e' │
│           LINK-AToken-Polygon           │ '0x3e1608F4Db4b37DDf86536ef441890fE3AA9F2Ea' │
│     LINK-VariableDebtToken-Polygon      │ '0x292f1Cc1BcedCd22E860c7C92D21877774B44C16' │
│      LINK-StableDebtToken-Polygon       │ '0x27908f7216Efe649706B68b6a443623D9aaF16D0' │
│          SUSHI-AToken-Polygon           │ '0xb7EA2d40B845A1B49E59c9a5f8B6F67b3c48fA04' │
│     SUSHI-VariableDebtToken-Polygon     │ '0x95230060256d957F852db649B381045ace7983Cc' │
│      SUSHI-StableDebtToken-Polygon      │ '0x169E542d769137E82E704477aDdfFe89e7FB9b90' │
│           USDC-AToken-Polygon           │ '0xCdc2854e97798AfDC74BC420BD5060e022D14607' │
│     USDC-VariableDebtToken-Polygon      │ '0xA24A380813FB7E283Acb8221F5E1e3C01052Bc93' │
│      USDC-StableDebtToken-Polygon       │ '0x01dBEdcb2437c79341cfeC4Cae765C53BE0E6EF7' │
│           USDT-AToken-Polygon           │ '0x6Ca4abE253bd510fCA862b5aBc51211C1E1E8925' │
│     USDT-VariableDebtToken-Polygon      │ '0x444672831D8E4A2350667C14E007F56BEfFcB79f' │
│      USDT-StableDebtToken-Polygon       │ '0xc601b4d43aF91fE4EAe327a2d2B12f37a568E05B' │
│           WBTC-AToken-Polygon           │ '0xde230bC95a03b695be69C44b9AA6C0e9dAc1B143' │
│     WBTC-VariableDebtToken-Polygon      │ '0xFDf3B7af2Cb32E5ADca11cf54d53D02162e8d622' │
│      WBTC-StableDebtToken-Polygon       │ '0x5BcBF666e14eCFe6e21686601c5cA7c7fbe674Cf' │
│           WETH-AToken-Polygon           │ '0x685bF4eab23993E94b4CFb9383599c926B66cF57' │
│     WETH-VariableDebtToken-Polygon      │ '0xb0c924f61B27cf3C114CBD70def08c62843ebb3F' │
│      WETH-StableDebtToken-Polygon       │ '0xC9Ac53b6ae1C653A54ab0E9D44693E807429aF1F' │
│          WMATIC-AToken-Polygon          │ '0x89a6AE840b3F8f489418933A220315eeA36d11fF' │
│    WMATIC-VariableDebtToken-Polygon     │ '0x02a5680AE3b7383854Bf446b1B3Be170E67E689C' │
│     WMATIC-StableDebtToken-Polygon      │ '0xEC59F2FB4EF0C46278857Bf2eC5764485974D17B' │
│          MockFlashLoanReceiver          │ '0x3eF99001FB402A8853ca1a06C37554255B4Ce5d9' │
└─────────────────────────────────────────┴──────────────────────────────────────────────┘

Mintable Reserves and Rewards
┌─────────────────────────────────────┬──────────────────────────────────────────────┐
│               (index)               │                   address                    │
├─────────────────────────────────────┼──────────────────────────────────────────────┤
│  AAVE-TestnetMintableERC20-Polygon  │ '0x0AB1917A0cf92cdcf7F7b637EaC3A46BBBE41409' │
│ AGEUR-TestnetMintableERC20-Polygon  │ '0xFCadBDefd30E11258559Ba239C8a5A8A8D28CB00' │
│  BAL-TestnetMintableERC20-Polygon   │ '0xE3981f4840843D67aF50026d34DA0f7e56A02D69' │
│  CRV-TestnetMintableERC20-Polygon   │ '0x3e4b51076d7e9B844B92F8c6377087f9cf8C8696' │
│  DAI-TestnetMintableERC20-Polygon   │ '0x9A753f0F7886C9fbF63cF59D0D4423C5eFaCE95B' │
│  DPI-TestnetMintableERC20-Polygon   │ '0x56e0507A53Ee252947a1E55D84Dc4032F914DD98' │
│  EURS-TestnetMintableERC20-Polygon  │ '0x302567472401C7c7B50ee7eb3418c375D8E3F728' │
│  GHST-TestnetMintableERC20-Polygon  │ '0x8AaF462990dD5CC574c94C8266208996426A47e7' │
│  JEUR-TestnetMintableERC20-Polygon  │ '0xBaaCc99123133851Ba2D6d34952aa08CBDf5A4E4' │
│  LINK-TestnetMintableERC20-Polygon  │ '0xD9E7e5dd6e122dDE11244e14A60f38AbA93097f2' │
│ SUSHI-TestnetMintableERC20-Polygon  │ '0xdDc3C9B8614092e6188A86450c8D597509893E20' │
│  USDC-TestnetMintableERC20-Polygon  │ '0x9aa7fEc87CA69695Dd1f879567CcF49F3ba417E2' │
│  USDT-TestnetMintableERC20-Polygon  │ '0x21C561e551638401b937b03fE5a0a0652B99B7DD' │
│  WBTC-TestnetMintableERC20-Polygon  │ '0x85E44420b6137bbc75a85CAB5c9A3371af976FdE' │
│  WETH-TestnetMintableERC20-Polygon  │ '0xd575d4047f8c667E064a4ad433D04E25187F40BB' │
│ WMATIC-TestnetMintableERC20-Polygon │ '0xb685400156cF3CBE8725958DeAA61436727A30c3' │
└─────────────────────────────────────┴──────────────────────────────────────────────┘
```
{% endtab %}

{% tab title="Harmony" %}
```
4|harmony- | 
4|harmony- | Accounts after deployment
4|harmony- | ========
4|harmony- | ┌─────────┬──────────────────────────────────┬──────────────────────────────────────────────┬────────────────┐
4|harmony- | │ (index) │               name               │                   account                    │    balance     │
4|harmony- | ├─────────┼──────────────────────────────────┼──────────────────────────────────────────────┼────────────────┤
4|harmony- | │    0    │            'deployer'            │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '580.55078559' │
4|harmony- | │    1    │            'aclAdmin'            │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '580.55078559' │
4|harmony- | │    2    │         'emergencyAdmin'         │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '580.55078559' │
4|harmony- | │    3    │           'poolAdmin'            │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '580.55078559' │
4|harmony- | │    4    │ 'addressesProviderRegistryOwner' │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '580.55078559' │
4|harmony- | │    5    │       'treasuryProxyAdmin'       │ '0x04c94825C3e3539e0f2bB21d435302d08B2Dbd77' │  '9.99169423'  │
4|harmony- | │    6    │      'incentivesProxyAdmin'      │ '0x04c94825C3e3539e0f2bB21d435302d08B2Dbd77' │  '9.99169423'  │
4|harmony- | │    7    │   'incentivesEmissionManager'    │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '580.55078559' │
4|harmony- | │    8    │     'incentivesRewardsVault'     │ '0x77c45699A715A64A7a7796d5CEe884cf617D5254' │ '580.55078559' │
4|harmony- | └─────────┴──────────────────────────────────┴──────────────────────────────────────────────┴────────────────┘
4|harmony- | 
4|harmony- | Deployments
4|harmony- | ===========
4|harmony- | ┌─────────────────────────────────────────┬──────────────────────────────────────────────┐
4|harmony- | │                 (index)                 │                   address                    │
4|harmony- | ├─────────────────────────────────────────┼──────────────────────────────────────────────┤
4|harmony- | │      PoolAddressesProviderRegistry      │ '0x3111Aa37Dd484154A6BA4091Dfd282d9AeAfc64C' │
4|harmony- | │               SupplyLogic               │ '0x1B53dE3f67F5C845BdF5472917DFfE05234E15df' │
4|harmony- | │               BorrowLogic               │ '0xb685400156cF3CBE8725958DeAA61436727A30c3' │
4|harmony- | │            LiquidationLogic             │ '0x2a58E9bbb5434FdA7FF78051a4B82cb0EF669C17' │
4|harmony- | │               EModeLogic                │ '0x78baC31Ed73c115EB7067d1AfE75eC7B4e16Df9e' │
4|harmony- | │               BridgeLogic               │ '0xD4b6566313c1dCd8823226bb456d80fc85B03d8B' │
4|harmony- | │            ConfiguratorLogic            │ '0x94E9E8876Fd68574f17B2cd7Fa19AA8342fFaF51' │
4|harmony- | │             FlashLoanLogic              │ '0x9A753f0F7886C9fbF63cF59D0D4423C5eFaCE95B' │
4|harmony- | │                PoolLogic                │ '0xD9E7e5dd6e122dDE11244e14A60f38AbA93097f2' │
4|harmony- | │              TreasuryProxy              │ '0x9aa7fEc87CA69695Dd1f879567CcF49F3ba417E2' │
4|harmony- | │           Treasury-Controller           │ '0x85E44420b6137bbc75a85CAB5c9A3371af976FdE' │
4|harmony- | │         Treasury-Implementation         │ '0xd575d4047f8c667E064a4ad433D04E25187F40BB' │
4|harmony- | │               WETHGateway               │ '0xdDc3C9B8614092e6188A86450c8D597509893E20' │
4|harmony- | │          WalletBalanceProvider          │ '0x8AaF462990dD5CC574c94C8266208996426A47e7' │
4|harmony- | │        UiIncentiveDataProviderV3        │ '0xE3981f4840843D67aF50026d34DA0f7e56A02D69' │
4|harmony- | │          UiPoolDataProviderV3           │ '0x56e0507A53Ee252947a1E55D84Dc4032F914DD98' │
4|harmony- | │           ERC20Faucet-Harmony           │ '0x8f57153F18b7273f9A814b93b31Cb3f9b035e7C2' │
4|harmony- | │      PoolAddressesProvider-Harmony      │ '0xd19443202328A66875a51560c28276868B8C61C2' │
4|harmony- | │        PoolDataProvider-Harmony         │ '0xFc7215C9498Fc12b22Bc0ed335871Db4315f03d3' │
4|harmony- | │   WONE-TestnetPriceAggregator-Harmony   │ '0x3E937B4881CBd500d05EeDAB7BA203f2b7B3f74f' │
4|harmony- | │   DAI-TestnetPriceAggregator-Harmony    │ '0x09C85Ef96e93f0ae892561052B48AE9DB29F2458' │
4|harmony- | │   LINK-TestnetPriceAggregator-Harmony   │ '0x28A8E6e41F84e62284970E4bc0867cEe2AAd0DA4' │
4|harmony- | │   USDC-TestnetPriceAggregator-Harmony   │ '0xD90db1ca5A6e9873BCD9B0279AE038272b656728' │
4|harmony- | │   WBTC-TestnetPriceAggregator-Harmony   │ '0xCcbBaf8D40a5C34bf1c836e8dD33c7B7646706C5' │
4|harmony- | │   WETH-TestnetPriceAggregator-Harmony   │ '0x127277bF2F5fA186bfC6b3a0ca00baefB5472d3a' │
4|harmony- | │   USDT-TestnetPriceAggregator-Harmony   │ '0x1775ECC8362dB6CaB0c7A9C0957cF656A5276c29' │
4|harmony- | │   AAVE-TestnetPriceAggregator-Harmony   │ '0x99B70f90b76716D9f909AD91de7e7F44d3445da4' │
4|harmony- | │           Pool-Implementation           │ '0x36556E9b01BCcCF0017C4998D972614f751Adf14' │
4|harmony- | │     PoolConfigurator-Implementation     │ '0x8e0988b28f9CdDe0134A206dfF94111578498C63' │
4|harmony- | │           ReservesSetupHelper           │ '0x55E1267C2e587b6b5E94aD4f72E3eDA725D58b8D' │
4|harmony- | │           ACLManager-Harmony            │ '0x1758d4e6f68166C4B2d9d0F049F33dEB399Daa1F' │
4|harmony- | │           AaveOracle-Harmony            │ '0x29Ff3c19C6853A0b6544b3CC241c360f422aBaD1' │
4|harmony- | │         FallbackOracle-Harmony          │ '0x99b97EB5948eA9aFEC4808F250f4fe245b8b3c02' │
4|harmony- | │           Pool-Proxy-Harmony            │ '0x85C1F3f1bB439180f7Bfda9DFD61De82e10bD554' │
4|harmony- | │     PoolConfigurator-Proxy-Harmony      │ '0xdb903B5a28260E87cF1d8B56740a90Dba1c8fe15' │
4|harmony- | │             IncentivesProxy             │ '0xC05FAA52459226aA19eDF47DD858Ff137D41Ce84' │
4|harmony- | │       IncentivesV2-Implementation       │ '0x51b116B1Efb91c60D032540136f15E6989Cf1834' │
4|harmony- | │       PullRewardsTransferStrategy       │ '0xD8Fb78112f804fE2B172c3130B478eCA238eBcE3' │
4|harmony- | │             AToken-Harmony              │ '0x305486F040Ff6Cf7E09403fA6802dE362E91bBcE' │
4|harmony- | │      DelegationAwareAToken-Harmony      │ '0xF1bE881Ee7034ebC0CD47E1af1bA94EC30DF3583' │
4|harmony- | │         StableDebtToken-Harmony         │ '0x509B2506FbA1BD41765F6A82C7B0Dd4229191768' │
4|harmony- | │        VariableDebtToken-Harmony        │ '0x57dDbfeab5Dc552d33dC8cacCdB490de80431334' │
4|harmony- | │  ReserveStrategy-rateStrategyStableTwo  │ '0x335De793a66B839974aED2673b72a452c3Ee93A4' │
4|harmony- | │ ReserveStrategy-rateStrategyVolatileOne │ '0x47E83aeB8E1940aF16fF763F2c25ba75a1F4D0c5' │
4|harmony- | │  ReserveStrategy-rateStrategyStableOne  │ '0x58Cd851c28dF05Edc7F018B533C0257DE57673f7' │
4|harmony- | │           WONE-AToken-Harmony           │ '0xA6a1ec235B90e0b5567521F52e5418B9BA189334' │
4|harmony- | │     WONE-VariableDebtToken-Harmony      │ '0xB344989ff1717549221AF8525110421e4955857b' │
4|harmony- | │      WONE-StableDebtToken-Harmony       │ '0xdBb47093f92090Ec0E1B3CDC48fAFB52Ea185403' │
4|harmony- | │           DAI-AToken-Harmony            │ '0xF5C62a60A2065D34b601CAfF8775F5A2857A9088' │
4|harmony- | │      DAI-VariableDebtToken-Harmony      │ '0xDD81Dec96a2e4c5221fe11854a32F37C49C1a72A' │
4|harmony- | │       DAI-StableDebtToken-Harmony       │ '0x88d8a116C758C782985DAD67798666e270F0F1a8' │
4|harmony- | │           LINK-AToken-Harmony           │ '0xd5Bc03707A290BAaB91FeFBAf397Fe90EE48Cc39' │
4|harmony- | │     LINK-VariableDebtToken-Harmony      │ '0x2DE29943BbFA3740C1C3C9532E61e3489b2f742A' │
4|harmony- | │      LINK-StableDebtToken-Harmony       │ '0xE052c9c02cd4949832cAC20A91B8cf7C59cDd93b' │
4|harmony- | │           USDC-AToken-Harmony           │ '0xf58153a81DbC7118a8Ad128024996E68dcDEE8B2' │
4|harmony- | │     USDC-VariableDebtToken-Harmony      │ '0x6bA6869B3B16a2478EAc78010e4c0DB534Fd79F2' │
4|harmony- | │      USDC-StableDebtToken-Harmony       │ '0x7C50b2Fb765D77547B7a9F44364308FeEE7526D6' │
4|harmony- | │           WBTC-AToken-Harmony           │ '0x9D6a5051882C1DFA7d26Cb862a13843c1fe0EF0A' │
4|harmony- | │     WBTC-VariableDebtToken-Harmony      │ '0x4953fFBeD89EfE9DC6B4Fe51f74924D6A9b7Ce4e' │
4|harmony- | │      WBTC-StableDebtToken-Harmony       │ '0x478FE510965e607C95EB52c91FB711c8006483B9' │
4|harmony- | │           WETH-AToken-Harmony           │ '0x7916c8E4d5B3C998B7e8d94bEE3625D0996dA3CC' │
4|harmony- | │     WETH-VariableDebtToken-Harmony      │ '0x87c271682553fBe445331C872D991c463091f625' │
4|harmony- | │      WETH-StableDebtToken-Harmony       │ '0x348d1F7BC7FF6803AB96e51B846069Fc1F74F8E5' │
4|harmony- | │           USDT-AToken-Harmony           │ '0xAe8c5CfF5D96c36372378A4eFEBcaE78e3552AD9' │
4|harmony- | │     USDT-VariableDebtToken-Harmony      │ '0xAe2A7BCEF650E798c8911a375bDcec248acbeEC9' │
4|harmony- | │      USDT-StableDebtToken-Harmony       │ '0xd6D10CEfD2E8A94B5B4Bd3D7B3F2d1cE39c0508c' │
4|harmony- | │           AAVE-AToken-Harmony           │ '0xAf16e6F087bb99aEf830409228CCcf8B039C758D' │
4|harmony- | │     AAVE-VariableDebtToken-Harmony      │ '0x0F8801a7a8964EA79a504EBa454CbAfF793feED7' │
4|harmony- | │      AAVE-StableDebtToken-Harmony       │ '0xCd5327194e4e95C4AECf863904FA80a8522c7C97' │
4|harmony- | │          MockFlashLoanReceiver          │ '0x651b8A8cA545b251a8f49B57D5838Da0a8DFbEF9' │
4|harmony- | └─────────────────────────────────────────┴──────────────────────────────────────────────┘
4|harmony- | 
4|harmony- | Mintable Reserves and Rewards
4|harmony- | ┌───────────────────────────────────┬──────────────────────────────────────────────┐
4|harmony- | │              (index)              │                   address                    │
4|harmony- | ├───────────────────────────────────┼──────────────────────────────────────────────┤
4|harmony- | │ WONE-TestnetMintableERC20-Harmony │ '0x3e4b51076d7e9B844B92F8c6377087f9cf8C8696' │
4|harmony- | │ DAI-TestnetMintableERC20-Harmony  │ '0x302567472401C7c7B50ee7eb3418c375D8E3F728' │
4|harmony- | │ LINK-TestnetMintableERC20-Harmony │ '0xBaaCc99123133851Ba2D6d34952aa08CBDf5A4E4' │
4|harmony- | │ USDC-TestnetMintableERC20-Harmony │ '0xFCadBDefd30E11258559Ba239C8a5A8A8D28CB00' │
4|harmony- | │ WBTC-TestnetMintableERC20-Harmony │ '0xc1eB89DA925cc2Ae8B36818d26E12DDF8F8601b0' │
4|harmony- | │ WETH-TestnetMintableERC20-Harmony │ '0x5343b5bA672Ae99d627A1C87866b8E53F47Db2E6' │
4|harmony- | │ USDT-TestnetMintableERC20-Harmony │ '0x2A9534682aF7e07bA9615e15dd9d88968173F6c3' │
4|harmony- | │ AAVE-TestnetMintableERC20-Harmony │ '0x407287b03D1167593AF113d32093942be13A535f' │
4|harmony- | └───────────────────────────────────┴──────────────────────────────────────────────┘
```
{% endtab %}
{% endtabs %}

