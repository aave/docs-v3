# ACLManager

## ACLManager

_**Access Control List Manager**_ is the main registry of system roles and permissions.

ACLManager allows a _**Role Admin**_ to manage roles. _**Role Admin**_ is itself a role that is managed by the `DEFAULT_ADMIN_ROLE`.

`DEFAULT_ADMIN_ROLE` is held by the _ACLAdmin,_ which is initialised in `PoolAddressesProvider`.

{% hint style="info" %}
ℹ️ On Ethereum chain `PoolAddressesProvider`, is owned by Aave Governance. In networks other than Ethereum, either the *Crosschain Governance Bridges* or *Community Multisigs* are used to manage the `PoolAddressesProvider`.
{% endhint %}


## Roles

Below we outline the powers/responsibilities of the roles and the specific methods that are only accessible to the holders of these roles.



| Role                  | Responsibilities / Powers                                                                                                                                                                                                                                             | Methods Accessible                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| FLASH\_BORROWER       | <p>Flash loan premium is waived for the holders of this role.</p><p></p><p>\*\*Does not include flashLoanSimple</p>                                                                                                                                                       | `flashLoan`                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| BRIDGE                | Can leverage the Portal feature                                                                                                                                                                                                                                       | <p><code>mintUnbacked</code><br><code>backUnbacked</code></p>                                                                                                                                                                                                                                                                                                                                                                                                                            |
| `ASSET_LISTING_ADMIN` | 
<p>Can update
  <ul>
    <li> asset oracle sources</li>
    <li>fallback oracle</li>
    <li>add new assets to the Aave market</li>
  </ul>
</p>
|
<ul>
  <li> setAssetSources </li>
  <li> setFallbackOracle</li>
  <li>initReserves</li>
</ul>
|
| RISK\_ADMIN           | <p>Can update</p><ul><li>grace period of Oracle Sentinels</li><li>reserve params</li><li>unbacked mint cap</li><li>liquidation protocol fee</li><li>existing eMode categories and create new. (not category 0)</li><li>add/remove asset in silo mode</li></ul> | <ul><li>setGracePeriod</li><li>setReserveBorrowing</li><li>configureReserveAsCollateral</li><li>setReserveStableRateBorrowing</li><li>setReserveFreeze</li><li>setBorrowableInIsolation</li><li>setReserveFactor</li><li>setDebtCeiling</li><li>setBorrowCap</li><li>setSupplyCap</li><li>setLiquidationProtocolFee</li><li>setEModeCategory</li><li>setAssetEModeCategory</li><li>setUnbackedMintCap</li><li>setReserveInterestRateStrategyAddress</li><li>setSiloedBorrowing</li></ul> |
| ACL\_ADMIN            | Manage the role admins in the ACLManager                                                                                                                                                                                                                              | setRoleAdmin, addPoolAdmin, removePoolAdmin, addEmergencyAdmin, removeEmergencyAdmin, addRiskAdmin, removeRiskAdmin, addFlashBorrower, removeFlashBorrower, addBridge, removeBridge, addAssetListingAdmin, removeAssetListingAdmin                                                                                                                                                                                                                                                       |
| EMERGENCY\_ADMIN      | Can pause/unpause the pool or individual reserve                                                                                                                                                                                                                      | setPoolPause                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| POOL\_ADMIN           | <p></p><p>Can</p><ul><li>update token implementations</li><li>drop reserves</li><li>pause/unpause reserves</li><li>activate/deactivate reserves</li><li>update premiums</li><li>do all the things available to RISK_ADMIN &#x26; ASSET_LISTING_ADMIN</li></ul>        | <p>dropReserve, updateAToken, updateStableDebtToken, updateVariableDebtToken, setReserveActive, updateBridgeProtocolFee, updateFlashloanPremiumTotal, updateFlashloanPremiumToProtocol,</p><p>all methods available to RISK_ADMIN, all methods available to ASSET_LISTING_ADMIN</p>                                                                                                                                                                                                      |

## View Methods

### isPoolAdmin

`function isPoolAdmin(address admin)`

Returns `true` if the address has `POOL_ADMIN` role.

### isEmergencyAdmin

`function isEmergencyAdmin(address admin)`

Returns `true` if the address has `EMERGENCY_ADMIN` role.

### isRiskAdmin

`function isRiskAdmin(address admin)`

Returns `true` if the address has `RISK_ADMIN` role.

### isFlashBorrower

`function isFlashBorrower(address borrower)`

Returns `true` if the address has `FLASH_BORROWER` role.

### isBridge

`function isBridge(address bridge)`

Returns `true` if the address has `BRIDGE` role.

### isAssetListingAdmin

`function isAssetListingAdmin(address admin)`

Returns `true` if the address has `ASSET_LISTING_ADMIN` role.

## Write Methods

### setRoleAdmin

`setRoleAdmin(bytes32 role, bytes32 adminRole)`

Setup admin to manage Roles.&#x20;

{% hint style="info" %}
This method can only be called by address with `DEFAULT_ADMIN_ROLE`.
{% endhint %}

Call Params

