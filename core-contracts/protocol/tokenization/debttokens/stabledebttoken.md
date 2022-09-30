# StableDebtToken

Implements a stable debt token to track the borrowing positions of users at stable rate mode.

`transfer` and `approve` functionalities are disabled as stable debt tokens are non-transferable.

The sToken value is pegged 1:1 to the value of underlying borrowed asset and represents the current total amount owed to the protocol i.e. principal debt + interest accrued.

The StableDebtToken contract inherits the [DebtTokenBase](../base/debttokenbase.md) and [IncentivizedERC20](../base/incentivizederc20.md) token contracts.

## Write Methods

### initialize

```solidity
function initialize(
    IPool initializingPool,
    address underlyingAsset,
    IAaveIncentivesController incentivesController,
    uint8 debtTokenDecimals,
    string memory debtTokenName,
    string memory debtTokenSymbol,
    bytes calldata params
) external override initializer 
```

Initializes the stable debt token.

#### Input Parameters:

| Name                 | Type                        | Description                                                              |
| :------------------- | :-------------------------- | :----------------------------------------------------------------------- |
| initializingPool     | `IPool`                     | The pool contract that is initializing this contract                     |
| underlyingAsset      | `address`                   | The address of the underlying asset of this aToken (E.g. WETH for aWETH) |
| incentivesController | `IAaveIncentivesController` | The smart contract managing potential incentives distribution            |
| debtTokenDecimals    | `uint8`                     | The decimals of the stableDebtToken, same as the underlying asset's      |
| debtTokenName        | `string`                    | The name of the stable token                                             |
| debtTokenSymbol      | `string`                    | The symbol of the stable token                                           |
| params               | `bytes`                     | A set of encoded parameters for additional initialization                |

### mint

```solidity
function mint(
    address user,
    address onBehalfOf,
    uint256 amount,
    uint256 rate
) external virtual override onlyPool returns (bool, uint256, uint256)
```

Mints stable debt token to the `onBehalfOf` address.

The resulting rate is the weighted average between the rate of the new debt and the rate of the previous debt.

#### Input Parameters:

| Name       | Type      | Description                                                                                                                      |
| :--------- | :-------- | :------------------------------------------------------------------------------------------------------------------------------- |
| user       | `address` | The address receiving the borrowed underlying, being the delegatee in case of credit delegate, or same as `onBehalfOf` otherwise |
| onBehalfOf | `address` | The address receiving the stable debt tokens                                                                                     |
| amount     | `uint256` | The amount of stable debt tokens to mint                                                                                         |
| rate       | `uint256` | The stable rate of the debt being minted                                                                                         |

#### Return Values:

| Type      | Description                                          |
| :-------- | :--------------------------------------------------- |
| `bool`    | `true` if it is the first borrow, `false` otherwise |
| `uint256` | The total stable debt                                |
| `uint256` | The average stable borrow rate                       |

### burn

```solidity
function burn(address from, uint256 amount)
    external
    virtual
    override
    onlyPool
    returns (uint256, uint256)
```

Burns the debt of `user`.

The resulting rate is the weighted average between the rate of the new debt and the rate of the previous debt.

In some instances, a burn transaction will emit a mint event if the amount to burn is less than the interest the user earned.

#### Input Parameters:

| Name   | Type      | Description                                                  |
| :----- | :-------- | :----------------------------------------------------------- |
| from   | `address` | The address from which the stable debt tokens will be burned |
| amount | `uint256` | The amount of stable debt tokens that will be burned         |

#### Return Values:

| Type      | Description                    |
| :-------- | :----------------------------- |
| `uint256` | The total stable debt          |
| `uint256` | The average stable borrow rate |

## View Methods

### totalSupply

```solidity
function totalSupply() public view virtual override returns (uint256) 
```

Returns the amount of tokens in existence - the most up to date total debt accrued by all protocol users for that specific stable rate of debt token.

Standard ERC20 function. 

#### Return Values:

| Type      | Description                       |
| :-------- | :-------------------------------- |
| `uint256` | The amount of tokens in existence |

### balanceOf

```solidity
function balanceOf(address account) public view virtual override returns (uint256) 
```

Returns the amount of tokens owned by `account` - the most up to date accumulated debt (principal + interest) of the user.

Standard ERC20 function. 

#### Input Parameters:

| Name  | Type      | Description                                         |
| :---- | :-------- | :-------------------------------------------------- |
| account | `address` | The balance of this address |

#### Return Values:

| Type      | Description                                         |
| :-------- | :-------------------------------------------------- |
| `uint256` | The amount of tokens owned by `account` |

### getAverageStableRate

