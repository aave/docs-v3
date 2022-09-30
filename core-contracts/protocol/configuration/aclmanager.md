# ACLManager

Access Control List Manager (ACLManager) is the main registry of system roles and permissions.

ACLManager allows a _Role Admin_ to manage roles. _Role Admin_ is itself a role that is managed by the `DEFAULT_ADMIN_ROLE`.

`DEFAULT_ADMIN_ROLE` is held by the _ACLAdmin,_ and should be initialized in the `PoolAddressesProvider` beforehand.

{% hint style="info" %}
On Ethereum chain `PoolAddressesProvider`, is owned by Aave Governance. In networks other than Ethereum, either the *Crosschain Governance Bridges* or *Community Multisigs* are used to manage the `PoolAddressesProvider`.
{% endhint %}

## Roles

Below we outline the responsibilities/powers of the roles and the specific methods that are only accessible to the holders of these roles.

| Role                  | Responsibilities / Powers                                                                                                                                                                                                                                       | Methods Accessible                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| :-------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `ACL_ADMIN`           | Manage the role admins in the ACLManager                                                                                                                                                                                                                        | <ul><li>setRoleAdmin</li><li>addPoolAdmin</li><li>removePoolAdmin</li><li>addEmergencyAdmin</li><li>removeEmergencyAdmin</li><li>addRiskAdmin</li><li>removeRiskAdmin</li><li>addFlashBorrower</li><li>removeFlashBorrower</li><li>addBridge</li><li>removeBridge</li><li>addAssetListingAdmin</li><li>removeAssetListingAdmin</li>                                                                                                                                                      |
| `POOL_ADMIN`          | <p>Can:</p><ul><li>update token implementations</li><li>drop reserves</li><li>pause/unpause reserves</li><li>activate/deactivate reserves</li><li>update premiums</li><li>do all the things available to RISK_ADMIN and ASSET_LISTING_ADMIN</li></ul>           | </p> <ul><li>All methods available to `RISK_ADMIN`</li><li>All methods available to `ASSET_LISTING_ADMIN`</li><li>dropReserve</li><li>updateAToken</li><li>updateStableDebtToken</li><li>updateVariableDebtToken</li><li>setReserveActive</li><li>updateBridgeProtocolFee</li><li>updateFlashloanPremiumTotal</li><li>updateFlashloanPremiumToProtocol</li></ul>                                                                                                                         |
| `EMERGENCY_ADMIN`     | Can pause/unpause the pool or individual reserve                                                                                                                                                                                                                | setPoolPause                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| `RISK_ADMIN`          | <p>Can update:</p><ul><li>grace period of Oracle Sentinels</li><li>reserve params</li><li>unbacked mint cap</li><li>liquidation protocol fee</li><li>existing eMode categories and create new. (not category 0)</li><li>add/remove asset in silo mode</li></ul> | <ul><li>setGracePeriod</li><li>setReserveBorrowing</li><li>configureReserveAsCollateral</li><li>setReserveStableRateBorrowing</li><li>setReserveFreeze</li><li>setBorrowableInIsolation</li><li>setReserveFactor</li><li>setDebtCeiling</li><li>setBorrowCap</li><li>setSupplyCap</li><li>setLiquidationProtocolFee</li><li>setEModeCategory</li><li>setAssetEModeCategory</li><li>setUnbackedMintCap</li><li>setReserveInterestRateStrategyAddress</li><li>setSiloedBorrowing</li></ul> |
| `FLASH_BORROWER`      | <p>Flash loan premium is waived for the holders of this role.</p><p></p><p> Does not include flashLoanSimple</p>                                                                                                                                                | <ul><li>flashLoan</ul></li> |
| `BRIDGE`              | Can leverage the Portal feature                                                                                                                                                                                                                                 | <ul><li>mintUnbacked</li> <li>backUnbacked</ul></li>|
| `ASSET_LISTING_ADMIN` | <p>Can update: <ul> <li> asset oracle sources</li> <li>fallback oracle</li> <li>add new assets to the Aave market</li> </ul> </p>                                                                                                                               | <ul> <li> setAssetSources </li> <li> setFallbackOracle</li> <li>initReserves</li> </ul> |