| Name      | Type    | Description                                                                                                                                                                             |
| --------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| role      | bytes32 | <p>keccak256 hash of one of the following:</p><ul><li>POOL_ADMIN</li><li>EMERGENCY_ADMIN</li><li>RISK_ADMIN</li><li>FLASH_BORROWER</li><li>BRIDGE</li><li>ASSET_LISTING_ADMIN</li></ul> |
| adminRole | bytes32 | adminRole responsible for role. 0x00 is reserved for DEFAULT\_ADMIN\_ROLE                                                                                                               |

### addPoolAdmin

`addPoolAdmin(address admin)`

Add address to the list of members in `POOL_ADMIN` role. Holders of this role can update token implementations, drop, (un)pause and (de)activate reserves, update premiums and do everything the `ASSET_LISTING_ADMIN` and `RISK_ADMIN` can do.

{% hint style="info" %}
Can be called only by _Role Admin_, specified by _Aave Governance_, responsible for managing `POOL_ADMIN` role.
{% endhint %}

Call Params

| Name  | Type    | Description                                     |
| ----- | ------- | ----------------------------------------------- |
| admin | address | address which will be granted POOL\_ADMIN role. |

### removePoolAdmin

`removePoolAdmin(address admin)`

Remove given address from the list of members in `POOL_ADMIN` role.

{% hint style="info" %}
Can be called only by _Role Admin_, specified by _Aave Governance_, responsible for managing `POOL_ADMIN` role.
{% endhint %}

Call Params

| Name  | Type    | Description                                                     |
| ----- | ------- | --------------------------------------------------------------- |
| admin | address | address for which POOL\_ADMIN role permissions must be revoked. |

### addEmergencyAdmin

`addEmergencyAdmin(address admin)`

Add address to the list of members in `EMERGENCY_ADMIN` role. Holders of this role can pause and unpause the pool or an individual reserve.

{% hint style="info" %}
Can be called only by _Role Admin_, specified by _Aave Governance_, responsible for managing`EMERGENCY_ADMIN` role.
{% endhint %}

| Name  | Type    | Description                                          |
| ----- | ------- | ---------------------------------------------------- |
| admin | address | address which will be granted EMERGENCY\_ADMIN role. |

### removeEmergencyAdmin

`function removeEmergencyAdmin(address admin)`

Remove given address from the list of members in `EMERGENCY_ADMIN` role.

{% hint style="info" %}
Can be called only by _Role Admin_, specified by _Aave Governance_, responsible for managing `EMERGENCY_ADMIN` role.
{% endhint %}

| Name  | Type    | Description                                                          |
| ----- | ------- | -------------------------------------------------------------------- |
| admin | address | address for which EMERGENCY\_ADMIN role permissions must be revoked. |

### addRiskAdmin

`function addRiskAdmin(address admin)`

Add address to the list of members in `RISK_ADMIN` role. Holders of this role can update grace period of Oracle Sentinels, reserve params, unbacked mint cap, liquidation fee and eMode categories.

| Name  | Type    | Description                                     |
| ----- | ------- | ----------------------------------------------- |
| admin | address | address which will be granted RISK\_ADMIN role. |

### removeRiskAdmin

Remove given address from the list of members in `RISK_ADMIN` role.

| Name  | Type    | Description                                                     |
| ----- | ------- | --------------------------------------------------------------- |
| admin | address | address for which RISK\_ADMIN role permissions must be revoked. |

### addFlashBorrower

`function addFlashBorrower(address borrower)`

Add address to the list of members in `FLASH_BORROWER` role. Holders of this role do not pay premium for flash loan (Does not apply to `flashLonaSimple`.

### removeFlashBorrower

`function removeFlashBorrower(address borrower)`

Remove given address from the list of members in `FLASH_BORROWER` role.

| Name  | Type    | Description                                                         |
| ----- | ------- | ------------------------------------------------------------------- |
| admin | address | address for which FLASH\_BORROWER role permissions must be revoked. |

### addBridge

`addBridge(address bridge)`

Add contract address to the list of _**bridges**_. Holders of this role can leverage the Portal feature to seamlessly move supplied assets across Aave V3 markets on different networks.

ℹ️ Can be called only by \*Role Admin,\* specified by Aave Governance\*,\* responsible for managing \`BRIDGE\` role.

| Name   | Type    | Description                                |
| ------ | ------- | ------------------------------------------ |
| bridge | address | address which will be granted BRIDGE role. |

### removeBridge

`removeBridge(address bridge)`

Remove contract address from the list of _**bridges**_.

ℹ️ Can be called only by \*Role Admin,\* specified by Aave Governance\*,\* responsible for managing \`BRIDGE\` role.

| Name   | Type    | Description                                                |
| ------ | ------- | ---------------------------------------------------------- |
| bridge | address | address for which BRIDGE role permissions must be revoked. |

### addAssetListingAdmin

`function addAssetListingAdmin(address admin)`

Add address to the list of member in `ASSET_LISTING_ADMIN` role. Holder of this role can update oracles & add new asset to the Aave market.

| Name  | Type    | Description                                               |
| ----- | ------- | --------------------------------------------------------- |
| admin | address | address which will be granted ASSET\_LISTING\_ADMIN role. |

### removeAssetListingAdmin

`function removeAssetListingAdmin(address admin)`

Remove address from the list of members in `ASSET_LISTING_ADMIN` role.

| Name  | Type    | Description                                                               |
| ----- | ------- | ------------------------------------------------------------------------- |
| admin | address | address for which ASSET\_LISTING\_ADMIN role permissions must be revoked. |
