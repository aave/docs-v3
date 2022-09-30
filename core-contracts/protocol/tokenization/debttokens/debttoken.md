# Debt Tokens

Debt tokens are interest-accruing tokens that are minted and burned on borrow and repay, representing the debt owed by the token holder. There are two types of debt tokens:
- [Stable Debt Tokens](./stabledebttoken.md): Represent a debt to the protocol with stable interest rate.
- [Variable Debt Tokens](./variabledebttoken.md): Represent a debt to the protocol with variable interest rate.

The stableDebtToken/variableDebtToken value is pegged 1:1 to the value of underlying borrowed asset and represents the current total amount owed to the protocol i.e. principal debt + interest accrued.

Although debt tokens are modelled on the ERC20/EIP20 standard, they are non-transferrable. Therefore they do not implement any of the standard ERC20/EIP20 functions relating to `transfer()` and `allowance()`.

The stable and variable debt tokens share a number of methods, these can be found in the [DebtTokenBase](../base/debttokenbase.md) contract.