# WalletBalanceProvider

## WalletBalanceProvider

Implements a logic of getting multiple tokens balance for one user address.

{% hint style="info" %}
For getting ETH (native chain token) balance use \`MOCK\_ETH\_ADDRESS = 0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE\`.
{% endhint %}

## Methods

### balanceOf

`function balanceOf(address user, address token)`

Returns the balance of the token for user (ETH included with `MOCK_ETH_ADDRESS`).

### batchBalanceOf

`function batchBalanceOf(address[] calldata users, address[] calldata tokens)`

Returns balances for a list of `users` and `tokens` (ETH included with `MOCK_ETH_ADDRESS`).

### getUserWalletBalances

`function getUserWalletBalances(address provider, address user)`

Provides balances of user wallet for all reserves available on the pool
