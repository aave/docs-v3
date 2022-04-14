# PoolAddressesProvider

## PoolAddressesProvider

Addresses register of the protocol for a particular market. This contract is immutable and the address will never change.

{% hint style="info" %}
Whenever the \`Pool\` contract is needed, we recommended you fetch the correct address from the \`PoolAddressesProvider\` smart contract.
{% endhint %}

The source code can be [found on Github](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/configuration/PoolAddressesProvider.sol)

## View Methods

### getMarketId

`function getMarketId()`

Fetch the market id of the associated Aave market.

Return Values

| Type   | Description                           |
| ------ | ------------------------------------- |
| string | A string representation of the market |

### getAddress

`function getAddress(bytes32 id)`

Fetch the address of protocol contract stored at given id.

Call Params

| Name | Type    | Description                                         |
| ---- | ------- | --------------------------------------------------- |
| id   | bytes32 | id. Example, the Protocol Data Provider uses id 0x1 |

Return Values

| Type    | Description                                       |
| ------- | ------------------------------------------------- |
| address | The address associated with the bytes32 id passed |

### getPool

`function getPool()`

Fetch the contract of latest pool

Return Values

| Type    | Description                        |
| ------- | ---------------------------------- |
| address | The address of the associated Pool |

### getPoolConfigurator

`function getPoolConfigurator()`

Fetch the `PoolConfigurator` is used for configuration methods, like init reserves or update token implementation etc, of the market.

Return Value

| Type    | Description                                         |
| ------- | --------------------------------------------------- |
| address | The address of associated market’s PoolConfigurator |

### getPriceOracle

`function getPriceOracle()`

Fetch Price Oracle used by the market.

Return Value

| Type    | Description                                                |
| ------- | ---------------------------------------------------------- |
| address | The address of the price oracle used by associated market. |

### getACLManager

`function getACLManager()`

Fetch ACLManger that manages the system role of the market

Return Value

| Type    | Description                                                                              |
| ------- | ---------------------------------------------------------------------------------------- |
| address | The address of the ACLManger contract managing the system role of the associated market. |

### getACLAdmin

`function getACLAdmin()`

Fetch ACLAdmin of the market which holds the `DEFAULT_ADMIN_ROLE` in ACLManager.

Return Value

| Type    | Description                                                            |
| ------- | ---------------------------------------------------------------------- |
| address | The address of the access control list admin of the associated market. |

### getPriceOracleSentinel

`function getPriceOracleSentinel()`

Return Value

| Type    | Description                                                        |
| ------- | ------------------------------------------------------------------ |
| address | The address of the Price oracle sentinel of the associated market. |

### getPoolDataProvider

`function getPoolDataProvider()`\
``Fetch address of latest pool data provider.

Return Value

| Type    | Description                                                     |
| ------- | --------------------------------------------------------------- |
| address | The address of the pool data provider of the associated market. |

## Write Methods

### setMarketId

`function setMarketId(string memory newMarketId)`

### setAddress

`function setAddress(bytes32 id, address newAddress)`

### setAddressAsProxy

`function setAddressAsProxy(bytes32 id, address newImplementationAddress)`

### setPoolImpl

`function setPoolImpl(address newPoolImpl)`

### setPoolConfiguratorImp

`function setPoolConfiguratorImpl(address newPoolConfiguratorImpl)`

### setPriceOracle

`function setPriceOracle(address newPriceOracle)`

### setACLManager

`function setACLManager(address newAclManager)`

### setACLAdmin

`function setACLAdmin(address newAclAdmin)`

### setPriceOracleSentinel

`function setPriceOracleSentinel(address newPriceOracleSentinel)`

### setPoolDataProvider

`function setPoolDataProvider(address newDataProvider)`
