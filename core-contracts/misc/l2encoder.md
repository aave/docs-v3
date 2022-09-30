# L2Encoder

Helper contract to encode calldata that is passed to the L2Pool. It is used to optimize calldata size in L2Pool for transaction cost reduction only indented to help generate calldata for uses/frontends.

## View Methods

### encodeSupplyParams

```solidity
function encodeSupplyParams(address asset, uint256 amount, uint16 referralCode) external view returns (bytes32)
```

Encodes supply parameters from standard input to compact representation of 1 bytes32. Without an onBehalfOf parameter as the compact calls to L2Pool will use msg.sender as onBehalfOf. Returns `bytes32` result to be passed to `supply()` method of L2Pool. 

#### Input Parameters:

| Name         | Type      | Description                                                                                                                                                     |
| :----------- | :-------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| asset        | `address` | The address of the underlying asset to supply                                                                                                                   |
| amount       | `uint256` | The amount of asset to be supplied                                                                                                                              |
| referralCode | `uint16`  | Code used to register the integrator originating the operation, for potential rewards. 0 if the action is executed directly by the user, without any middle-men |

#### Return Values:

| Type      | Description                                 |
| :-------- | :------------------------------------------ |
| `bytes32` | Compact representation of supply parameters |

### encodeSupplyWithPermitParams;

```solidity
function encodeSupplyWithPermitParams(
    address asset,
    uint256 amount,
    uint16 referralCode,
    uint256 deadline,
    uint8 permitV,
    bytes32 permitR,
    bytes32 permitS
  ) external view returns (bytes32, bytes32, bytes32)
```

Encodes supplyWithPermit parameters from standard input to compact representation of 3 bytes32. Without an onBehalfOf parameter as the compact calls to L2Pool will use msg.sender as onBehalfOf. Returns (`bytes32, bytes32, bytes32`) result to be passed to `supplyWithPermit()` method of L2Pool.

Permit signature must be signed by `msg.sender` with spender as Pool address.

#### Input Parameters:

| Name         | Type      | Description                                                                                                                                                     |
| :----------- | :-------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| asset        | `address` | The address of the underlying asset to supply. Same asset as used in permit v, r, and s                                                                         |
| amount       | `uint256` | The amount of asset to be supplied. Same amount as used in permit v, r, and s                                                                                   |
| referralCode | `uint16`  | Code used to register the integrator originating the operation, for potential rewards. 0 if the action is executed directly by the user, without any middle-men |
| deadline     | `uint256` | The unix timestamp up until which permit signature is valid                                                                                                     |
| permitV      | `uint8`   | The v parameter of the ERC712 permit signature                                                                                                                  |
| permitR      | `bytes32` | The r parameter of the ERC712 permit signature                                                                                                                  |
| permitS      | `bytes32` | The s parameter of the ERC712 permit signature                                                                                                                  |

#### Return Values:

| Type      | Description                                           |
| :-------- | :---------------------------------------------------- |
| `bytes32` | Compact representation of supplyWithPermit parameters |
| `bytes32` | The r parameter of the ERC712 permit signature        |
| `bytes32` | The s parameter of the ERC712 permit signature        |

### encodeWithdrawParams

```solidity
function encodeWithdrawParams(address asset, uint256 amount) external view returns (bytes32)
```

Encodes withdraw parameters from standard input to compact representation of 1 bytes32. Returns `bytes32` result to be passed to `withdraw()` method of L2Pool. Without a to parameter as the compact calls `to` L2Pool will use msg.sender as `to`.

#### Input Parameters:

| Name   | Type      | Description                                                     |
| :----- | :-------- | :-------------------------------------------------------------- |
| asset  | `address` | The address of the underlying asset to withdraw, not the aToken |
| amount | `uint256` | The underlying amount to be withdrawn                           |

#### Return Values:

| Type      | Description                                   |
| :-------- | :-------------------------------------------- |
| `bytes32` | Compact representation of withdraw parameters |

### encodeBorrowParams

```solidity
function encodeBorrowParams(
    address asset,
    uint256 amount,
    uint256 interestRateMode,
    uint16 referralCode
) external view returns (bytes32)
```

Encodes borrow parameters from standard input to compact representation of 1 bytes32. Without an onBehalfOf parameter as the compact calls to L2Pool will use msg.sender as onBehalfOf. Returns `bytes32` result to be passed to `borrow()` method of L2Pool.

