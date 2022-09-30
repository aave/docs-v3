# DelegationAwareAToken

The special type of aToken that are minted and burned upon supply and withdraw of assets that has voting power associated _(which can be delegated)_ with them. These _aTokens_ are enabled to delegate voting power of the underlying asset to a different address.

_DelegationAwareAToken_ enables/implements all the methods available for aTokens with the additional [`delegateUnderlyingTo`](delegationawareatoken.md#delegateunderlyingto) method.

Refer to [aToken](atoken.md) docs for the full list of the contract api.

### delegateUnderlyingTo

```solidity
function delegateUnderlyingTo(address delegatee) external onlyPoolAdmin
```

Delegates voting power of the underlying asset to a `delegatee` address.

{% hint style="danger" %}
This method can only be called by `POOL_ADMIN.`
{% endhint %}

#### Input Parameters:

| Name      | Type      | Description                                         |
| :-------- | :-------- | :-------------------------------------------------- |
| delegatee | `address` | The address of the user to delegate voting power to |