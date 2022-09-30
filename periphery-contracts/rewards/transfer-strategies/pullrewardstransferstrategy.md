# PullRewardsTransferStrategy

Transfer strategy that pulls ERC20 rewards from an external account to the user address. The external account could be a smart contract or EOA that must approve to the PullRewardsTransferStrategy contract address.

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

## View Methods

### getRewardsVault

```solidity
function getRewardsVault() external view returns (address)
```

Returns the address of the rewards vault.

#### Return Values:

| Type      | Description                      |
| :-------- | :------------------------------- |
| `address` | The address of the rewards vault |