#### Input Parameters:

| Name             | Type      | Description                                                                                                                                                         |
| :--------------- | :-------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| asset            | `address` | The address of the underlying asset to borrow                                                                                                                       |
| amount           | `uint256` | The amount to be borrowed, expressed in wei units                                                                                                                   |
| interestRateMode | `uint256` | The interest rate mode at which the user wants to borrow: 1 for Stable, 2 for Variable                                                                              |
| referralCode     | `uint16`  | The code used to register the integrator originating the operation, for potential rewards. 0 if the action is executed directly by the user, without any middle-men |

#### Return Values:

| Type      | Description                                   |
| :-------- | :-------------------------------------------- |
| `bytes32` | Compact representation of withdraw parameters |

### encodeRepayParams

```solidity
function encodeRepayParams(address asset, uint256 amount, uint256 interestRateMode) external view returns (bytes32)
```

Encodes repay parameters from standard input to compact representation of 1 bytes32. Without an onBehalfOf parameter as the compact calls to L2Pool will use msg.sender as onBehalfOf. Returns `bytes32` result to be passed to `repay()` method of L2Pool.

#### Input Parameters:

| Name             | Type      | Description                                                                                                                                         |
| :--------------- | :-------- | :-------------------------------------------------------------------------------------------------------------------------------------------------- |
| asset            | `address` | The address of the borrowed underlying asset previously borrowed                                                                                    |
| amount           | `uint256` | The amount to repay, expressed in wei units. Use `type(uint256).max`in order to repay the whole debt for `asset` on the specific `interestRateMode` |
| interestRateMode | `uint256` | The interest rate mode at of the debt the user wants to repay: 1 for Stable, 2 for Variable                                                         |

#### Return Values:

| Type      | Description                                |
| :-------- | :----------------------------------------- |
| `bytes32` | Compact representation of repay parameters |

### encodeRepayWithPermitParams

```solidity
function encodeRepayWithPermitParams(
    address asset,
    uint256 amount,
    uint256 interestRateMode,
    uint256 deadline,
    uint8 permitV,
    bytes32 permitR,
    bytes32 permitS
) external view returns (bytes32, bytes32, bytes32)
```

Encodes repayWithPermit parameters from standard input to compact representation of 3 bytes32. Without an onBehalfOf parameter as the compact calls to L2Pool will use msg.sender as onBehalfOf. Returns (`bytes32, bytes32, bytes32`) result to be passed to `repayWithPermit()` method of L2Pool.

#### Input Parameters:

| Name             | Type      | Description                                                                                               |
| :--------------- | :-------- | :-------------------------------------------------------------------------------------------------------- |
| asset            | `address` | The address of the borrowed underlying asset previously borrowed. Same asset as used in permit v, r and s |
| amount           | `uint256` | The amount to repay. Same amount as used in permit v, r and s                                             |
| interestRateMode | `uint256` | The interest rate mode at of the debt the user wants to repay: 1 for Stable, 2 for Variable               |
| deadline         | `uint256` | The unix timestamp up until which permit signature is valid                                               |
| permitV          | `uint8`   | The v parameter of the ERC712 permit signature                                                            |
| permitR          | `bytes32` | The r parameter of the ERC712 permit signature                                                            |
| permitS          | `bytes32` | The s parameter of the ERC712 permit signature                                                            |

#### Return Values:

| Type      | Description                                           |
| :-------- | :---------------------------------------------------- |
| `bytes32` | Compact representation of supplyWithPermit parameters |
| `bytes32` | The r parameter of the ERC712 permit signature        |
| `bytes32` | The s parameter of the ERC712 permit signature        |

### encodeRepayWithATokensParams 

```solidity
function encodeRepayWithATokensParams(address asset, uint256 amount, uint256 interestRateMode) external view returns (bytes32)
```

Encodes repay with aToken parameters from standard input to compact representation of 1 bytes32. Returns `bytes32` result to be passed to `repay()` method of L2Pool.

#### Input Parameters:

| Name             | Type      | Description                                                                                                                                         |
| :--------------- | :-------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| asset            | `address` | The address of the borrowed underlying asset previously borrowed                                                                                    |
| amount           | `uint256` | The amount to repay, expressed in wei units. Use `type(uint256).max`in order to repay the whole debt for `asset` on the specific `interestRateMode` |
| interestRateMode | `uint256` | The interest rate mode at of the debt the user wants to repay: 1 for Stable, 2 for Variable                                                         |

