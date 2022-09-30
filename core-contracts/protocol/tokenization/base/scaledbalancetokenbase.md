# ScaledBalanceTokenBase

Basic ERC20 implementation of scaled balance token.

## View Methods

### scaledBalanceOf

```solidity
function scaledBalanceOf(address user) external view override returns (uint256)
```

Returns the scaled balance of the user.

The scaled balance is the sum of all the updated stored balance divided by the reserve's liquidity index at the moment of the update.

#### Input Parameters:

| Name  | Type      | Description                                         |
| :---- | :-------- | :-------------------------------------------------- |
| user | `address` | The user whose balance is calculated |

#### Return Values:

| Type      | Description                                         |
| :-------- | :-------------------------------------------------- |
| `uint256` | The scaled balance of the user |

### getScaledUserBalanceAndSupply

```solidity
function getScaledUserBalanceAndSupply(address user)
    external
    view
    override
    returns (uint256, uint256)
```

Returns the scaled balance of the user and the scaled total supply.

#### Input Parameters:

| Name  | Type      | Description                                         |
| :---- | :-------- | :-------------------------------------------------- |
| user | `address` | The address of the user |

#### Return Values:

| Type      | Description                                         |
| :-------- | :-------------------------------------------------- |
| `uint256` | The scaled balance of the user |
| `uint256` | The scaled total supply |

### scaledTotalSupply

```solidity
function scaledTotalSupply() public view virtual override returns (uint256) 
```

Returns the scaled total supply of the scaled balance token. Represents sum(debt/index).

#### Return Values:

| Type      | Description                                         |
| :-------- | :-------------------------------------------------- |
| `uint256` | The scaled total supply |

### getPreviousIndex

```solidity
function getPreviousIndex(address user) external view virtual override returns (uint256)
```

Returns last index interest was accrued to the user's balance (expressed in ray).

#### Input Parameters:

| Name  | Type      | Description                                         |
| :---- | :-------- | :-------------------------------------------------- |
| user | `address` | The address of the user |

#### Return Values:

| Type      | Description                                         |
| :-------- | :-------------------------------------------------- |
| `uint256` | The last index interest was accrued to the user's balance, expressed in ray |