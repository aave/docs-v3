# L2Pool

The L2Pool.sol is the contract for the L2 optimized user facing methods of the protocol that takes byte encoded input arguments. It exposes the liquidity management methods that can be invoked using either Solidity or Web3 libraries. Calldata optimized extension of the Pool contract allowing users to pass compact calldata representation to reduce transaction costs on rollups.

{% hint style="info" %}
Pool methods not exposed in L2Pool.sol (such as flashLoan, setUserEMode etc.) are the same on L2 as on other versions of protocol. Refer to [Pool](../../core-contracts/pool.md) docs for the rest of the methods.
{% endhint %}

{% hint style="info" %}
Since we have a limited set of supported assets that are already given an individual id, we use the 16 bit asset id in the encoded arguments instead of 160 bit asset address.
{% endhint %}

## Methods

### supply

```solidity
function supply(bytes32 args) external override
```

Calldata efficient wrapper of the supply function on behalf of the caller. Supplies asset into the protocol, minting the same amount of corresponding aTokens, and transferring them to `msg.sender`.

{% hint style="info" %}
You can use data returned from [`encodeSupplyParams`]() method in L2Encoder helper contract to pass to this method.
{% endhint %}

#### Input Parameters:

| Name | Type      | Description                                                                                                                                                                                                                                                                                                                                                            |
| :--- | :-------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| args | `bytes32` | <p>Arguments for the supply function packed in one bytes32<br>bit 0-15: `uint16` assetId - the index of the asset in the reservesList<br>bit 16-143: `uint128` shortenedAmount - cast to 256 bits at decode time, if `type(uint128).max` the value will be expanded to `type(uint256).max`<br>bit 144-159: `uint16` referralCode - used for 3rd party integrations</p> |

### supplyWithPermit

```solidity
function supplyWithPermit(bytes32 args, bytes32 r, bytes32 s) external override
```

Calldata efficient wrapper of the supplyWithPermit function on behalf of the caller. Supply with transfer approval of supplied asset via permit function. This method removes the need for separate approval transaction before supplying asset to the pool.

{% hint style="info" %}
You can use data returned from [`encodeSupplyWithPermitParams`]() method in L2Encoder helper contract to pass to this method.
{% endhint %}

#### Input Parameters:

| Name | Type      | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| :--- | :-------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| args | `bytes32` | <p>Arguments for the supply function packed in one bytes32<br>bit 0-15: `uint16` assetId - the index of the asset in the reservesList<br>bit 16-143: `uint128` shortenedAmount - cast to 256 bits at decode time, if `type(uint128).max` the value will be expanded to `type(uint256).max`<br>bit 144-159: `uint16` referralCode - used for 3rd party integrations<br>bit 160-191: `uint32` shortenedDeadline - shortened deadline from the original `uint256`<br>bit 192-199: `uint8` permitV - the V parameter of ERC712 permit signature</p> |
| r    | `bytes32` | The R parameter of ERC712 permit signature                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| s    | `bytes32` | The S parameter of ERC712 permit signature                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |

### withdraw

```solidity
function withdraw(bytes32 args) external override
```

Calldata efficient wrapper of the withdraw function, withdrawing to the caller. Withdraws `amount` of the underlying `asset`, i.e. redeems the underlying token and burns the aTokens.

If user has any existing debt backed by the underlying token, then the maximum amount available to withdraw is the amount that will not leave user with health factor < 1 after withdrawal.

{% hint style="info" %}
You can use data returned from [`encodeWithdrawParams`]() method in L2Encoder helper contract to pass to this method.
{% endhint %}

#### Input Parameters:

| Name | Type      | Description                                                                                                                                                                                                                                                                                      |
| :--- | :-------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| args | `bytes32` | <p>Arguments for the withdraw function packed in one bytes32<br>bit 0-15: `uint16` assetId - the index of the asset in the reservesList<br>bit 16-143: `uint128` shortenedAmount - cast to 256 bits at decode time, if `type(uint128).max` the value will be expanded to `type(uint256).max`</p> |

### borrow

```solidity
function borrow(bytes32 args) external override
```

Calldata efficient wrapper of the borrow function, borrowing on behalf of the caller. Borrows `amount` of `asset` with `interestRateMode`, sending the `amount` to `msg.sender`, with the debt being incurred by `onBehalfOf`.

