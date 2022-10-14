# ReservesSetupHelper

Deployment helper to setup the assets risk parameters at [`PoolConfigurator`](../protocol/pool/poolconfigurator.md) in batch.

The `ReservesSetupHelper` is an [`Ownable`](https://github.com/aave/aave-v3-core/blob/master/contracts/dependencies/openzeppelin/contracts/Ownable.sol) contract, so only the deployer or future owners can call this contract.

The source code is available on [GitHub](https://github.com/aave/aave-v3-core/blob/master/contracts/deployments/ReservesSetupHelper).

## Write Methods

### configureReserves

```solidity
function configureReserves(
    PoolConfigurator configurator,
    ConfigureReserveInput[] calldata inputParams
) external onlyOwner
```

External function called by the owner account to setup the assets risk parameters in batch.

The [`POOL_ADMIN`](../protocol/configuration/aclmanager.md#pooladmin) or [`RISK_ADMIN`](../protocol/configuration/aclmanager.md#riskadmin) admin must transfer the ownership to `ReservesSetupHelper` before calling this function.

#### Input Parameters:

| Name         | Type                      | Description                                                                                                                                                                                         |
| :----------- | :------------------------ | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| configurator | `address`                 | The address of the [`PoolConfigurator`](../protocol/pool/poolconfigurator.md) contract                                                                                                              |
| inputParams  | `ConfigureReserveInput[]` | An array of [`ConfigureReserveInput`](https://github.com/aave/aave-v3-core/blob/master/contracts/deployments/ReservesSetupHelper.sol#L14) struct that contains the assets and their risk parameters |

The [`ConfigureReserveInput`](https://github.com/aave/aave-v3-core/blob/master/contracts/deployments/ReservesSetupHelper.sol#L14) struct is composed of the following fields:

| Name                   | Type      | Description                                                   |
| :--------------------- | :-------- | :------------------------------------------------------------ |
| asset                  | `address` | The address of the asset                                      |
| baseLTV                | `uint256` | The base Loan To Value of the reserve                         |
| liquidationThreshold   | `uint256` | The liquidation threshold of the reserve                      |
| liquidationBonus       | `uint256` | The liquidation bonus of the reserve                          |
| reserveFactor          | `uint256` | The reserve factor of the reserve                             |
| borrowCap              | `uint256` | The borrow cap of the reserve                                 |
| supplyCap              | `uint256` | The supply cap of the reserve                                 |
| stableBorrowingEnabled | `bool`    | `true` if stable rate borrowing is enabled, `false` otherwise |
| borrowingEnabled       | `bool`    | `true` if borrowing is enabled, `false` otherwise             |

## ABI
<details>
<summary>ReservesSetupHelper ABI</summary>

```
[
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
        "inputs": [
            {
                "internalType": "contract PoolConfigurator",
                "name": "configurator",
                "type": "address"
            },
            {
                "components": [
                    {
                        "internalType": "address",
                        "name": "asset",
                        "type": "address"
                    },
                    {
                        "internalType": "uint256",
                        "name": "baseLTV",
                        "type": "uint256"
                    },
                    {
                        "internalType": "uint256",
                        "name": "liquidationThreshold",
                        "type": "uint256"
                    },
                    {
                        "internalType": "uint256",
                        "name": "liquidationBonus",
                        "type": "uint256"
                    },
                    {
                        "internalType": "uint256",
                        "name": "reserveFactor",
                        "type": "uint256"
                    },
                    {
                        "internalType": "uint256",
                        "name": "borrowCap",
                        "type": "uint256"
                    },
                    {
                        "internalType": "uint256",
                        "name": "supplyCap",
                        "type": "uint256"
                    },
                    {
                        "internalType": "bool",
                        "name": "stableBorrowingEnabled",
                        "type": "bool"
                    },
                    {
                        "internalType": "bool",
                        "name": "borrowingEnabled",
                        "type": "bool"
                    }
                ],
                "internalType": "struct ReservesSetupHelper.ConfigureReserveInput[]",
                "name": "inputParams",
                "type": "tuple[]"
            }
        ],
        "name": "configureReserves",
        "outputs": [],
        "stateMutability": "nonpayable",
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