# Collector

Stores the fees collected by the protocol and allows the fund administrator to approve or transfer the collected ERC20 tokens.

Implementation contract that must be initialized using transparent proxy pattern.

## Write Methods

### initialize

```solidity
function initialize(address reserveController) external initializer
```

Initialize the transparent proxy with the admin of the Collector.

#### Input Parameters:

| Name              | Type      | Description                                      |
| :---------------- | :-------- | :----------------------------------------------- |
| reserveController | `address` | The address of the admin that controls Collector | 

### approve

```solidity
function approve(IERC20 token, address recipient, uint256 amount) external onlyFundsAdmin
```

Approve an amount of tokens to be pulled by the recipient.

#### Input Parameters:

| Name      | Type      | Description                                                                 |
| :-------- | :-------- | :-------------------------------------------------------------------------- |
| token     | `IERC20`  | The address of the asset                                                    | 
| recipient | `address` | The address of the entity allowed to pull tokens                            | 
| amount    | `uint256` | The amount allowed to be pulled. If set to `0`, it will revoke the approval | 

### transfer

```solidity
function transfer(IERC20 token, address recipient, uint256 amount) external onlyFundsAdmin 
```

Transfer an amount of tokens to the recipient.

#### Input Parameters:

| Name      | Type      | Description                                      |
| :-------- | :-------- | :----------------------------------------------- |
| token     | `IERC20`  | The address of the asset                         | 
| recipient | `address` | The address of the entity to transfer the tokens | 
| amount    | `uint256` | The amount to be transferred                     | 

### setFundsAdmin

```solidity
function setFundsAdmin(address admin) external onlyFundsAdmin
```

Transfer the ownership of the funds administrator role. This function should only be callable by the current funds administrator. 

#### Input Parameters:

| Name  | Type      | Description                                |
| :---- | :-------- | :----------------------------------------- |
| admin | `address` | The address of the new funds administrator | 

## View Methods

### getFundsAdmin

```solidity
function getFundsAdmin() external view returns (address)
```

Retrieve the current funds administrator.

#### Return Values:

| Type      | Description                            |
| :-------- | :------------------------------------- |
| `address` | The address of the funds administrator | 

## Pure Methods

### getRevision

```solidity
function getRevision() internal pure override returns (uint256)
```

Returns the revision number of the contract. Needs to be defined in the inherited class as a constant.

#### Return Values:

| Type      | Description         |
| :-------- | :------------------ |
| `uint256` | The revision number | 

## ABI
<details>
<summary>Collector ABI</summary>

```

[
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "internalType": "address",
                "name": "fundsAdmin",
                "type": "address"
            }
        ],
        "name": "NewFundsAdmin",
        "type": "event"
    },
    {
        "inputs": [],
        "name": "REVISION",
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
                "internalType": "contract IERC20",
                "name": "token",
                "type": "address"
            },
            {
                "internalType": "address",
                "name": "recipient",
                "type": "address"
            },
            {
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            }
        ],
        "name": "approve",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "getFundsAdmin",
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
                "name": "reserveController",
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
                "name": "admin",
                "type": "address"
            }
        ],
        "name": "setFundsAdmin",
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
            },
            {
                "internalType": "address",
                "name": "recipient",
                "type": "address"
            },
            {
                "internalType": "uint256",
                "name": "amount",
                "type": "uint256"
            }
        ],
        "name": "transfer",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    }
]
```
</details>