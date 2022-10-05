# ParaSwapLiquiditySwapAdapter

Adapter to swap liquidity using ParaSwap.

The [collateral swap]() feature is enabled via the ParaSwapLiquiditySwapAdapter. The user must approve the contract to pull ATokens in order to successfully swap liquidity. 

Source code available on [GitHub](https://github.com/aave/aave-v3-periphery/blob/master/contracts/adapters/paraswap/ParaSwapLiquiditySwapAdapter.sol).

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

Swaps the received reserve amount from the flash loan into the asset specified in the params. The received funds from the swap are then deposited into the protocol on behalf of the user. The user should give this contract allowance to pull the ATokens in order to withdraw the underlying asset and repay the flash loan. 

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

### swapAndDeposit

```solidity
function swapAndDeposit(
    IERC20Detailed assetToSwapFrom,
    IERC20Detailed assetToSwapTo,
    uint256 amountToSwap,
    uint256 minAmountToReceive,
    uint256 swapAllBalanceOffset,
    bytes calldata swapCalldata,
    IParaSwapAugustus augustus,
    PermitSignature calldata permitParams
) external nonReentrant
```

Swaps an amount of an asset to another and deposits the new asset amount on behalf of the user without using a flash loan. This method can be used when the temporary transfer of the collateral asset to this contract does not affect the user position. The user should give this contract allowance to pull the ATokens in order to withdraw the underlying asset and perform the swap.

#### Input Parameters:

| Name      | Type      | Description                                         |
| :-------- | :-------- | :-------------------------------------------------- |
| assetToSwapFrom | `IERC20Detailed` | The address of the underlying asset to be swapped from |
| assetToSwapTo | `IERC20Detailed` | The address of the underlying asset to be swapped to and deposited |
| amountToSwap | `uint256` | The amount to be swapped, or maximum amount when swapping all balance |
| minAmountToReceive | `uint256` | The minimum amount to be received from the swap |
| swapAllBalanceOffset | `uint256` | Set to offset of fromAmount in Augustus calldata if wanting to swap all balance, otherwise 0 |
| swapAllBalanceOffset | `uint256` | Calldata for ParaSwap's AugustusSwapper contract |
| augustus | `IParaSwapAugustus` | The address of ParaSwap's AugustusSwapper contract |
| permitParams | `PermitSignature` | Struct containing the permit signatures, set to all zeroes if not used |

## ABI
<details>
<summary>ParaSwapLiquiditySwapAdapter ABI</summary>

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
                "name": "assetToSwapFrom",
                "type": "address"
            },
            {
                "internalType": "contract IERC20Detailed",
                "name": "assetToSwapTo",
                "type": "address"
            },
            {
                "internalType": "uint256",
                "name": "amountToSwap",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "minAmountToReceive",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "swapAllBalanceOffset",
                "type": "uint256"
            },
            {
                "internalType": "bytes",
                "name": "swapCalldata",
                "type": "bytes"
            },
            {
                "internalType": "contract IParaSwapAugustus",
                "name": "augustus",
                "type": "address"
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
                "name": "permitParams",
                "type": "tuple"
            }
        ],
        "name": "swapAndDeposit",
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