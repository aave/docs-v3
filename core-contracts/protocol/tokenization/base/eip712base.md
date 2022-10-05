# EIP712Base

The base contract implementation of EIP712.

## Write Methods

### DOMAIN_SEPARATOR

```solidity
function DOMAIN_SEPARATOR() public view virtual returns (bytes32)
```

Get the domain separator for the token. Return cached value if chainId matches cache, otherwise recomputes separator.

#### Return Values:

| Type      | Description                                        |
| :-------- | :------------------------------------------------- |
| `bytes32` | The domain separator of the token at current chain |

## View Methods

### nonces

```solidity
function nonces(address owner) public view virtual returns (uint256)
```

Returns the nonce value for address specified as parameter

#### Input Parameters:

| Name  | Type      | Description                                       |
| :---- | :-------- | :------------------------------------------------ |
| owner | `address` | The address for which the nonce is being returned |

#### Return Values:

| Type      | Description                            |
| :-------- | :------------------------------------- |
| `uint256` |  The nonce value for the input address |

## ABI
<details>
<summary>EIP712Base ABI</summary>

```
[
    {
        "inputs": [],
        "name": "DOMAIN_SEPARATOR",
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
        "inputs": [],
        "name": "EIP712_REVISION",
        "outputs": [
            {
                "internalType": "bytes",
                "name": "",
                "type": "bytes"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "owner",
                "type": "address"
            }
        ],
        "name": "nonces",
        "outputs": [
            {
                "internalType": "uint256",
                "name": "",
                "type": "uint256"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    }
]
```
</details>