# ReservesSetupHelper

Deployment helper to setup the assets risk parameters at PoolConfigurator in batch.

The ReservesSetupHelper is an Ownable contract, so only the deployer or future owners can call this contract.

## Write Methods

### configureReserves

```solidity
function configureReserves(
    PoolConfigurator configurator,
    ConfigureReserveInput[] calldata inputParams
) external onlyOwner
```

External function called by the owner account to setup the assets risk parameters in batch.

The Pool or Risk admin must transfer the ownership to ReservesSetupHelper before calling this function.

#### Input Parameters:

| Name         | Type                      | Description                                                                                 |
| :----------- | :------------------------ | :------------------------------------------------------------------------------------------ |
| configurator | `PoolConfigurator`        | The address of PoolConfigurator contract                                                    |
| inputParams  | `ConfigureReserveInput[]` | An array of ConfigureReserveInput struct that contains the assets and their risk parameters |

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