# IncentivizedERC20

Basic ERC20 implementation, inspired by the Openzeppelin ERC20 implementation.

## Write Methods

### setIncentivesController

```solidity
function setIncentivesController(IAaveIncentivesController controller) external onlyPoolAdmin
```

Sets a new Incentives Controller.

Only Pool Admin can call this methods. To update Incentives Controller on main Aave market, Governance Proposal must be submitted.

#### Input Parameters:

| Name  | Type      | Description                                         |
| :---- | :-------- | :-------------------------------------------------- |
| controller | `IAaveIncentivesController` | The new Incentives controller |

### transfer

Moves `amount` tokens from the caller's account to `recipient`. Returns a boolean value indicating whether the operation succeeded. Emits a `Transfer` event.

```solidity
function transfer(address recipient, uint256 amount) external virtual override returns (bool)
```

#### Input Parameters:

| Name  | Type      | Description                                         |
| :---- | :-------- | :-------------------------------------------------- |
| recipient | `address` | The recipient of the tokens |
| amount | `uint256` | The amount of tokens to transfer |

#### Return Values:

| Type      | Description                                         |
| :-------- | :-------------------------------------------------- |
| `bool` | Returns true if tokens transferred successfully, false otherwise |

### approve

```solidity
function approve(address spender, uint256 amount) external virtual override returns (bool)
```

Sets `amount` as the allowance of `spender` over the caller's tokens. Returns a boolean value indicating whether the operation succeeded. Emits an {Approval} event.

{% hint style="danger" %}
Beware that changing an allowance with this method brings the risk that someone may use both the old and the new allowance by unfortunate transaction ordering. One possible solution to mitigate this race condition is to first reduce the spender's allowance to 0 and set the desired value afterwards.
{% endhint %}

#### Input Parameters:

| Name  | Type      | Description                                         |
| :---- | :-------- | :-------------------------------------------------- |
| spender | `address` | The address of the spender |
| amount | `uint256` | The amount of tokens to approve |

#### Return Values:

| Type      | Description                                         |
| :-------- | :-------------------------------------------------- |
| `bool` | Returns true if the operation was successful, false otherwise |

### transferFrom

```solidity
function transferFrom(address sender, address recipient, uint256 amount) external virtual override returns (bool)
```

Moves `amount` tokens from `sender` to `recipient` using the allowance mechanism. `amount` is then deducted from the caller's allowance. Returns a boolean value indicating whether the operation succeeded. Emits a {Transfer} event.

#### Input Parameters:

| Name  | Type      | Description                                         |
| :---- | :-------- | :-------------------------------------------------- |
| spender | `address` | The address to send tokens from |
| recipient | `address` | The amount of tokens to approve |
| amount | `uint256` | The amount of tokens to approve |

#### Return Values:

| Type      | Description                                         |
| :-------- | :-------------------------------------------------- |
| `bool` | Returns true if the operation was successful, false otherwise |

### increaseAllowance

```solidity
function increaseAllowance(address spender, uint256 addedValue) external virtual returns (bool)
```

Increases the allowance of spender to spend _msgSender() tokens.

#### Input Parameters:

| Name  | Type      | Description                                         |
| :---- | :-------- | :-------------------------------------------------- |
| spender | `address` | The user allowed to spend on behalf of _msgSender() |
| addedValue | `uint256` | The amount being added to the allowance |

#### Return Values:

| Type      | Description                                         |
| :-------- | :-------------------------------------------------- |
| `bool` | Returns `true` if the operation was successful, false otherwise |

### decreaseAllowance

```solidity
function decreaseAllowance(address spender, uint256 subtractedValue)
    external
    virtual
    returns (bool)
```

Decreases the allowance of spender to spend _msgSender() tokens.

#### Input Parameters:

| Name  | Type      | Description                                         |
| :---- | :-------- | :-------------------------------------------------- |
| spender | `address` | The user allowed to spend on behalf of _msgSender() |
| subtractedValue | `uint256` | The amount being subtracted from the allowance |

#### Return Values:

| Type      | Description                                         |
| :-------- | :-------------------------------------------------- |
| `bool` | Returns `true` if the operation was successful, false otherwise |

### name

```solidity
function name() public view override returns (string memory)
```

Returns the name of the token.

#### Return Values:

| Type      | Description                                         |
| :-------- | :------------------- |
| `string` | The name of the token |

### symbol

```solidity
function symbol() external view override returns (string memory) 
```

Returns the symbol of the token, usually a shorter version of the name.

#### Return Values:

| Type      | Description                                         |
| :-------- | :-------------------------------------------------- |
| `string` | The symbol of the token |

### decimals

```solidity
function decimals() external view override returns (uint8)
```

Returns the number of decimals used to get its user representation. 

For example, if decimals equals 2, a balance of 505 tokens should be displayed to a user as 5,05 (505 / 10 ** 2).

Tokens usually opt for a value of 18, imitating the relationship between Ether and Wei.

#### Return Values:

| Type      | Description                                         |
| :-------- | :-------------------------------------------------- |
| `uint8` | The number of decimals used to get its user representation |

### totalSupply

```solidity
function totalSupply() public view virtual override returns (uint256)
```

Returns the amount of tokens in existence.

#### Return Values:

| Type      | Description                                         |
| :-------- | :-------------------------------------------------- |
| `uint256` | The amount of tokens in existence |

### balanceOf

```solidity
function balanceOf(address account) public view virtual override returns (uint256)
```

Returns the amount of tokens owned by `account`.

#### Input Parameters:

| Name  | Type      | Description                                         |
| :---- | :-------- | :-------------------------------------------------- |
| account | `address` | The balance of this address |

#### Return Values:

| Type      | Description                                         |
| :-------- | :-------------------------------------------------- |
| `uint256` | The amount of tokens owned by `account` |

### getIncentivesController

```solidity
function getIncentivesController() external view virtual returns (IAaveIncentivesController)
```

Returns the address of the Incentives Controller contract.

#### Return Values:

| Type      | Description                                         |
| :-------- | :-------------------------------------------------- |
| `uint256` | The address of the Incentives Controller |

### allowance

```solidity
function allowance(address owner, address spender)
    external
    view
    virtual
    override
    returns (uint256)
```

Returns the remaining number of tokens that `spender` will be allowed to spend on behalf of the `owner` through `transferFrom()`. This is zero by default. This value changes when `approve()` or `transferFrom()` are called.
  
#### Input Parameters:

| Name  | Type      | Description                                         |
| :---- | :-------- | :-------------------------------------------------- |
| owner | `address` | The address of the owner of the tokens to allow to spend |
| spender | `address` | The address of the spender who will be allowed to spend on behalf of the `owner |

#### Return Values:

| Type      | Description                                         |
| :-------- | :-------------------------------------------------- |
| `uint256` | The remaining number of tokens that `spender` will be allowed to spend on behalf of the `owner`. Zero by default |