## Write Methods

### setRoleAdmin

```solidity
function setRoleAdmin(bytes32 role, bytes32 adminRole) external override onlyRole(DEFAULT_ADMIN_ROLE)
```

Set the `role` as admin of a specific role. By default the `adminRole` for all roles is `DEFAULT_ADMIN_ROLE`.

{% hint style="info" %}
This method can only be called by an address with `DEFAULT_ADMIN_ROLE`.
{% endhint %}

#### Input Parameters:

| Name      | Type      | Description                                                                                                                                                                                                                        |
| :-------- | :-------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| role      | `bytes32` | <p>The role to be managed by the admin role - keccak256 hash of one of the following:</p><ul><li>POOL_ADMIN</li><li>EMERGENCY_ADMIN</li><li>RISK_ADMIN</li><li>FLASH_BORROWER</li><li>BRIDGE</li><li>ASSET_LISTING_ADMIN</li></ul> |
| adminRole | `bytes32` | The admin role. 0x00 is reserved for the `DEFAULT_ADMIN_ROLE`                                                                                                                                                                      |

### addPoolAdmin

```solidity
function addPoolAdmin(address admin) external override
```

Adds a new admin as PoolAdmin. The address is added to the list of members in `POOL_ADMIN` role. Holders of this role can update token implementations, drop, (un)pause and (de)activate reserves, update premiums and do everything the `ASSET_LISTING_ADMIN` and `RISK_ADMIN` can do.

{% hint style="info" %}
Can be called only by _Role Admin_, specified by _Aave Governance_, responsible for managing `POOL_ADMIN` role.
{% endhint %}

#### Input Parameters:

| Name  | Type      | Description                                         |
| :---- | :-------- | :-------------------------------------------------- |
| admin | `address` | The address which will be granted `POOL_ADMIN` role |

### removePoolAdmin

```solidity
function removePoolAdmin(address admin) external override
```

Removes an admin as PoolAdmin. The given address is removed from the list of members in `POOL_ADMIN` role.

{% hint style="info" %}
Can be called only by _Role Admin_, specified by _Aave Governance_, responsible for managing `POOL_ADMIN` role.
{% endhint %}

#### Input Parameters:

| Name  | Type      | Description                                                         |
| :---- | :-------- | :------------------------------------------------------------------ |
| admin | `address` | The address for which `POOL_ADMIN` role permissions will be removed |

### addEmergencyAdmin

```solidity
function addEmergencyAdmin(address admin) external override
```

Adds a new admin as an EmergencyAdmin. The address is added to the list of members in `EMERGENCY_ADMIN` role. Holders of this role can pause and unpause the pool or an individual reserve.

{% hint style="info" %}
Can be called only by _Role Admin_, specified by _Aave Governance_, responsible for managing `EMERGENCY_ADMIN` role.
{% endhint %}

#### Input Parameters:

| Name  | Type      | Description                                              |
| :---- | :-------- | :------------------------------------------------------- |
| admin | `address` | The address which will be granted `EMERGENCY_ADMIN` role |

### removeEmergencyAdmin

```solidity  
function removeEmergencyAdmin(address admin) external override
```

Removes an admin as EmergencyAdmin. The given address is removed from the list of members in `EMERGENCY_ADMIN` role.

{% hint style="info" %}
Can be called only by _Role Admin_, specified by _Aave Governance_, responsible for managing `EMERGENCY_ADMIN` role.
{% endhint %}

#### Input Parameters:

| Name  | Type      | Description                                                              |
| :---- | :-------- | :----------------------------------------------------------------------- |
| admin | `address` | The address for which `EMERGENCY_ADMIN` role permissions will be removed |

### addRiskAdmin

```solidity
function addRiskAdmin(address admin) external override
```

Adds a new admin as a RiskAdmin. The address is added to the list of members in `RISK_ADMIN` role. Holders of this role can update grace period of Oracle Sentinels, reserve params, unbacked mint cap, liquidation fee and eMode categories.

#### Input Parameters:

| Name  | Type      | Description                                         |
| :---- | :-------- | :-------------------------------------------------- |
| admin | `address` | The address which will be granted `RISK_ADMIN` role |

### removeRiskAdmin

```solidity
function removeRiskAdmin(address admin) external override
```

Removes an admin as RiskAdmin. The given address is removed from the list of members in `RISK_ADMIN` role.

#### Input Parameters:

| Name  | Type      | Description                                                         |
| :---- | :-------- | :------------------------------------------------------------------ |
| admin | `address` | The address for which `RISK_ADMIN` role permissions will be removed |

### addFlashBorrower

```solidity
function addFlashBorrower(address borrower) external override
```

Adds a new address as FlashBorrower. The address is added to the list of members in `FLASH_BORROWER` role. Holders of this role do not pay premium for flash loan (does not apply to `flashLoanSimple`).

#### Input Parameters:

| Name     | Type      | Description                                             |
| :------- | :-------- | :------------------------------------------------------ |
| borrower | `address` | The address which will be granted `FLASH_BORROWER` role |

### removeFlashBorrower

```solidity
function removeFlashBorrower(address borrower) external override
```

Removes an admin as FlashBorrower. The given address is removed from the list of members in `FLASH_BORROWER` role.

#### Input Parameters:

| Name     | Type      | Description                                                             |
| :------- | :-------- | :---------------------------------------------------------------------- |
| borrower | `address` | The address for which `FLASH_BORROWER` role permissions will be removed |

### addBridge

```solidity
function addBridge(address bridge) external override
```

Adds a new address as Bridge. The contract address is added to the list of bridges. Holders of this role can leverage the Portal feature to seamlessly move supplied assets across Aave V3 markets on different networks.

{% hint style="info" %}
Can be called only by _Role Admin_, specified by _Aave Governance_, responsible for managing `BRIDGE` role.
{% endhint %}

#### Input Parameters:

| Name   | Type      | Description                                     |
| :----- | :-------- | :---------------------------------------------- |
| bridge | `address` | The address which will be granted `BRIDGE` role |

### removeBridge

```solidity
function removeBridge(address bridge) external override
```

Removes an address as Bridge. The given contract address is removed from the list of bridges.

{% hint style="info" %}
Can be called only by _Role Admin_, specified by _Aave Governance_, responsible for managing `BRIDGE` role.
{% endhint %}

#### Input Parameters:

| Name     | Type      | Description                                                     |
| :------- | :-------- | :-------------------------------------------------------------- |
| borrower | `address` | The address for which `BRIDGE` role permissions will be removed |

### addAssetListingAdmin

```solidity
function addAssetListingAdmin(address admin) external override
```

Adds a new admin as AssetListingAdmin. The address is added to the list of member in `ASSET_LISTING_ADMIN` role. Holder of this role can update oracles & add new asset to the Aave market.

#### Input Parameters:

| Name  | Type      | Description                                                  |
| :---- | :-------- | :----------------------------------------------------------- |
| admin | `address` | The address which will be granted `ASSET_LISTING_ADMIN` role |

### removeAssetListingAdmin

```solidity
function removeAssetListingAdmin(address admin) external override
```

Removes an admin as AssetListingAdmin. The given address is removed from the list of members in `ASSET_LISTING_ADMIN` role.

#### Input Parameters:

| Name  | Type      | Description                                                                  |
| :---- | :-------- | :--------------------------------------------------------------------------- |
| admin | `address` | The address for which `ASSET_LISTING_ADMIN` role permissions will be removed |

## View Methods

### isPoolAdmin

```solidity
function isPoolAdmin(address admin) external view override returns (bool)
```

Returns `true` if the address has `POOL_ADMIN` role, false otherwise.

#### Input Parameters:

| Name  | Type      | Description          |
| :---- | :-------- | :------------------- |
| admin | `address` | The address to check |

#### Return Values:

| Type   | Description                                             |
| :----- | :------------------------------------------------------ |
| `bool` | True if the given address is PoolAdmin, false otherwise |

### isEmergencyAdmin

