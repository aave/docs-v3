# PoolAddressesProvider

Main registry of addresses part of or connected to the protocol for a particular market, including permissioned roles. This contract is owned by the Aave Governance, is immutable and the address will never change.

{% hint style="info" %}
Whenever the `Pool` contract is needed, we recommended you fetch the correct address from the `PoolAddressesProvider` smart contract.
{% endhint %}

The source code can be found [here](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/configuration/PoolAddressesProvider.sol).

## Write Methods

### setMarketId

```solidity
function setMarketId(string memory newMarketId) external override onlyOwner
```

Updates the identifier of the Aave market by associating an id with a specific PoolAddressesProvider. This can be used to create an onchain registry of PoolAddressesProviders to identify and validate multiple Aave markets.

#### Input Parameters:

| Name        | Type     | Description              |
| :---------- | :------- | :----------------------- |
| newMarketId | `string` | The new id of the market |

### setAddress

```solidity
function setAddress(bytes32 id, address newAddress) external override onlyOwner
```

Sets the address of protocol contract stored at the given id, replacing the address saved in the addresses map.

{% hint style="warning" %}
Use this function carefully, as it will do a hard replacement of the current address in the addresses map.
{% endhint %}

#### Input Parameters:

| Name        | Type      | Description                                              |
| :---------- | :-------- | :------------------------------------------------------- |
| id          | `bytes32` | keccak256 hash of UTF8Bytes string representing contract |
| newAddress  | `address` | The new address to be set corresponding to the `id`      |

For example, `utils.keccak256(utils.toUtf8Bytes("INCENTIVES_CONTROLLER"))`, is set to the address of `INCENTIVES_CONTROLLER`.

### setAddressAsProxy

```solidity
function setAddressAsProxy(bytes32 id, address newImplementationAddress) external override onlyOwner
```

Updates the implementation address of a proxy contract with a specified `id`. 

{% hint style="info" %}
If there is no proxy registered, it will instantiate one and set the implementation as the `newImplementationAddress`.
{% endhint %}

{% hint style="warning" %}
Use this function carefully, only for ids that don't have an explicit setter function in order to avoid unexpected consequences.
{% endhint %}

#### Input Parameters:

| Name                     | Type      | Description                                                           |
| :----------------------- | :-------- | :-------------------------------------------------------------------- |
| id                       | `bytes32` | The id of the proxy contract                                          |
| newImplementationAddress | `address` | The address of new implementation contract corresponding to the proxy |

### setPoolImpl

```solidity
function setPoolImpl(address newPoolImpl) external override onlyOwner 
```

Updates the implementation of the Pool, or creates a proxy.

#### Input Parameters:

| Name        | Type      | Description                                     |
| :---------- | :-------- | :---------------------------------------------- |
| newPoolImpl | `address` | The address of new Pool implementation contract |

### setPoolConfiguratorImpl

```solidity
function setPoolConfiguratorImpl(address newPoolConfiguratorImpl) external override onlyOwner
```

Updates the implementation of the PoolConfigurator, or creates a proxy.

#### Input Parameters:

| Name                    | Type      | Description                                                 |
| :---------------------- | :-------- | :---------------------------------------------------------- |
| newPoolConfiguratorImpl | `address` | The address of new PoolConfigurator implementation contract |

### setPriceOracle

```solidity
function setPriceOracle(address newPriceOracle) external override onlyOwner
```

Updates the address of the price oracle.

#### Input Parameters:

| Name           | Type      | Description                    |
| :------------- | :-------- | :----------------------------- |
| newPriceOracle | `address` | The address of new PriceOracle |

### setACLManager

```solidity
function setACLManager(address newAclManager) external override onlyOwner
```

Updates the address of the Access Control List Manager.

#### Input Parameters:

| Name          | Type      | Description                       |
| :------------ | :-------- | :-------------------------------- |
| newAclManager | `address` | The address of the new ACLManager |

### setACLAdmin

```solidity
function setACLAdmin(address newAclAdmin) external override onlyOwner 
```

Updates the address of the Access Control List Admin.

#### Input Parameters:

| Name        | Type      | Description                 |
| :---------- | :-------- | :-------------------------- |
| newAclAdmin | `address` | The address of new ACLAdmin |

### setPriceOracleSentinel

```solidity
function setPriceOracleSentinel(address newPriceOracleSentinel) external override onlyOwner
```

Updates the address of the price oracle sentinel.

#### Input Parameters:

| Name                   | Type      | Description                            |
| :--------------------- | :-------- | :------------------------------------- |
| newPriceOracleSentinel | `address` | The address of new PriceOracleSentinel |

### setPoolDataProvider

```solidity
function setPoolDataProvider(address newDataProvider) external override onlyOwner
```

Updates the address of the data provider.

#### Input Parameters:

| Name            | Type      | Description                     |
| :-------------- | :-------- | :------------------------------ |
| newDataProvider | `address` | The address of new DataProvider |

## View Methods

### getMarketId

```solidity
function getMarketId() external view override returns (string memory)
```

Returns the market id of the associated Aave market.

#### Return Values:

| Type     | Description                              |
| :------- | :--------------------------------------- |
| `string` | A string representation of the market id |

### getAddress

```solidity
function getAddress(bytes32 id) public view override returns (address)
```

Returns the address of protocol contract stored at the given id. The returned address might be an EOA or a contract, which may be proxied. It will return ZERO if there is no registered address with the given id.

#### Input Parameters:

| Name | Type      | Description                                                 |
| :--- | :-------- | :---------------------------------------------------------- |
| id   | `bytes32` | The id. For example, the Protocol Data Provider uses id 0x1 |

#### Return Values:

| Type      | Description                               |
| :-------- | :---------------------------------------- |
| `address` | The address associated with the id passed |

#### Example: 
```tsx
// Get address of incentive controller
import { utils } from '@ethers/lib/utils';

const id =  utils.keccak256(utils.toUtf8Bytes("INCENTIVES_CONTROLLER"));
const address = poolAddressProvider.getAddress(id);
```

### getPool

```solidity
function getPool() external view override returns (address)
```

Returns the address of the latest Pool proxy contract.

#### Return Values:

| Type      | Description                              |
| :-------- | :--------------------------------------- |
| `address` | The address of the associated Pool proxy |

### getPoolConfigurator

```solidity
function getPoolConfigurator() external view override returns (address)
```

Returns the address of the PoolConfigurator proxy. Used for configuration methods, like init reserves or update token implementation etc, of the market.

#### Return Values:

| Type      | Description                        |
| :-------- | :--------------------------------- |
| `address` | The PoolConfigurator proxy address |

### getPriceOracle

```solidity
function getPriceOracle() external view override returns (address)
```

Returns the address of the Price Oracle used by the market.

#### Return Values:

| Type      | Description                                                   |
| :-------- | :------------------------------------------------------------ |
| `address` | The address of the price oracle used by the associated market |

### getACLManager

```solidity
function getACLManager() external view override returns (address)
```

Returns the address of the Access Control List Manager (ACLManager) that manages the system role of the market.

#### Return Values:

| Type      | Description                                                                             |
| :-------- | :-------------------------------------------------------------------------------------- |
| `address` | The address of the ACLManger contract managing the system role of the associated market |

### getACLAdmin

```solidity
function getACLAdmin() external view override returns (address)
```

Returns the address of the Access Control List Admin (ACLAdmin) of the market which holds the `DEFAULT_ADMIN_ROLE` in ACLManager.

#### Return Values:

| Type      | Description                                                           |
| :-------- | :-------------------------------------------------------------------- |
| `address` | The address of the Access Control List admin of the associated market |

### getPriceOracleSentinel

```solidity
function getPriceOracleSentinel() external view override returns (address)
```

Returns the address of the price oracle sentinel.

#### Return Values:

| Type      | Description                                                     |
| :-------- | :-------------------------------------------------------------- |
| `address` | The address of the PriceOracleSentinel of the associated market |

### getPoolDataProvider

```solidity
function getPoolDataProvider() external view override returns (address)
```

Returns the address of latest pool data provider.

#### Return Values:

| Type      | Description                                                    |
| :-------- | :------------------------------------------------------------- |
| `address` | The address of the pool data provider of the associated market |