#### Return Values:

| Type      | Description                                            |
| :-------- | :----------------------------------------------------- |
| `bytes32` | Compact representation of repay with aToken parameters |

### encodeSwapBorrowRateMode

```solidity
function encodeSwapBorrowRateMode(address asset, uint256 interestRateMode) external view returns (bytes32)
```

Encodes swap borrow rate mode parameters from standard input to compact representation of 1 bytes32. Returns `bytes32` result to be passed to `swapBorrowRateMode()` method of L2Pool.

#### Input Parameters:

| Name             | Type      | Description                                                                                |
| :--------------- | :-------- | :----------------------------------------------------------------------------------------- |
| asset            | `address` | The address of the underlying asset borrowed                                               |
| interestRateMode | `uint256` | The current interest rate mode of the position being swapped: 1 for Stable, 2 for Variable |

#### Return Values:

| Type      | Description                                                |
| :-------- | :--------------------------------------------------------- |
| `bytes32` | Compact representation of swap borrow rate mode parameters |

### encodeRebalanceStableBorrowRate

```solidity
function encodeRebalanceStableBorrowRate(address asset, address user) external view returns (bytes32)
```

Encodes rebalance stable borrow rate parameters from standard input to compact representation of 1 bytes32. Returns `bytes32` result to be passed to `rebalanceStableBorrowRate()` method of L2Pool.

#### Input Parameters:

| Name  | Type      | Description                                                                             |
| :---- | :-------- | :-------------------------------------------------------------------------------------- |
| asset | `address` | The address of the underlying asset borrowed for which the position is being rebalanced |
| user  | `address` | The address of the user to be rebalanced                                                |

#### Return Values:

| Type      | Description                                                       |
| :-------- | :---------------------------------------------------------------- |
| `bytes32` | Compact representation of rebalance stable borrow rate parameters |

### encodeSetUserUseReserveAsCollateral

```solidity
function encodeSetUserUseReserveAsCollateral(address asset, bool useAsCollateral) external view returns (bytes32)
```

Encodes set user use reserve as collateral parameters from standard input to compact representation of 1 bytes32. Returns `bytes32` result to be passed to `setUserUseReserveAsCollateral()` method of L2Pool.

#### Input Parameters:

| Name            | Type      | Description                                                             |
| :-------------- | :-------- | :---------------------------------------------------------------------- |
| asset           | `address` | The address of the underlying asset borrowed                            |
| useAsCollateral | `bool`    | True if the user wants to use the supply as collateral, false otherwise |

#### Return Values:

| Type      | Description                                                             |
| :-------- | :---------------------------------------------------------------------- |
| `bytes32` | Compact representation of set user use reserve as collateral parameters |

### encodeLiquidationCall;

```solidity
function encodeLiquidationCall(
    address collateralAsset,
    address debtAsset,
    address user,
    uint256 debtToCover,
    bool receiveAToken
) external view returns (bytes32, bytes32)
```

Encodes liquidation input parameters from standard input to compact representation of 2 bytes32. Returns (`bytes32, bytes32`) result to be passed to `liquidationCall()` method of L2Pool.

#### Input Parameters:

| Name            | Type      | Description                                                                                                                              |
| :-------------- | :-------- | :--------------------------------------------------------------------------------------------------------------------------------------- |
| collateralAsset | `address` | The address of the underlying asset used as collateral, to receive as result of the liquidation                                          |
| debtAsset       | `address` | The address of the underlying borrowed asset to be repaid with the liquidation                                                           |
| user            | `address` | The address of the borrower getting liquidated                                                                                           |
| debtToCover     | `uint256` | The debt amount of borrowed `asset` the liquidator wants to cover                                                                        |
| receiveAToken   | `bool`    | True if the liquidators wants to receive the collateral aTokens, `false` if he wants to receive the underlying collateral asset directly |

#### Return Values:

| Type      | Description                                                          |
| :-------- | :------------------------------------------------------------------- |
| `bytes32` | First half ot compact representation of liquidation input parameters |

## ABI
<details>
<summary>L2Encoder ABI</summary>