```solidity
function isEmergencyAdmin(address admin) external view override returns (bool)
```

Returns `true` if the address has `EMERGENCY_ADMIN` role, false otherwise.

#### Input Parameters:

| Name  | Type      | Description          |
| :---- | :-------- | :------------------- |
| admin | `address` | The address to check |

#### Return Values:

| Type   | Description                                                  |
| :----- | :----------------------------------------------------------- |
| `bool` | True if the given address is EmergencyAdmin, false otherwise |

### isRiskAdmin

```solidity
function isRiskAdmin(address admin) external view override returns (bool)
```

Returns `true` if the address has `RISK_ADMIN` role, false otherwise.

#### Input Parameters:

| Name  | Type      | Description          |
| :---- | :-------- | :------------------- |
| admin | `address` | The address to check |

#### Return Values:

| Type   | Description                                             |
| :----- | :------------------------------------------------------ |
| `bool` | True if the given address is RiskAdmin, false otherwise |

### isFlashBorrower

```solidity
function isFlashBorrower(address borrower) external view override returns (bool)
```

Returns `true` if the address has `FLASH_BORROWER` role, false otherwise.

#### Input Parameters:

| Name  | Type      | Description          |
| :---- | :-------- | :------------------- |
| admin | `address` | The address to check |

#### Return Values:

| Type   | Description                                                 |
| :----- | :---------------------------------------------------------- |
| `bool` | True if the given address is FlashBorrower, false otherwise |

### isBridge

```solidity
function isBridge(address bridge) external view override returns (bool)
```

Returns `true` if the address has `BRIDGE` role, false otherwise.

#### Input Parameters:

| Name  | Type      | Description          |
| :---- | :-------- | :------------------- |
| admin | `address` | The address to check |

#### Return Values:

| Type   | Description                                          |
| :----- | :--------------------------------------------------- |
| `bool` | True if the given address is Bridge, false otherwise |

### isAssetListingAdmin

```solidity
function isAssetListingAdmin(address admin) external view override returns (bool)
```

Returns `true` if the address has `ASSET_LISTING_ADMIN` role, false otherwise.

#### Input Parameters:

| Name  | Type      | Description          |
| :---- | :-------- | :------------------- |
| admin | `address` | The address to check |

#### Return Values:

| Type   | Description                                                     |
| :----- | :-------------------------------------------------------------- |
| `bool` | True if the given address is AssetListingAdmin, false otherwise |

## ABI

<details>
<summary>ACLManager ABI</summary>

