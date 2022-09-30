# TransferStrategyBase

## Write Methods

### performTransfer

```solidity
function performTransfer(address to, address reward, uint256 amount) external virtual returns (bool)
```

Perform custom transfer logic via delegate call from source contract to a TransferStrategy implementation.

#### Input Parameters:

| Name   | Type      | Description                                          |
| :----- | :-------- | :--------------------------------------------------- |
| to     | `address` | The address to transfer rewards to                   |
| reward | `address` | The address of the reward token                      |
| amount | `uint256` | The amount to transfer to the `to` address parameter |

#### Return Values:

| Type      | Description                                   |
| :-------- | :-------------------------------------------- |
| `address` | Returns `true` if the transfer logic succeeds |

### emergencyWithdrawal

```solidity
function emergencyWithdrawal(address token, address to, uint256 amount) external onlyRewardsAdmin
```
Perform an emergency token withdrawal only callable by the Rewards admin.

#### Input Parameters:

| Name   | Type      | Description                                                   |
| :----- | :-------- | :------------------------------------------------------------ |
| token  | `address` | The address of the token to withdraw funds from this contract |
| to     | `address` | The address of the recipient of the withdrawal                |
| amount | `uint256` | The amount of the withdrawal                                  |

## View Methods

### getIncentivesController

```solidity
function getIncentivesController() external view override returns (address)
```

Returns the address of the Incentives Controller.

#### Return Values:

| Type      | Description                              |
| :-------- | :--------------------------------------- |
| `address` | The address of the Incentives Controller |

### getRewardsAdmin

```solidity
function getRewardsAdmin() external view override returns (address)
```

Returns the address of the Rewards admin.

#### Return Values:

| Type      | Description                      |
| :-------- | :------------------------------- |
| `address` | The address of the Rewards admin |