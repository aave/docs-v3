# AaveOracle

## AaveOracle

Contract to get asset prices, manage price sources and update the fallback oracle.

Protocol V3 uses Chainlink Aggregators as the source of all asset prices.

{% hint style="warning" %}
The fallback oracles from V1 and V2 of the protocol are now deprecated and no longer maintained.
{% endhint %}

## Methods

### View

#### getAssetPrice

`function getAssetPrice(address asset)`

Returns the price of the supported `asset` in `BASE_CURRENCY` of the Aave Market in wei.

Return Value

| Type    | Description                                        |
| ------- | -------------------------------------------------- |
| uint256 | Price in BASE\_CURRENCY of the Aave market in wei. |

#### getAssetsPrices

`function getAssetsPrices(address[] calldata assets)`

Returns the prices of the supported `assets` in `BASE_CURRENCY` of the Aave Market. All prices are in wei.

Call Params

| Name   | Type       | Description                                                   |
| ------ | ---------- | ------------------------------------------------------------- |
| assets | address\[] | The addresses of the assets for which price is being queried. |

Return Value

| Type       | Description                                         |
| ---------- | --------------------------------------------------- |
| uint256\[] | Prices in BASE\_CURRENCY of the Aave market in wei. |

#### getSourceOfAsset

`function getSourceOfAsset(address asset)`

Returns the address of the price source for `asset`.

#### getFallbackOracle

`function getFallbackOracle()`

Returns the address of the fallback oracle.

### Write

#### setAssetSources

`function setAssetSources(address[] calldata assets, address[] calldata sources)`

Sets the price source for given list of assets.

{% hint style="danger" %}
This method can be called only by `POOL_ADMIN` or `ASSET_LISTING_ADMIN`. Check [ACLManager](aclmanager.md) for details on system roles.
{% endhint %}

Call Params

| Name    | Type       | Description                                                                                 |
| ------- | ---------- | ------------------------------------------------------------------------------------------- |
| assets  | address\[] | The addresses of the assets for which source is being set.                                  |
| sources | address\[] | The address of the source of each asset. Length of assets and sources array should be same. |

#### setFallbackOracle

`function setFallbackOracle(address fallbackOracle)`

Sets/updates the fallback oracle.

{% hint style="danger" %}
This method can be called only by `POOL_ADMIN`or`ASSET_LISTING_ADMIN`. Check [ACLManager](aclmanager.md) for details on system roles.
{% endhint %}

Call Params

| Name           | Type    | Description                         |
| -------------- | ------- | ----------------------------------- |
| fallbackOracle | address | The address of the fallback oracle. |