{% hint style="info" %}
You can use data returned from [`encodeBorrowParams`]() method in L2Encoder helper contract to pass to this method.
{% endhint %}

#### Input Parameters:

| Name | Type      | Description                                                                                                                                                                                                                                                                                                                                                                                                                  |
| :--- | :-------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| args | `bytes32` | <p>Arguments for the borrow function packed in one bytes32<br>bit 0-15: `uint16` assetId - the index of the asset in the reservesList<br>bit 16-143: `uint128` shortenedAmount - cast to 256 bits at decode time, if `type(uint128).max` the value will be expanded to `type(uint256).max`<br>bit 144 - 151: `uint8` shortenedInterestRateMode<br>bit 152 - 167: `uint16` referralCode - used for 3rd party integrations</p> |

### repay

```solidity
function repay(bytes32 args) external override returns (uint256)
```

Calldata efficient wrapper of the repay function, repaying on behalf of the caller. Repays debt of an `asset` for the given `interestRateMode`.

{% hint style="info" %}
You can use data returned from [`encodeRepayParams`]() method in L2Encoder helper contract to pass to this method.
{% endhint %}

#### Input Parameters:

| Name | Type      | Description                                                                                                                                                                                                                                                                                                                                       |
| :--- | :-------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| args | `bytes32` | <p>Arguments for the repay function packed in one bytes32<br>bit 0-15: `uint16` assetId - the index of the asset in the reservesList<br>bit 16-143: `uint128` shortenedAmount - cast to 256 bits at decode time, if `type(uint128).max` the value will be expanded to `type(uint256).max`<br>bit 144 - 151: `uint8` shortenedInterestRateMode</p> |

#### Return Values:

| Type      | Description             |
| :-------- | :---------------------- |
| `uint256` | The final amount repaid |

### repayWithPermit

```solidity
function repayWithPermit(bytes32 args, bytes32 r, bytes32 s) external override returns (uint256)
```

Calldata efficient wrapper of the repayWithPermit function, repaying on behalf of the caller. Repay with transfer approval of borrowed asset via permit function. This method removes the need for separate approval transaction before repaying asset to the pool.

{% hint style="info" %}
You can use data returned from [`encodeRepayWithPermitParams`]() method in L2Encoder helper contract to pass to this method.​
{% endhint %}

#### Input Parameters:

| Name | Type      | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| :--- | :-------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| args | `bytes32` | <p>Arguments for the repayWithPermit function packed in one bytes32<br>bit 0-15: `uint16` assetId - the index of the asset in the reservesList<br>bit 16-143: `uint128` shortenedAmount - cast to 256 bits at decode time, if `type(uint128).max` the value will be expanded to `type(uint256).max`<br>bit 144 - 151: `uint8` shortenedInterestRateMode<br>bit 152-183: `uint32` shortenedDeadline - shortened deadline from original `uint256`<br>bit 184-191: `uint8` permitV - the V parameter of ERC712 permit signature</p> |
| r    | `bytes32` | The R parameter of ERC712 permit signature                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| s    | `bytes32` | The S parameter of ERC712 permit signature                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |

#### Return Values:

| Type      | Description             |
| :-------- | :---------------------- |
| `uint256` | The final amount repaid |

### repayWithATokens

```solidity
function repayWithATokens(bytes32 args) external override returns (uint256)
```

Calldata efficient wrapper of the repayWithATokens function. Allows user to repay with aTokens of the underlying debt asset without any approvals, for example, Pay DAI debt using aDAI tokens.

{% hint style="info" %}
You can use data data returned from [encodeRepayWithATokensParams]() method in L2Encoder helper contract to pass to this method.
{% endhint %}

#### Input Parameters:

| Name | Type      | Description                                                                                                                                                                                                                                                                                                                                                  |
| :--- | :-------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| args | `bytes32` | <p>Arguments for the repayWithATokens function packed in one bytes32<br>bit 0-15: `uint16` assetId - the index of the asset in the reservesList<br>bit 16-143: `uint128` shortenedAmount - cast to 256 bits at decode time, if `type(uint128).max` the value will be expanded to `type(uint256).max`<br>bit 144 - 151: `uint8` shortenedInterestRateMode</p> |