```
[
    {
        "inputs": [
            {
                "internalType": "contract IPoolAddressesProvider",
                "name": "provider",
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
                "internalType": "bytes32",
                "name": "role",
                "type": "bytes32"
            },
            {
                "indexed": true,
                "internalType": "bytes32",
                "name": "previousAdminRole",
                "type": "bytes32"
            },
            {
                "indexed": true,
                "internalType": "bytes32",
                "name": "newAdminRole",
                "type": "bytes32"
            }
        ],
        "name": "RoleAdminChanged",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "bytes32",
                "name": "role",
                "type": "bytes32"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "account",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "sender",
                "type": "address"
            }
        ],
        "name": "RoleGranted",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "bytes32",
                "name": "role",
                "type": "bytes32"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "account",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "sender",
                "type": "address"
            }
        ],
        "name": "RoleRevoked",
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
        "name": "ASSET_LISTING_ADMIN_ROLE",
        "outputs": [
            {
                "internalType": "bytes32",
                "name": "",
                "type": "bytes32"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "BRIDGE_ROLE",
        "outputs": [
            {
                "internalType": "bytes32",
                "name": "",
                "type": "bytes32"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "DEFAULT_ADMIN_ROLE",
        "outputs": [
            {
                "internalType": "bytes32",
                "name": "",
                "type": "bytes32"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "EMERGENCY_ADMIN_ROLE",
        "outputs": [
            {
                "internalType": "bytes32",
                "name": "",
                "type": "bytes32"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "FLASH_BORROWER_ROLE",
        "outputs": [
            {
                "internalType": "bytes32",
                "name": "",
                "type": "bytes32"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "POOL_ADMIN_ROLE",
        "outputs": [
            {
                "internalType": "bytes32",
                "name": "",
                "type": "bytes32"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "RISK_ADMIN_ROLE",
        "outputs": [
            {
                "internalType": "bytes32",
                "name": "",
                "type": "bytes32"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "admin",
                "type": "address"
            }
        ],
        "name": "addAssetListingAdmin",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "bridge",
                "type": "address"
            }
        ],
        "name": "addBridge",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "admin",
                "type": "address"
            }
        ],
        "name": "addEmergencyAdmin",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "borrower",
                "type": "address"
            }
        ],
        "name": "addFlashBorrower",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "admin",
                "type": "address"
            }
        ],
        "name": "addPoolAdmin",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "admin",
                "type": "address"
            }
        ],
        "name": "addRiskAdmin",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "bytes32",
                "name": "role",
                "type": "bytes32"
            }
        ],
        "name": "getRoleAdmin",
        "outputs": [
            {
                "internalType": "bytes32",
                "name": "",
                "type": "bytes32"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "bytes32",
                "name": "role",
                "type": "bytes32"
            },
            {
                "internalType": "address",
                "name": "account",
                "type": "address"
            }
        ],
        "name": "grantRole",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "bytes32",
                "name": "role",
                "type": "bytes32"
            },
            {
                "internalType": "address",
                "name": "account",
                "type": "address"
            }
        ],
        "name": "hasRole",
        "outputs": [
            {
                "internalType": "bool",
                "name": "",
                "type": "bool"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "admin",
                "type": "address"
            }
        ],
        "name": "isAssetListingAdmin",
        "outputs": [
            {
                "internalType": "bool",
                "name": "",
                "type": "bool"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "bridge",
                "type": "address"
            }
        ],
        "name": "isBridge",
        "outputs": [
            {
                "internalType": "bool",
                "name": "",
                "type": "bool"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "admin",
                "type": "address"
            }
        ],
        "name": "isEmergencyAdmin",
        "outputs": [
            {
                "internalType": "bool",
                "name": "",
                "type": "bool"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "borrower",
                "type": "address"
            }
        ],
        "name": "isFlashBorrower",
        "outputs": [
            {
                "internalType": "bool",
                "name": "",
                "type": "bool"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "admin",
                "type": "address"
            }
        ],
        "name": "isPoolAdmin",
        "outputs": [
            {
                "internalType": "bool",
                "name": "",
                "type": "bool"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "admin",
                "type": "address"
            }
        ],
        "name": "isRiskAdmin",
        "outputs": [
            {
                "internalType": "bool",
                "name": "",
                "type": "bool"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "admin",
                "type": "address"
            }
        ],
        "name": "removeAssetListingAdmin",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "bridge",
                "type": "address"
            }
        ],
        "name": "removeBridge",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "admin",
                "type": "address"
            }
        ],
        "name": "removeEmergencyAdmin",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "borrower",
                "type": "address"
            }
        ],
        "name": "removeFlashBorrower",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "admin",
                "type": "address"
            }
        ],
        "name": "removePoolAdmin",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "admin",
                "type": "address"
            }
        ],
        "name": "removeRiskAdmin",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "bytes32",
                "name": "role",
                "type": "bytes32"
            },
            {
                "internalType": "address",
                "name": "account",
                "type": "address"
            }
        ],
        "name": "renounceRole",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "bytes32",
                "name": "role",
                "type": "bytes32"
            },
            {
                "internalType": "address",
                "name": "account",
                "type": "address"
            }
        ],
        "name": "revokeRole",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "bytes32",
                "name": "role",
                "type": "bytes32"
            },
            {
                "internalType": "bytes32",
                "name": "adminRole",
                "type": "bytes32"
            }
        ],
        "name": "setRoleAdmin",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "bytes4",
                "name": "interfaceId",
                "type": "bytes4"
            }
        ],
        "name": "supportsInterface",
        "outputs": [
            {
                "internalType": "bool",
                "name": "",
                "type": "bool"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    }
]
```
</details>