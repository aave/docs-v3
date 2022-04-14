# Siloed Borrowing

This feature allow assets with potentially manipulatable oracles (for example illiquid Uni V3 pairs) to be listed on Aave as supply-only assets (i.e. cannot be used as collateral) without having any risk of impacting the overall solvency of the protocol.

{% hint style="info" %}
[Risk or Pool admin](../core-contracts/aclmanager.md#roles), selected by Aave Governace, can call [`setSiloedBorrowing`](../core-contracts/poolconfigurator.md#setsiloedborrowing) to set an asset in _Siloed Borrowing_ mode.&#x20;
{% endhint %}

## Supply Siloed Assets

A user can supply S_iloed Asset_ just like any other asset using [`supply()`](../core-contracts/pool.md#supply) method in `pool.sol`, though, the asset will not be enabled to use as collateral i.e. supplied amount will not add to total collateral balance of the user.



### Borrow Siloed Assets

{% hint style="danger" %}
User borrowing a _siloed asset_ will _ **not**_ be allowed to __ borrow __ any other asset_._
{% endhint %}

User can borrow _Siloed Assets_ using [`borrow()`](../core-contracts/pool.md#borrow) method in `pool.sol` , only if:

* It is first borrow onBehalf of the address&#x20;

OR

* Existing user debt is of the same _siloed asset._

To check if user is in Siloed Borrowing state, you can see if underlying asset borrowed by user is s_iloed_ using  [`getSiloedBorrowing()`](https://app.gitbook.com/o/-M3C70U6u8KvfI48VhfC/s/yDw3RAf1nkR3haH9P6Zl/\~/changes/ShqJceNEVzojIUJTfByx/periphery-contracts/aaveprotocoldataprovider#getsiloedborrowing) method on AaveProtocolDataProvider.sol.

### Check if Reserved for Siloed Borrowing

```solidity
import {AaveProtocolDataProvider} from '@aave/core-v3/contracts/misc/AaveProtocolDataProvider.sol';
AaveProtocolDataProvider poolDataProvider = AaveProtocolDataProvider(provider.getPoolDataProvider());
// address of the underlying asset
address asset = "0x...";

protocolDataProvider.getSiloedBorrowing(asset);
```



### FAQ

#### How can user enter siloed borrowing state?

User automatically enters siloed borrowing state on their first successful borrow of siloed asset.

#### How does user exit siloed borrowing state?

User must repay all their debt to exist siloed borrowing state.