```
[
    {
        "inputs": [
            {
                "internalType": "contract IPool",
                "name": "pool",
                "type": "address"
            }
        ],
        "stateMutability": "nonpayable",
        "type": "constructor"
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
                "name": "interestRateMode",
                "type": "uint256"
            },
            {
                "internalType": "uint16",
                "name": "referralCode",
                "type": "uint16"
            }
        ],
        "name": "encodeBorrowParams",
        "outputs": [
            {
                "internalType": "bytes32",
                "name": "",
                "type": "bytes32"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "collateralAsset",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "debtAsset",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "user",
                "type": "address"
            },
            {
                "internalType": "uint256",
                "name": "debtToCover",
                "type": "uint256"
            },
            {
                "internalType": "bool",
                "name": "receiveAToken",
                "type": "bool"
            }
        ],
        "name": "encodeLiquidationCall",
        "outputs": [
            {
                "internalType": "bytes32",
                "name": "",
                "type": "bytes32"
            },
            {
                "internalType": "bytes32",
                "name": "",
                "type": "bytes32"
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
                "internalType": "address",
                "name": "user",
                "type": "address"
            }
        ],
        "name": "encodeRebalanceStableBorrowRate",
        "outputs": [
            {
                "internalType": "bytes32",
                "name": "",
                "type": "bytes32"
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
                "name": "interestRateMode",
                "type": "uint256"
            }
        ],
        "name": "encodeRepayParams",
        "outputs": [
            {
                "internalType": "bytes32",
                "name": "",
                "type": "bytes32"
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
                "name": "interestRateMode",
                "type": "uint256"
            }
        ],
        "name": "encodeRepayWithATokensParams",
        "outputs": [
            {
                "internalType": "bytes32",
                "name": "",
                "type": "bytes32"
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
                "name": "interestRateMode",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "deadline",
                "type": "uint256"
            },
            {
                "internalType": "uint8",
                "name": "permitV",
                "type": "uint8"
            },
            {
                "internalType": "bytes32",
                "name": "permitR",
                "type": "bytes32"
            },
            {
                "internalType": "bytes32",
                "name": "permitS",
                "type": "bytes32"
            }
        ],
        "name": "encodeRepayWithPermitParams",
        "outputs": [
            {
                "internalType": "bytes32",
                "name": "",
                "type": "bytes32"
            },
            {
                "internalType": "bytes32",
                "name": "",
                "type": "bytes32"
            },
            {
                "internalType": "bytes32",
                "name": "",
                "type": "bytes32"
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
                "internalType": "bool",
                "name": "useAsCollateral",
                "type": "bool"
            }
        ],
        "name": "encodeSetUserUseReserveAsCollateral",
        "outputs": [
            {
                "internalType": "bytes32",
                "name": "",
                "type": "bytes32"
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
                "internalType": "uint16",
                "name": "referralCode",
                "type": "uint16"
            }
        ],
        "name": "encodeSupplyParams",
        "outputs": [
            {
                "internalType": "bytes32",
                "name": "",
                "type": "bytes32"
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
                "internalType": "uint16",
                "name": "referralCode",
                "type": "uint16"
            },
            {
                "internalType": "uint256",
                "name": "deadline",
                "type": "uint256"
            },
            {
                "internalType": "uint8",
                "name": "permitV",
                "type": "uint8"
            },
            {
                "internalType": "bytes32",
                "name": "permitR",
                "type": "bytes32"
            },
            {
                "internalType": "bytes32",
                "name": "permitS",
                "type": "bytes32"
            }
        ],
        "name": "encodeSupplyWithPermitParams",
        "outputs": [
            {
                "internalType": "bytes32",
                "name": "",
                "type": "bytes32"
            },
            {
                "internalType": "bytes32",
                "name": "",
                "type": "bytes32"
            },
            {
                "internalType": "bytes32",
                "name": "",
                "type": "bytes32"
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
                "name": "interestRateMode",
                "type": "uint256"
            }
        ],
        "name": "encodeSwapBorrowRateMode",
        "outputs": [
            {
                "internalType": "bytes32",
                "name": "",
                "type": "bytes32"
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
            }
        ],
        "name": "encodeWithdrawParams",
        "outputs": [
            {
                "internalType": "bytes32",
                "name": "",
                "type": "bytes32"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    }
]
```
</details>