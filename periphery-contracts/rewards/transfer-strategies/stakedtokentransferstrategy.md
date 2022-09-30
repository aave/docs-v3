# StakedTokenTransferStrategy

Transfer strategy that stakes the rewards into a staking contract and transfers the staking contract token. The underlying token must be transferred to this contract to be able to stake it on demand.

## Write Methods

### performTransfer

```solidity
function performTransfer(address to, address reward, uint256 amount) external override (
    TransferStrategyBase, 
    ITransferStrategyBase
) onlyIncentivesController returns (bool)
```

Perform custom transfer logic via delegate call from source contract to a TransferStrategy implementation.

#### Input Parameters:

| Name   | Type      | Description                                      |
| :----- | :-------- | :----------------------------------------------- |
| to     | `address` | The account to transfer rewards                  |
| reward | `address` | The address of the reward token                  |
| amount | `uint256` | The amount to transfer to `to` address parameter |

#### Return Values:

| Type   | Description                                   |
| :----- | :-------------------------------------------- |
| `bool` | Returns `true` if the transfer logic succeeds |

### renewApproval

```solidity
function renewApproval() external onlyRewardsAdmin
```

Perform a `MAX_UINT` approval of AAVE to the Staked Aave contract.

### dropApproval

```solidity
function dropApproval() external onlyRewardsAdmin
```

Drop approval of AAVE to the Staked Aave contract in case of emergency.

## View Methods

### getStakeContract

```solidity
function getStakeContract() external view returns (address) 
```

Returns the Staked Token contract address.

#### Return Values:

| Type      | Description                       |
| :-------- | :-------------------------------- |
| `address` | The Staked Token contract address |

### getUnderlyingToken

```solidity
function getUnderlyingToken() external view returns (address)
```

Returns the underlying token address from the stake contract.

#### Return Values:

| Type      | Description                                          |
| :-------- | :--------------------------------------------------- |
| `address` | The underlying token address from the stake contract |