#### Return Values:

| Type      | Description             |
| :-------- | :---------------------- |
| `uint256` | The final amount repaid |

### swapBorrowRateMode

```solidity
function swapBorrowRateMode(bytes32 args) external override
```

Calldata efficient wrapper of the swapBorrowRateMode function. Swaps `msg.sender`'s borrow rate mode between stable and variable.

{% hint style="info" %}
You can use data returned from [`encodeSwapBorrowRateMode`]() method in L2Encoder helper contract to pass to this method.​
{% endhint %}

#### Input Parameters:

| Name | Type      | Description                                                                                                                                                                                           |
| :--- | :-------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| args | `bytes32` | <p>Arguments for the swapBorrowRateMode function packed in one bytes32<br>bit 0-15: `uint16` assetId - the index of the asset in the reservesList<br>bit 16-23: `uint8` shortenedInterestRateMode</p> |

### rebalanceStableBorrowRate

```solidity
function rebalanceStableBorrowRate(bytes32 args) external override
```

Calldata efficient wrapper of the rebalanceStableBorrowRate function. Rebalances stable borrow rate of the user for given asset. In case of liquidity crunches on the protocol, stable rate borrows might need to be rebalanced to bring back equilibrium between the borrow and supply rates.

{% hint style="info" %}
You can use data returned from [`encodeRebalanceStableBorrowRate`]() method in L2Encoder helper contract to pass to this method.​
{% endhint %}

#### Input Parameters:

| Name | Type      | Description                                                                                                                                                                              |
| :--- | :-------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| args | `bytes32` | <p>Arguments for the rebalanceStableBorrowRate function packed in one bytes32<br>bit 0-15: `uint16` assetId - the index of the asset in the reservesList<br>bit 16-175: user address</p> |

### setUserUseReserveAsCollateral

```solidity
function setUserUseReserveAsCollateral(bytes32 args) external override
```

Calldata efficient wrapper of the setUserUseReserveAsCollateral function. Sets the asset of `msg.sender` to be used as collateral or not.

{% hint style="info" %}
You can use data returned from [`encodeSetUserUseReserveAsCollateral`]() method in L2Encoder helper contract to pass to this method.​
{% endhint %}

#### Input Parameters:

| Name | Type      | Description                                                                                                                                                                                                                           |
| :--- | :-------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| args | `bytes32` | <p>Arguments for the setUserUseReserveAsCollateral function packed in one bytes32<br>bit 0-15: `uint16` assetId - the index of the asset in the reservesList<br>bit 16: 0 => enable useAsCollateral, 1 => disable useAsCollateral</p> |

### liquidationCall

```solidity
function liquidationCall(bytes32 args1, bytes32 args2) external override
```

Calldata efficient wrapper of the liquidationCall function. Liquidate positions with a health factor below 1.

{% hint style="info" %}
You can use data returned from [`encodeLiquidationCall`]() method in L2Encoder helper contract to pass to this method.​
{% endhint %}

#### Input Parameters:

| Name  | Type      | Description                                                                                                                                                                                                                                                                                                                  |
| :---- | :-------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| args1 | `bytes32` | <p>Part of the arguments for the liquidationCall function packed in one bytes32<br>bit 0-15: `uint16` collateralAssetId - the index of the collateral asset in the reservesList<br>bit 16-31: `uint16` debtAssetId - the index of the debt asset in the reservesList<br>bit 32-191: address of the user being liquidated</p> |
| args2 | `bytes32` | <p>Part of the arguments for the liquidationCall function packed in one bytes32<br>bit 0-127: `uint128` shortenedDebtToCover is cast to 256 bits at decode time, if `type(uint128).max` the value will be expanded to `type(uint256).max`<br>bit 128: receiveAToken - 0 => receive aToken, 1 => receive underlying asset</p> |

## ABI
<details>
<summary>L2Pool ABI</summary>