```solidity
function getAverageStableRate() external view virtual override returns (uint256)
```

Returns the average stable rate across all the stable rate debt in the protocol as `uint256`.

#### Return Values:

| Type      | Description                                         |
| :-------- | :-------------------------------------------------- |
| `uint256` | The average stable rate |

### getUserLastUpdated

```solidity
function getUserLastUpdated(address user) external view virtual override returns (uint40)
```

Returns the timestamp of the last action/update taken by the `user` as `uint40`.

#### Input Parameters:

| Name  | Type      | Description                                         |
| :---- | :-------- | :-------------------------------------------------- |
| user | `address` | The address of the user |

#### Return Values:

| Type      | Description                                         |
| :-------- | :-------------------------------------------------- |
| `uint40` | The timestamp |

### getUserStableRate

```solidity
function getUserStableRate(address user) external view virtual override returns (uint256)
```

Returns the stable rate of the user debt.

#### Input Parameters:

| Name  | Type      | Description                                         |
| :---- | :-------- | :-------------------------------------------------- |
| user | `address` | The address of the user |

#### Return Values:

| Type      | Description                                         |
| :-------- | :-------------------------------------------------- |
| `uint256` | The stable rate of the user |

### getSupplyData

```solidity
function getSupplyData()
    external
    view
    override
    returns (
      uint256,
      uint256,
      uint256,
      uint40
)
```

Returns the principal, the total supply, the average stable rate and the timestamp for the last update.

#### Return Values:

| Type      | Description                                         |
| :-------- | :-------------------------------------------------- |
| `uint256` | The principal |
| `uint256` | The total supply |
| `uint256` | The average stable rate |
| `uint40` | The timestamp of the last update |

### getTotalSupplyAndAvgRate

```solidity
function getTotalSupplyAndAvgRate() external view override returns (uint256, uint256)
```

Returns the total supply and the average stable rate.

#### Return Values:

| Type      | Description                                         |
| :-------- | :-------------------------------------------------- |
| `uint256` | The total supply |
| `uint256` | The average rate |


### getTotalSupplyLastUpdated

```solidity
function getTotalSupplyLastUpdated() external view override returns (uint40)
```

Returns the timestamp of the last update of the total supply.

#### Return Values:

| Type      | Description                                         |
| :-------- | :-------------------------------------------------- |
| `uint40` | The timestamp |

### principalBalanceOf

```solidity
function principalBalanceOf(address user) external view virtual override returns (uint256)
```

Returns the principal debt balance of the user.

#### Return Values:

| Type      | Description                                         |
| :-------- | :-------------------------------------------------- |
| `uint256` | The debt balance of the user since the last burn/mint action |

### UNDERLYING_ASSET_ADDRESS

```solidity
function UNDERLYING_ASSET_ADDRESS() external view override returns (address) 
```

Returns the address of the underlying asset of this stableDebtToken (E.g. WETH for stableDebtWETH).

#### Return Values:

| Type      | Description                                         |
| :-------- | :-------------------------------------------------- |
| `address` | The address of the underlying asset |

## Pure Methods

### getRevision

```solidity
function getRevision() internal pure virtual override returns (uint256)
```

Returns the revision number of the contract. Needs to be defined in the inherited class as a constant.

Returns `0x1`.

#### Return Values:

| Type      | Description         |
| :-------- | :------------------ |
| `uint256` | The revision number | 

## NOT SUPPORTED OPERATIONS

Being non-transferrable, the stable debt token does not implement any of the standard ERC20 functions for `transfer` and `allowance`.

The functions below will revert with the error code 80, `OPERATION_NOT_SUPPORTED`.

### transfer

```solidity
function transfer(address, uint256) external virtual override returns (bool)
```

{% hint style="danger" %}
Operation not supported. 
{% endhint %}

### allowance

```solidity
function allowance(address, address) external view virtual override returns (uint256)
```

{% hint style="danger" %}
Operation not supported. 
{% endhint %}

### approve

```solidity
function approve(address, uint256) external virtual override returns (bool) 
```

{% hint style="danger" %}
Operation not supported. 
{% endhint %}

### transferFrom

```solidity
function transferFrom(
    address,
    address,
    uint256
) external virtual override returns (bool)
```

{% hint style="danger" %}
Operation not supported. 
{% endhint %}

### increaseAllowance

```solidity
function increaseAllowance(address, uint256) external virtual override returns (bool)
```

{% hint style="danger" %}
Operation not supported. 
{% endhint %}

### decreaseAllowance
```solidity
function decreaseAllowance(address, uint256) external virtual override returns (bool)
```

{% hint style="danger" %}
Operation not supported. 
{% endhint %}