# Market Feature Summary

## Repay With Collateral
Allows borrowers to clear debt using the supplied liquidity in the protocol.

_Repay with collateral_ feature is enabled via [ParaSwapRepayAdapter](https://github.com/aave/aave-v3-periphery/blob/master/contracts/adapters/paraswap/ParaSwapRepayAdapter.sol). User must approve the contract to pull ATokens to successfully repay with collateral. 

{% hint style="info" %}
This feature does not support repaying with the same asset (eg. repaying USDC debt with USDC collateral). For repaying with same collateral asset see [Repay With ATokens feature](#repay-with-atokens).
{% endhint %}

### Supported Markets
Feature available on following **mainnet** markets:
- [V2 Ethereum Main](https://docs.aave.com/developers/v/2.0/deployed-contracts/deployed-contracts)
- [V2 Polygon](https://docs.aave.com/developers/v/2.0/deployed-contracts/matic-polygon-market)
- [V2 Avalanche](https://docs.aave.com/developers/v/2.0/deployed-contracts/avalanche-market)
- [V3 Polygon](./deployed-contracts/v3-mainnet/polygon.md)
- [V3 Avalanche](./deployed-contracts/v3-mainnet/avalanche.md)
- [V3 Fantom](./deployed-contracts/v3-mainnet/fantom.md)

## Collateral Swap
Allows the user to swap the supplied liquidity in one asset to another without separate withdraw and supply transaction. Eg. Swap _aUSDC_ to _aDAI_ in single transaction.

_Collateral swap_ feature is enabled via [ParaSwapLiquiditySwapAdapter](https://github.com/aave/aave-v3-periphery/blob/master/contracts/adapters/paraswap/ParaSwapLiquiditySwapAdapter.sol). User must approve the contract to pull ATokens in-order to successfully swap liquidity.

### Supported Markets
Feature available on following **mainnet** markets:
- [V2 Ethereum Main](https://docs.aave.com/developers/v/2.0/deployed-contracts/deployed-contracts)
- [V2 Polygon](https://docs.aave.com/developers/v/2.0/deployed-contracts/matic-polygon-market)
- [V2 Avalanche](https://docs.aave.com/developers/v/2.0/deployed-contracts/avalanche-market)
- [V3 Polygon](./deployed-contracts/v3-mainnet/polygon.md)
- [V3 Avalanche](./deployed-contracts/v3-mainnet/avalanche.md)
- [V3 Fantom](./deployed-contracts/v3-mainnet/fantom.md)

## Repay With ATokens
New feature added native to Aave protocol V3, which allows user to repay debt with the supplied liquidity of the same asset in the pool. Eg. repay _USDC_ debt with _aUSDC_. Details at [New Feature: repay with atoken](./whats-new/repay-with-atokens.md).

### Supported Markets
Feature available on all V3 **mainnet** and **testnet** markets:
- [V3 Polygon](./deployed-contracts/v3-mainnet/polygon.md)
- [V3 Avalanche](./deployed-contracts/v3-mainnet/avalanche.md)
- [V3 Fantom](./deployed-contracts/v3-mainnet/fantom.md)
- [V3 Harmony](./deployed-contracts/v3-mainnet/harmony.md)
- [V3 Optimism](./deployed-contracts/v3-mainnet/optimism.md)
- [V3 Arbitrum](./deployed-contracts/v3-mainnet/arbitrum.md)

## Staking
AAVE or ABPT [Aave Balancer Pool Token](https://pools.balancer.exchange/#/pool/0xc697051d1c6296c24ae3bcef39aca743861d9a81/about) holder can stake their AAVE or ABPT in the Safety Module to add more security to the protocol and earn Safety Incentives. In case of a shortfall event, up to 30% of your stake can be slashed to cover the deficit, providing an additional layer of protection for the protocol.

{% hint style="info" %}
Staking option is only available on Ethereum Mainnet. Learn more about risks involved in [here](https://docs.aave.com/faq/migration-and-staking)
{% endhint %}

## Snapshot Voting, On-Chain Governance
[Aave Governance](https://docs.aave.com/developers/v/2.0/protocol-governance/governance) allows holders of AAVE to vote and propose changes/upgrades to the protocol and governance.
The [Aave Snapshot Space](https://snapshot.org/#/aave.eth) is a place for voters to gauge community sentiment for on-chain votes, or decide off-chain proposals.

### Supported Markets

- [V2 Ethereum Main](https://docs.aave.com/developers/v/2.0/deployed-contracts/deployed-contracts)
- [V2 Ethereum AMM](https://docs.aave.com/developers/v/2.0/deployed-contracts/amm-market)