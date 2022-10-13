# AaveOracle

Contract to get asset prices, manage price sources and update the [fallback oracle]().

Protocol V3 uses [Chainlink Aggregators]() as the source of all asset prices.

{% hint style="warning" %}
The fallback oracles from V1 and V2 of the protocol are now deprecated and no longer maintained.
{% endhint %}

The source code is available on [GitHub](https://github.com/aave/aave-v3-core/blob/master/contracts/misc/AaveOracle.sol).

## Write Methods

### setAssetSources

```solidity
function setAssetSources(address[] calldata assets, address[] calldata sources) external override onlyAssetListingOrPoolAdmins
```

Sets the price `sources` for given list of `assets`.

{% hint style="danger" %}
This method can be called only by [POOL_ADMIN](../protocol/configuration/aclmanager.md#roles) or [ASSET_LISTING_ADMIN](../protocol/configuration/aclmanager.md#roles). Please look at the [ACLManager](../protocol/configuration/aclmanager.md) contract for further details on system roles.
{% endhint %}

#### Input Parameters:

| Name    | Type        | Description                                                                                |
| :------ | :---------- | :----------------------------------------------------------------------------------------- |
| assets  | `address[]` | The addresses of the assets for which source is being set                                  |
| sources | `address[]` | The address of the source of each asset. Length of assets and sources array should be same |

### setFallbackOracle

```solidity
function setFallbackOracle(address fallbackOracle) external override onlyAssetListingOrPoolAdmins
```

Sets/updates the `fallbackOracle`.

{% hint style="danger" %}
This method can be called only by [POOL_ADMIN](../protocol/configuration/aclmanager.md#roles) or [ASSET_LISTING_ADMIN](../protocol/configuration/aclmanager.md#roles). Please look at the [ACLManager](../protocol/configuration/aclmanager.md) contract for further details on system roles.
{% endhint %}

#### Input Parameters:

| Name           | Type      | Description                        |
| :------------- | :-------- | :--------------------------------- |
| fallbackOracle | `address` | The address of the fallback oracle |

## View Methods

### getAssetPrice

```solidity
function getAssetPrice(address asset) public view override returns (uint256) 
```

Returns the price of the supported `asset` in [BASE_CURRENCY](https://github.com/aave/aave-v3-core/blob/master/contracts/interfaces/IPriceOracleGetter.sol#L15) of the Aave Market in wei.

#### Input Parameters:

| Name    | Type      | Description              |
| :------ | :-------- | :----------------------- |
| asset   | `address` | The address of the asset |

#### Return Values:

| Type      | Description                                                         |
| :-------- | :------------------------------------------------------------------ |
| `uint256` | The price of the asset in `BASE_CURRENCY` of the Aave market in wei |

### getAssetsPrices

```solidity
function getAssetsPrices(address[] calldata assets) external view override returns (uint256[] memory)
```

Returns a list of prices from a list of the supported `assets` addresses in [BASE_CURRENCY](https://github.com/aave/aave-v3-core/blob/master/contracts/interfaces/IPriceOracleGetter.sol#L15) of the Aave Market. All prices are in wei.

#### Input Parameters:

| Name   | Type        | Description                                                   |
| :----- | :---------- | :------------------------------------------------------------ |
| assets | `address[]` | The list of assets addresses for which price is being queried |

#### Return Values:

| Type        | Description                                                                 |
| :---------- | :-------------------------------------------------------------------------- |
| `uint256[]` | The prices of the given assets in `BASE_CURRENCY` of the Aave market in wei |

### getSourceOfAsset

```solidity
function getSourceOfAsset(address asset) external view override returns (address)
```

Returns the address of the price source for an `asset` address.

#### Input Parameters:

| Name  | Type      | Description              |
| :---- | :-------- | :----------------------- |
| asset | `address` | The address of the asset |

#### Return Values:

| Type      | Description               |
| :-------- | :------------------------ |
| `address` | The address of the source |

### getFallbackOracle

```solidity
function getFallbackOracle() external view returns (address)
```

Returns the `address` of the fallback oracle.

#### Return Values:

| Type      | Description                        |
| :-------- | :--------------------------------- |
| `address` | The address of the fallback oracle |

## ABI
<details>
<summary>AaveOracle ABI</summary>

```
[
    {
        "inputs": [
            {
                "internalType": "contract IPoolAddressesProvider",
                "name": "provider",
                "type": "address"
            },
            {
                "internalType": "address[]",
                "name": "assets",
                "type": "address[]"
            },
            {
                "internalType": "address[]",
                "name": "sources",
                "type": "address[]"
            },
            {
                "internalType": "address",
                "name": "fallbackOracle",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "baseCurrency",
                "type": "address"
            },
            {
                "internalType": "uint256",
                "name": "baseCurrencyUnit",
                "type": "uint256"
            }
        ],
        "stateMutability": "nonpayable",
        "type": "constructor"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "asset",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "source",
                "type": "address"
            }
        ],
        "name": "AssetSourceUpdated",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "baseCurrency",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "baseCurrencyUnit",
                "type": "uint256"
            }
        ],
        "name": "BaseCurrencySet",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "fallbackOracle",
                "type": "address"
            }
        ],
        "name": "FallbackOracleUpdated",
        "type": "event"
    },
    {
        "inputs": [],
        "name": "ADDRESSES_PROVIDER",
        "outputs": [
            {
                "internalType": "contract IPoolAddressesProvider",
                "name": "",
                "type": "address"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "BASE_CURRENCY",
        "outputs": [
            {
                "internalType": "address",
                "name": "",
                "type": "address"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "BASE_CURRENCY_UNIT",
        "outputs": [
            {
                "internalType": "uint256",
                "name": "",
                "type": "uint256"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "asset",
                "type": "address"
            }
        ],
        "name": "getAssetPrice",
        "outputs": [
            {
                "internalType": "uint256",
                "name": "",
                "type": "uint256"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address[]",
                "name": "assets",
                "type": "address[]"
            }
        ],
        "name": "getAssetsPrices",
        "outputs": [
            {
                "internalType": "uint256[]",
                "name": "",
                "type": "uint256[]"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "getFallbackOracle",
        "outputs": [
            {
                "internalType": "address",
                "name": "",
                "type": "address"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "asset",
                "type": "address"
            }
        ],
        "name": "getSourceOfAsset",
        "outputs": [
            {
                "internalType": "address",
                "name": "",
                "type": "address"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address[]",
                "name": "assets",
                "type": "address[]"
            },
            {
                "internalType": "address[]",
                "name": "sources",
                "type": "address[]"
            }
        ],
        "name": "setAssetSources",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "fallbackOracle",
                "type": "address"
            }
        ],
        "name": "setFallbackOracle",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    }
]
```
</details>