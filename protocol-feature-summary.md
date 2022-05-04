# Protocol Features

The Aave Protocol offers features which may only be available in select networks or market. The following guide gives a breakdown of each features availability across all Aave Protocol deployments.

## Page Contents

- [Network Feature Summary](#network-feature-summary)
    - [AAVE Staking](#staking)
    - [AAVE Governance](#on-chain-governance)
    - [Snapshot Voting](#snapshot-voting)
    - [Cross-Chain Governance Bridges](#cross-chain-governance-bridges)
- [Market Feature Summary](#market-feature-summary)
    - [Repay With Collateral](#repay-with-collateral)
    - [Collateral Swap](#collateral-swap)
    - [Repay with aTokens](#repay-with-atokens)


## Network Feature Summary

| Network     | AAVE Staking | Governance Voting | Cross-Chain Governance Execution | Snapshot Voting                            |
|-------------|--------------|-------------------|----------------------------------|--------------------------------------------|
| ETH Mainnet | Yes          | Yes               | ---                              | AAVE, aAAVE (V2 Market), stkAAVE, stkABPT  |
| Polygon     | No           | No                | Yes                              | AAVE, aAAVE (V2 Market)                    |
| Avalanche   | No           | No                | No                               | AAVE, aAAVE (V2 Market)                    |
| Arbitrum    | No           | No                | Yes                              | None                                       |
| Optimism    | No           | No                | No                               | None                                       |
| Fantom      | No           | No                | No                               | None                                       |
| Harmony     | No           | No                | No                               | None                                       |

## Market Feature Summary

| Market         | Repay With Collateral | Collateral Swap | Repay With aTokens |
|----------------|-----------------------|-----------------|--------------------|
| V2 ETH Mainnet | Yes                   | Yes             | No                 |
| V2 Polygon     | Yes                   | Yes             | No                 |
| V2 Avalanche   | Yes                   | Yes             | No                 |
| V3 Polygon     | Yes                   | Yes             | Yes                |
| V3 Avalanche   | Yes                   | Yes             | Yes                |
| V3 Arbitrum    | No                    | No              | Yes                |
| V3 Optimism    | No                    | No              | Yes                |
| V3 Fantom      | Yes                   | Yes             | Yes                |
| V3 Harmony     | No                    | No              | Yes                |

## Repay With Collateral
Allows borrowers to repay debt using the supplied liquidity in the protocol.

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
Staking option is only available on Ethereum Mainnet. Learn more about risks involved [here](https://docs.aave.com/faq/migration-and-staking)
{% endhint %}

## Snapshot Voting
The [Aave Snapshot Space](https://snapshot.org/#/aave.eth) is a place for voters to gauge community sentiment for on-chain votes, or decide off-chain proposals. Voting on Snapshot proposals is done via a gasless signature, and is available with a variety of assets and chains. A list of available voting strategies can be viewed [here](#network-feature-summary) or queried in realtime via this [GraphQL endpoint](https://hub.snapshot.org/graphql?query=%0Aquery%20Spaces%20%7B%0A%20%20spaces(%0A%20%20%20%20first%3A%2020%2C%0A%20%20%20%20skip%3A%200%2C%0A%20%20%20%20orderBy%3A%20%22created%22%2C%0A%20%20%20%20orderDirection%3A%20desc%2C%0A%20%20%20%20where%3A%20%7Bid%3A%20%22aave.eth%22%7D%0A%20%20)%20%7B%0A%20%20%20%20id%0A%20%20%20%20name%0A%20%20%20%20about%0A%20%20%20%20network%0A%20%20%20%20symbol%0A%20%20%20%20strategies%20%7B%0A%20%20%20%20%20%20name%0A%20%20%20%20%20%20network%0A%20%20%20%20%20%20params%0A%20%20%20%20%7D%0A%20%20%20%20admins%0A%20%20%20%20members%0A%20%20%20%20filters%20%7B%0A%20%20%20%20%20%20minScore%0A%20%20%20%20%20%20onlyMembers%0A%20%20%20%20%7D%0A%20%20%20%20plugins%0A%20%20%7D%0A%7D).


## On-Chain Governance
[Aave Governance](https://docs.aave.com/developers/v/2.0/protocol-governance/governance) allows holders of AAVE or stkAAVE to vote and propose changes/upgrades to the protocol and governance. The governance process is described [here](https://docs.aave.com/governance/) in more detail.

{% hint style="info" %}
Aave Governance is only enabled on Ethereum mainnet.
{% endhint %}

## Cross-Chain Governance Bridges
All voting for Aave Governance proposals occurs on Ethereum mainnet. Governance bridges can be used to take the result of proposal voting on Ethereum mainnet to execute proposals on other chains. [This repo](https://github.com/aave/governance-crosschain-bridges) contains the techinical implementation for cross-chain bridges. 

{% hint style="info" %}
Cross-chain bridges are currently available for the Polygon and Arbitrum networks.
{% endhint %}
