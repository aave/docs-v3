# ParaSwapRepayAdapter

ParaSwap Adapter to perform a repay of a debt with collateral.

The [repay with collateral]() feature is enabled via the ParaSwapRepayAdapter. The user must first approve the contract to pull ATokens to successfully repay with collateral.

Source code available on [GitHub](https://github.com/aave/aave-v3-periphery/blob/master/contracts/adapters/paraswap/ParaSwapRepayAdapter.sol).

## Write Methods

### executeOperation

```solidity
function executeOperation(
    address asset,
    uint256 amount,
    uint256 premium,
    address initiator,
    bytes calldata params
) external override nonReentrant returns (bool)
```

Uses the received funds from the flash loan to repay a debt on the protocol on behalf of the user. Then pulls the collateral from the user and swaps it to the debt asset to repay the flash loan. The user should give this contract allowance to pull the ATokens in order to withdraw the underlying asset, swap it and repay the flash loan. Supports only one asset on the flash loan.

#### Input Parameters:

| Name      | Type      | Description                                         |
| :-------- | :-------- | :-------------------------------------------------- |
| asset | `address` | The address of the flash-borrowed asset |
| amount | `uint256` | The amount of the flash-borrowed asset |
| premium | `uint256` | The fee of the flash-borrowed asset |
| initiator | `address` | The address of the flashloan initiator |
| params | `bytes` | The byte-encoded params passed when initiating the flashloan |

#### Return Values:

| Type      | Description            |
| :-------- | :--------------------- |
| `bool` | `true` if the execution of the operation succeeds, `false` otherwise |

### swapAndRepay

```solidity
function swapAndRepay(
    IERC20Detailed collateralAsset,
    IERC20Detailed debtAsset,
    uint256 collateralAmount,
    uint256 debtRepayAmount,
    uint256 debtRateMode,
    uint256 buyAllBalanceOffset,
    bytes calldata paraswapData,
    PermitSignature calldata permitSignature
) external nonReentrant 
```

Swaps the user collateral for the debt asset and then repay the debt on the protocol on behalf of the user without using flash loans. This method can be used when the temporary transfer of the collateral asset to this contract does not affect the user position. The user should give this contract allowance to pull the ATokens in order to withdraw the underlying asset.

#### Input Parameters:

| Name      | Type      | Description                                         |
| :-------- | :-------- | :-------------------------------------------------- |
| collateralAsset | `IERC20Detailed` | The address of the asset to be swapped |
| debtAsset | `IERC20Detailed` | The address of debt asset |
| collateralAmount | `uint256` | The amount of the collateral to be swapped |
| debtRepayAmount | `uint256` | The amount of the debt to be repaid, or maximum amount when repaying entire debt |
| debtRateMode | `uint256` | The rate mode of the debt to be repaid |
| buyAllBalanceOffset | `uint256` | Set to offset of toAmount in Augustus calldata if wanting to pay entire debt, otherwise 0 |
| paraswapData | `bytes` | The data for Paraswap Adapter |
| permitSignature | `PermitSignature` | Struct containing the permit signature |

### getDebtRepayAmount

```solidity
function getDebtRepayAmount(
    IERC20Detailed debtAsset,
    uint256 rateMode,
    uint256 buyAllBalanceOffset,
    uint256 debtRepayAmount,
    address initiator
) private view returns (uint256)
```

Gets the amount of debt to be repaid.

#### Input Parameters:

| Name      | Type      | Description                                         |
| :-------- | :-------- | :-------------------------------------------------- |
| debtAsset | `IERC20Detailed` | The address of debt asset |
| rateMode | `uint256` | The rate mode of the debt to be repaid |
| buyAllBalanceOffset | `uint256` | Set to offset of toAmount in Augustus calldata if wanting to pay entire debt, otherwise 0 |
| debtRepayAmount | `uint256` | The amount of the debt to be repaid, or maximum amount when repaying entire debt |
| initiator | `address` | The address of the flashloan initiator |


#### Return Values:

| Type      | Description            |
| :-------- | :--------------------- |
| `uint256` | The amount of the debt to be repaid, or maximum amount when repaying entire debt |

## ABI
<details>
<summary>ParaSwapRepayAdapter ABI</summary>

```
[
    {
        "inputs": [
            {
                "internalType": "contract IPoolAddressesProvider",
                "name": "addressesProvider",
                "type": "address"
            },
            {
                "internalType": "contract IParaSwapAugustusRegistry",
                "name": "augustusRegistry",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "owner",
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
                "internalType": "address",
                "name": "fromAsset",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "toAsset",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "amountSold",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "receivedAmount",
                "type": "uint256"
            }
        ],
        "name": "Bought",
        "type": "event"
    },
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
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "fromAsset",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "toAsset",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "fromAmount",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "receivedAmount",
                "type": "uint256"
            }
        ],
        "name": "Swapped",
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
        "name": "AUGUSTUS_REGISTRY",
        "outputs": [
            {
                "internalType": "contract IParaSwapAugustusRegistry",
                "name": "",
                "type": "address"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "MAX_SLIPPAGE_PERCENT",
        "outputs": [
            {
                "internalType": "uint256",
                "name": "",
                "type": "uint256"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "ORACLE",
        "outputs": [
            {
                "internalType": "contract IPriceOracleGetter",
                "name": "",
                "type": "address"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "POOL",
        "outputs": [
            {
                "internalType": "contract IPool",
                "name": "",
                "type": "address"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "asset",
                "type": "address"
            },
            {
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "premium",
                "type": "uint256"
            },
            {
                "internalType": "address",
                "name": "initiator",
                "type": "address"
            },
            {
                "internalType": "bytes",
                "name": "params",
                "type": "bytes"
            }
        ],
        "name": "executeOperation",
        "outputs": [
            {
                "internalType": "bool",
                "name": "",
                "type": "bool"
            }
        ],
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
                "internalType": "contract IERC20",
                "name": "token",
                "type": "address"
            }
        ],
        "name": "rescueTokens",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "contract IERC20Detailed",
                "name": "collateralAsset",
                "type": "address"
            },
            {
                "internalType": "contract IERC20Detailed",
                "name": "debtAsset",
                "type": "address"
            },
            {
                "internalType": "uint256",
                "name": "collateralAmount",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "debtRepayAmount",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "debtRateMode",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "buyAllBalanceOffset",
                "type": "uint256"
            },
            {
                "internalType": "bytes",
                "name": "paraswapData",
                "type": "bytes"
            },
            {
                "components": [
                    {
                        "internalType": "uint256",
                        "name": "amount",
                        "type": "uint256"
                    },
                    {
                        "internalType": "uint256",
                        "name": "deadline",
                        "type": "uint256"
                    },
                    {
                        "internalType": "uint8",
                        "name": "v",
                        "type": "uint8"
                    },
                    {
                        "internalType": "bytes32",
                        "name": "r",
                        "type": "bytes32"
                    },
                    {
                        "internalType": "bytes32",
                        "name": "s",
                        "type": "bytes32"
                    }
                ],
                "internalType": "struct BaseParaSwapAdapter.PermitSignature",
                "name": "permitSignature",
                "type": "tuple"
            }
        ],
        "name": "swapAndRepay",
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