## ABI

<details>
<summary>PoolAddressesProvider ABI</summary>

```
[
    {
        "inputs": [
            {
                "internalType": "string",
                "name": "marketId",
                "type": "string"
            },
            {
                "internalType": "address",
                "name": "owner",
                "type": "address"
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
                "name": "oldAddress",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "newAddress",
                "type": "address"
            }
        ],
        "name": "ACLAdminUpdated",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "oldAddress",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "newAddress",
                "type": "address"
            }
        ],
        "name": "ACLManagerUpdated",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "bytes32",
                "name": "id",
                "type": "bytes32"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "oldAddress",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "newAddress",
                "type": "address"
            }
        ],
        "name": "AddressSet",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "bytes32",
                "name": "id",
                "type": "bytes32"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "proxyAddress",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "address",
                "name": "oldImplementationAddress",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "newImplementationAddress",
                "type": "address"
            }
        ],
        "name": "AddressSetAsProxy",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "string",
                "name": "oldMarketId",
                "type": "string"
            },
            {
                "indexed": true,
                "internalType": "string",
                "name": "newMarketId",
                "type": "string"
            }
        ],
        "name": "MarketIdSet",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "previousOwner",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "newOwner",
                "type": "address"
            }
        ],
        "name": "OwnershipTransferred",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "oldAddress",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "newAddress",
                "type": "address"
            }
        ],
        "name": "PoolConfiguratorUpdated",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "oldAddress",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "newAddress",
                "type": "address"
            }
        ],
        "name": "PoolDataProviderUpdated",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "oldAddress",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "newAddress",
                "type": "address"
            }
        ],
        "name": "PoolUpdated",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "oldAddress",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "newAddress",
                "type": "address"
            }
        ],
        "name": "PriceOracleSentinelUpdated",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "oldAddress",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "newAddress",
                "type": "address"
            }
        ],
        "name": "PriceOracleUpdated",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "bytes32",
                "name": "id",
                "type": "bytes32"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "proxyAddress",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "implementationAddress",
                "type": "address"
            }
        ],
        "name": "ProxyCreated",
        "type": "event"
    },
    {
        "inputs": [],
        "name": "getACLAdmin",
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
        "name": "getACLManager",
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
                "internalType": "bytes32",
                "name": "id",
                "type": "bytes32"
            }
        ],
        "name": "getAddress",
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
        "name": "getMarketId",
        "outputs": [
            {
                "internalType": "string",
                "name": "",
                "type": "string"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "getPool",
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
        "name": "getPoolConfigurator",
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
        "name": "getPoolDataProvider",
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
        "name": "getPriceOracle",
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
        "name": "getPriceOracleSentinel",
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
        "name": "owner",
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
        "name": "renounceOwnership",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "newAclAdmin",
                "type": "address"
            }
        ],
        "name": "setACLAdmin",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "newAclManager",
                "type": "address"
            }
        ],
        "name": "setACLManager",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "bytes32",
                "name": "id",
                "type": "bytes32"
            },
            {
                "internalType": "address",
                "name": "newAddress",
                "type": "address"
            }
        ],
        "name": "setAddress",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "bytes32",
                "name": "id",
                "type": "bytes32"
            },
            {
                "internalType": "address",
                "name": "newImplementationAddress",
                "type": "address"
            }
        ],
        "name": "setAddressAsProxy",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "string",
                "name": "newMarketId",
                "type": "string"
            }
        ],
        "name": "setMarketId",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "newPoolConfiguratorImpl",
                "type": "address"
            }
        ],
        "name": "setPoolConfiguratorImpl",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "newDataProvider",
                "type": "address"
            }
        ],
        "name": "setPoolDataProvider",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "newPoolImpl",
                "type": "address"
            }
        ],
        "name": "setPoolImpl",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "newPriceOracle",
                "type": "address"
            }
        ],
        "name": "setPriceOracle",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "newPriceOracleSentinel",
                "type": "address"
            }
        ],
        "name": "setPriceOracleSentinel",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "newOwner",
                "type": "address"
            }
        ],
        "name": "transferOwnership",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    }
]
```
</details>