```
[
    {
        "inputs": [
            {
                "internalType": "contract IPoolAddressesProvider",
                "name": "provider",
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
                "name": "reserve",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "backer",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "fee",
                "type": "uint256"
            }
        ],
        "name": "BackUnbacked",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "reserve",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "address",
                "name": "user",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "onBehalfOf",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "enum DataTypes.InterestRateMode",
                "name": "interestRateMode",
                "type": "uint8"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "borrowRate",
                "type": "uint256"
            },
            {
                "indexed": true,
                "internalType": "uint16",
                "name": "referralCode",
                "type": "uint16"
            }
        ],
        "name": "Borrow",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "target",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "address",
                "name": "initiator",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "asset",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "enum DataTypes.InterestRateMode",
                "name": "interestRateMode",
                "type": "uint8"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "premium",
                "type": "uint256"
            },
            {
                "indexed": true,
                "internalType": "uint16",
                "name": "referralCode",
                "type": "uint16"
            }
        ],
        "name": "FlashLoan",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "asset",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "totalDebt",
                "type": "uint256"
            }
        ],
        "name": "IsolationModeTotalDebtUpdated",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "collateralAsset",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "debtAsset",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "user",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "debtToCover",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "liquidatedCollateralAmount",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "address",
                "name": "liquidator",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "bool",
                "name": "receiveAToken",
                "type": "bool"
            }
        ],
        "name": "LiquidationCall",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "reserve",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "address",
                "name": "user",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "onBehalfOf",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            },
            {
                "indexed": true,
                "internalType": "uint16",
                "name": "referralCode",
                "type": "uint16"
            }
        ],
        "name": "MintUnbacked",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "reserve",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "amountMinted",
                "type": "uint256"
            }
        ],
        "name": "MintedToTreasury",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "reserve",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "user",
                "type": "address"
            }
        ],
        "name": "RebalanceStableBorrowRate",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "reserve",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "user",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "repayer",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "bool",
                "name": "useATokens",
                "type": "bool"
            }
        ],
        "name": "Repay",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "reserve",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "liquidityRate",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "stableBorrowRate",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "variableBorrowRate",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "liquidityIndex",
                "type": "uint256"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "variableBorrowIndex",
                "type": "uint256"
            }
        ],
        "name": "ReserveDataUpdated",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "reserve",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "user",
                "type": "address"
            }
        ],
        "name": "ReserveUsedAsCollateralDisabled",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "reserve",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "user",
                "type": "address"
            }
        ],
        "name": "ReserveUsedAsCollateralEnabled",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "reserve",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "address",
                "name": "user",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "onBehalfOf",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            },
            {
                "indexed": true,
                "internalType": "uint16",
                "name": "referralCode",
                "type": "uint16"
            }
        ],
        "name": "Supply",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "reserve",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "user",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "enum DataTypes.InterestRateMode",
                "name": "interestRateMode",
                "type": "uint8"
            }
        ],
        "name": "SwapBorrowRateMode",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "user",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "uint8",
                "name": "categoryId",
                "type": "uint8"
            }
        ],
        "name": "UserEModeSet",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "reserve",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "user",
                "type": "address"
            },
            {
                "indexed": true,
                "internalType": "address",
                "name": "to",
                "type": "address"
            },
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            }
        ],
        "name": "Withdraw",
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
        "name": "BRIDGE_PROTOCOL_FEE",
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
        "name": "FLASHLOAN_PREMIUM_TOTAL",
        "outputs": [
            {
                "internalType": "uint128",
                "name": "",
                "type": "uint128"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "FLASHLOAN_PREMIUM_TO_PROTOCOL",
        "outputs": [
            {
                "internalType": "uint128",
                "name": "",
                "type": "uint128"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "MAX_NUMBER_RESERVES",
        "outputs": [
            {
                "internalType": "uint16",
                "name": "",
                "type": "uint16"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "MAX_STABLE_RATE_BORROW_SIZE_PERCENT",
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
        "name": "POOL_REVISION",
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
                "name": "fee",
                "type": "uint256"
            }
        ],
        "name": "backUnbacked",
        "outputs": [],
        "stateMutability": "nonpayable",
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
            },
            {
                "internalType": "address",
                "name": "onBehalfOf",
                "type": "address"
            }
        ],
        "name": "borrow",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "bytes32",
                "name": "args",
                "type": "bytes32"
            }
        ],
        "name": "borrow",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "uint8",
                "name": "id",
                "type": "uint8"
            },
            {
                "components": [
                    {
                        "internalType": "uint16",
                        "name": "ltv",
                        "type": "uint16"
                    },
                    {
                        "internalType": "uint16",
                        "name": "liquidationThreshold",
                        "type": "uint16"
                    },
                    {
                        "internalType": "uint16",
                        "name": "liquidationBonus",
                        "type": "uint16"
                    },
                    {
                        "internalType": "address",
                        "name": "priceSource",
                        "type": "address"
                    },
                    {
                        "internalType": "string",
                        "name": "label",
                        "type": "string"
                    }
                ],
                "internalType": "struct DataTypes.EModeCategory",
                "name": "category",
                "type": "tuple"
            }
        ],
        "name": "configureEModeCategory",
        "outputs": [],
        "stateMutability": "nonpayable",
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
                "internalType": "address",
                "name": "onBehalfOf",
                "type": "address"
            },
            {
                "internalType": "uint16",
                "name": "referralCode",
                "type": "uint16"
            }
        ],
        "name": "deposit",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "asset",
                "type": "address"
            }
        ],
        "name": "dropReserve",
        "outputs": [],
        "stateMutability": "nonpayable",
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
                "name": "from",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "to",
                "type": "address"
            },
            {
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "balanceFromBefore",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "balanceToBefore",
                "type": "uint256"
            }
        ],
        "name": "finalizeTransfer",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "receiverAddress",
                "type": "address"
            },
            {
                "internalType": "address[]",
                "name": "assets",
                "type": "address[]"
            },
            {
                "internalType": "uint256[]",
                "name": "amounts",
                "type": "uint256[]"
            },
            {
                "internalType": "uint256[]",
                "name": "interestRateModes",
                "type": "uint256[]"
            },
            {
                "internalType": "address",
                "name": "onBehalfOf",
                "type": "address"
            },
            {
                "internalType": "bytes",
                "name": "params",
                "type": "bytes"
            },
            {
                "internalType": "uint16",
                "name": "referralCode",
                "type": "uint16"
            }
        ],
        "name": "flashLoan",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "receiverAddress",
                "type": "address"
            },
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
                "internalType": "bytes",
                "name": "params",
                "type": "bytes"
            },
            {
                "internalType": "uint16",
                "name": "referralCode",
                "type": "uint16"
            }
        ],
        "name": "flashLoanSimple",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "asset",
                "type": "address"
            }
        ],
        "name": "getConfiguration",
        "outputs": [
            {
                "components": [
                    {
                        "internalType": "uint256",
                        "name": "data",
                        "type": "uint256"
                    }
                ],
                "internalType": "struct DataTypes.ReserveConfigurationMap",
                "name": "",
                "type": "tuple"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "uint8",
                "name": "id",
                "type": "uint8"
            }
        ],
        "name": "getEModeCategoryData",
        "outputs": [
            {
                "components": [
                    {
                        "internalType": "uint16",
                        "name": "ltv",
                        "type": "uint16"
                    },
                    {
                        "internalType": "uint16",
                        "name": "liquidationThreshold",
                        "type": "uint16"
                    },
                    {
                        "internalType": "uint16",
                        "name": "liquidationBonus",
                        "type": "uint16"
                    },
                    {
                        "internalType": "address",
                        "name": "priceSource",
                        "type": "address"
                    },
                    {
                        "internalType": "string",
                        "name": "label",
                        "type": "string"
                    }
                ],
                "internalType": "struct DataTypes.EModeCategory",
                "name": "",
                "type": "tuple"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "uint16",
                "name": "id",
                "type": "uint16"
            }
        ],
        "name": "getReserveAddressById",
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
        "inputs": [
            {
                "internalType": "address",
                "name": "asset",
                "type": "address"
            }
        ],
        "name": "getReserveData",
        "outputs": [
            {
                "components": [
                    {
                        "components": [
                            {
                                "internalType": "uint256",
                                "name": "data",
                                "type": "uint256"
                            }
                        ],
                        "internalType": "struct DataTypes.ReserveConfigurationMap",
                        "name": "configuration",
                        "type": "tuple"
                    },
                    {
                        "internalType": "uint128",
                        "name": "liquidityIndex",
                        "type": "uint128"
                    },
                    {
                        "internalType": "uint128",
                        "name": "currentLiquidityRate",
                        "type": "uint128"
                    },
                    {
                        "internalType": "uint128",
                        "name": "variableBorrowIndex",
                        "type": "uint128"
                    },
                    {
                        "internalType": "uint128",
                        "name": "currentVariableBorrowRate",
                        "type": "uint128"
                    },
                    {
                        "internalType": "uint128",
                        "name": "currentStableBorrowRate",
                        "type": "uint128"
                    },
                    {
                        "internalType": "uint40",
                        "name": "lastUpdateTimestamp",
                        "type": "uint40"
                    },
                    {
                        "internalType": "uint16",
                        "name": "id",
                        "type": "uint16"
                    },
                    {
                        "internalType": "address",
                        "name": "aTokenAddress",
                        "type": "address"
                    },
                    {
                        "internalType": "address",
                        "name": "stableDebtTokenAddress",
                        "type": "address"
                    },
                    {
                        "internalType": "address",
                        "name": "variableDebtTokenAddress",
                        "type": "address"
                    },
                    {
                        "internalType": "address",
                        "name": "interestRateStrategyAddress",
                        "type": "address"
                    },
                    {
                        "internalType": "uint128",
                        "name": "accruedToTreasury",
                        "type": "uint128"
                    },
                    {
                        "internalType": "uint128",
                        "name": "unbacked",
                        "type": "uint128"
                    },
                    {
                        "internalType": "uint128",
                        "name": "isolationModeTotalDebt",
                        "type": "uint128"
                    }
                ],
                "internalType": "struct DataTypes.ReserveData",
                "name": "",
                "type": "tuple"
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
            }
        ],
        "name": "getReserveNormalizedIncome",
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
        "inputs": [
            {
                "internalType": "address",
                "name": "asset",
                "type": "address"
            }
        ],
        "name": "getReserveNormalizedVariableDebt",
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
        "name": "getReservesList",
        "outputs": [
            {
                "internalType": "address[]",
                "name": "",
                "type": "address[]"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "user",
                "type": "address"
            }
        ],
        "name": "getUserAccountData",
        "outputs": [
            {
                "internalType": "uint256",
                "name": "totalCollateralBase",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "totalDebtBase",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "availableBorrowsBase",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "currentLiquidationThreshold",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "ltv",
                "type": "uint256"
            },
            {
                "internalType": "uint256",
                "name": "healthFactor",
                "type": "uint256"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "user",
                "type": "address"
            }
        ],
        "name": "getUserConfiguration",
        "outputs": [
            {
                "components": [
                    {
                        "internalType": "uint256",
                        "name": "data",
                        "type": "uint256"
                    }
                ],
                "internalType": "struct DataTypes.UserConfigurationMap",
                "name": "",
                "type": "tuple"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "user",
                "type": "address"
            }
        ],
        "name": "getUserEMode",
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
        "inputs": [
            {
                "internalType": "address",
                "name": "asset",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "aTokenAddress",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "stableDebtAddress",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "variableDebtAddress",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "interestRateStrategyAddress",
                "type": "address"
            }
        ],
        "name": "initReserve",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "contract IPoolAddressesProvider",
                "name": "provider",
                "type": "address"
            }
        ],
        "name": "initialize",
        "outputs": [],
        "stateMutability": "nonpayable",
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
        "name": "liquidationCall",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "bytes32",
                "name": "args1",
                "type": "bytes32"
            },
            {
                "internalType": "bytes32",
                "name": "args2",
                "type": "bytes32"
            }
        ],
        "name": "liquidationCall",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address[]",
                "name": "assets",
                "type": "address[]"
            }
        ],
        "name": "mintToTreasury",
        "outputs": [],
        "stateMutability": "nonpayable",
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
                "internalType": "address",
                "name": "onBehalfOf",
                "type": "address"
            },
            {
                "internalType": "uint16",
                "name": "referralCode",
                "type": "uint16"
            }
        ],
        "name": "mintUnbacked",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "bytes32",
                "name": "args",
                "type": "bytes32"
            }
        ],
        "name": "rebalanceStableBorrowRate",
        "outputs": [],
        "stateMutability": "nonpayable",
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
        "name": "rebalanceStableBorrowRate",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "bytes32",
                "name": "args",
                "type": "bytes32"
            }
        ],
        "name": "repay",
        "outputs": [
            {
                "internalType": "uint256",
                "name": "",
                "type": "uint256"
            }
        ],
        "stateMutability": "nonpayable",
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
                "internalType": "address",
                "name": "onBehalfOf",
                "type": "address"
            }
        ],
        "name": "repay",
        "outputs": [
            {
                "internalType": "uint256",
                "name": "",
                "type": "uint256"
            }
        ],
        "stateMutability": "nonpayable",
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
        "name": "repayWithATokens",
        "outputs": [
            {
                "internalType": "uint256",
                "name": "",
                "type": "uint256"
            }
        ],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "bytes32",
                "name": "args",
                "type": "bytes32"
            }
        ],
        "name": "repayWithATokens",
        "outputs": [
            {
                "internalType": "uint256",
                "name": "",
                "type": "uint256"
            }
        ],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "bytes32",
                "name": "args",
                "type": "bytes32"
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
        "name": "repayWithPermit",
        "outputs": [
            {
                "internalType": "uint256",
                "name": "",
                "type": "uint256"
            }
        ],
        "stateMutability": "nonpayable",
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
                "internalType": "address",
                "name": "onBehalfOf",
                "type": "address"
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
        "name": "repayWithPermit",
        "outputs": [
            {
                "internalType": "uint256",
                "name": "",
                "type": "uint256"
            }
        ],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "token",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "to",
                "type": "address"
            },
            {
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
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
                "internalType": "address",
                "name": "asset",
                "type": "address"
            }
        ],
        "name": "resetIsolationModeTotalDebt",
        "outputs": [],
        "stateMutability": "nonpayable",
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
                "components": [
                    {
                        "internalType": "uint256",
                        "name": "data",
                        "type": "uint256"
                    }
                ],
                "internalType": "struct DataTypes.ReserveConfigurationMap",
                "name": "configuration",
                "type": "tuple"
            }
        ],
        "name": "setConfiguration",
        "outputs": [],
        "stateMutability": "nonpayable",
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
                "name": "rateStrategyAddress",
                "type": "address"
            }
        ],
        "name": "setReserveInterestRateStrategyAddress",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "uint8",
                "name": "categoryId",
                "type": "uint8"
            }
        ],
        "name": "setUserEMode",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "bytes32",
                "name": "args",
                "type": "bytes32"
            }
        ],
        "name": "setUserUseReserveAsCollateral",
        "outputs": [],
        "stateMutability": "nonpayable",
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
        "name": "setUserUseReserveAsCollateral",
        "outputs": [],
        "stateMutability": "nonpayable",
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
                "internalType": "address",
                "name": "onBehalfOf",
                "type": "address"
            },
            {
                "internalType": "uint16",
                "name": "referralCode",
                "type": "uint16"
            }
        ],
        "name": "supply",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "bytes32",
                "name": "args",
                "type": "bytes32"
            }
        ],
        "name": "supply",
        "outputs": [],
        "stateMutability": "nonpayable",
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
                "internalType": "address",
                "name": "onBehalfOf",
                "type": "address"
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
        "name": "supplyWithPermit",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "bytes32",
                "name": "args",
                "type": "bytes32"
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
        "name": "supplyWithPermit",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "bytes32",
                "name": "args",
                "type": "bytes32"
            }
        ],
        "name": "swapBorrowRateMode",
        "outputs": [],
        "stateMutability": "nonpayable",
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
        "name": "swapBorrowRateMode",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "uint256",
                "name": "protocolFee",
                "type": "uint256"
            }
        ],
        "name": "updateBridgeProtocolFee",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "uint128",
                "name": "flashLoanPremiumTotal",
                "type": "uint128"
            },
            {
                "internalType": "uint128",
                "name": "flashLoanPremiumToProtocol",
                "type": "uint128"
            }
        ],
        "name": "updateFlashloanPremiums",
        "outputs": [],
        "stateMutability": "nonpayable",
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
                "internalType": "address",
                "name": "to",
                "type": "address"
            }
        ],
        "name": "withdraw",
        "outputs": [
            {
                "internalType": "uint256",
                "name": "",
                "type": "uint256"
            }
        ],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "bytes32",
                "name": "args",
                "type": "bytes32"
            }
        ],
        "name": "withdraw",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    }
]
```
</details>