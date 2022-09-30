# PriceOracleSentinel

This contract validates if the operations are allowed depending on the `PriceOracle` health.

Once the `PriceOracle` gets up after an outage or downtime, users can make their positions healthy during a grace period. The `PriceOracle` is considered healthy once its completely up and the grace period has passed.

## Write Methods

### setSequencerOracle

```solidity
function setSequencerOracle(address newSequencerOracle) public onlyPoolAdmin
```

Updates the address of the sequencer oracle.

{% hint style="warning" %}
Can only be called by PoolAdmin.
{% endhint %}

#### Input Parameters:

| Name               | Type      | Description                                       |
| :----------------- | :-------- | :------------------------------------------------ |
| newSequencerOracle | `address` | The address of the new Sequencer Oracle to be set |

### setGracePeriod

```solidity
function setGracePeriod(uint256 newGracePeriod) public onlyRiskOrPoolAdmins
```

Updates the duration of the grace period.

{% hint style="warning" %}
Can only be called by PoolAdmin or RiskAdmin.
{% endhint %}

#### Input Parameters:

| Name           | Type      | Description                                 |
| :------------- | :-------- | :------------------------------------------ |
| newGracePeriod | `uint256` | The duration of new grace period in seconds |

## View Methods

### isBorrowAllowed

```solidity
function isBorrowAllowed() public view override returns (bool)
```

Returns true if the `borrow` operation is allowed. The operation is not allowed when `PriceOracle` is down or the grace period has not passed.

#### Return Values:

| Type   | Description                                                                                                            |
| :----- | :--------------------------------------------------------------------------------------------------------------------- |
| `bool` | Returns true if the `borrow` operation is allowed (the PriceOracle is up and grace period has passed), false otherwise |

### isLiquidationAllowed

```solidity
function isLiquidationAllowed() public view override returns (bool)
```

Returns true if the `liquidation` operation is allowed. The operation is not allowed when `PriceOracle` is down or the grace period has not passed.

#### Return Values:

| Type   | Description                                                                                                            |
| :----- | :--------------------------------------------------------------------------------------------------------------------- |
| `bool` | Returns true if the `liquidation` operation is allowed (the PriceOracle is up and grace period has passed), false otherwise |

### getSequencerOracle

```solidity
function getSequencerOracle() public view returns (address)
```

Returns the SequencerOracle.

#### Return Values: 

| Type      | Description                                  |
| :-------- | :------------------------------------------- |
| `address` | The address of the sequencer oracle contract |

### getGracePeriod

```solidity
function getGracePeriod() public view returns (uint256)
```

Returns the grace period.

#### Return Values:

Returns the grace period.
| Type      | Description                                 |
| :-------- | :------------------------------------------ |
| `uint256` | The duration of the grace period in seconds |

## ABI
<details>
<summary>PriceOracleSentinel ABI</summary>

```
[
    {
        "inputs": [
            {
                "internalType": "contract IPoolAddressesProvider",
                "name": "provider",
                "type": "address"
            },
            {
                "internalType": "contract ISequencerOracle",
                "name": "oracle",
                "type": "address"
            },
            {
                "internalType": "uint256",
                "name": "gracePeriod",
                "type": "uint256"
            }
        ],
        "stateMutability": "nonpayable",
        "type": "constructor"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": false,
                "internalType": "uint256",
                "name": "newGracePeriod",
                "type": "uint256"
            }
        ],
        "name": "GracePeriodUpdated",
        "type": "event"
    },
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": false,
                "internalType": "address",
                "name": "newSequencerOracle",
                "type": "address"
            }
        ],
        "name": "SequencerOracleUpdated",
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
        "name": "getGracePeriod",
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
        "name": "getSequencerOracle",
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
        "name": "isBorrowAllowed",
        "outputs": [
            {
                "internalType": "bool",
                "name": "",
                "type": "bool"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "isLiquidationAllowed",
        "outputs": [
            {
                "internalType": "bool",
                "name": "",
                "type": "bool"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "uint256",
                "name": "newGracePeriod",
                "type": "uint256"
            }
        ],
        "name": "setGracePeriod",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "newSequencerOracle",
                "type": "address"
            }
        ],
        "name": "setSequencerOracle",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    }
]
```
</details>