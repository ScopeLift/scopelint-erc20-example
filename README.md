# Scopelint ERC-20 Example

A sample repository demonstrating `scopelint check` and `scopelint spec` usage.

This contains a simple ERC-20 token contract, and an incomplete test suite for it.
The test contracts and names are written following the [Best Practices guide](https://book.getfoundry.sh/tutorials/best-practices).
Note that two ways to organize tests are mentioned, and scopelint requires the "treat contracts as describe blocks" approach.


## `scopelint check`

First, run `scopelint check` against this repository and you'll see the following output.
(Beneath the gif is the output as text to facilitate reading).

![](./assets/check.gif)

```
$ scopelint check
Invalid constant or immutable name in ./src/ERC20.sol on line 29: decimals
Invalid src method name in ./src/ERC20.sol on line 170: computeDomainSeparator
error: Convention checks failed, see details above
```

There are two problems found:
1. The variable `decimals` is immutable, so it should be called `DECIMALS`.
2. The method `computeDomainSeparator` is an `internal` method, so it should be called `_computeDomainSeparator`.

Fix both of those issues, and run `scopelint check` again—you should see no errors.

## `scopelint spec`

Now run `scopelint spec` against this repository and you'll see the following output.
(Beneath the gif is the output as text to facilitate reading).

![](./assets/spec.gif)

```
$ scopelint spec

Contract Specification: ERC20
├── constructor
│   ├──  Stored Name Matches Constructor Input
│   ├──  Stored Symbol Matches Constructor Input
│   ├──  Stored Decimals Matches Constructor Input
│   ├──  Sets Initial Chain Id
│   └──  Sets Initial Domain Separator
├── approve
│   ├──  Sets Allowance Mapping To Approved Amount
│   ├──  Returns True For Successful Approval
│   └──  Emits Approval Event
├── transfer
│   ├──  Revert If: Spender Has Insufficient Balance
│   ├──  Does Not Change Total Supply
│   ├──  Increases Recipient Balance By Sent Amount
│   ├──  Decreases Sender Balance By Sent Amount
│   ├──  Returns True
│   └──  Emits Transfer Event
├── transferFrom
├── permit
├── DOMAIN_SEPARATOR
├── computeDomainSeparator
├── _mint
└── _burn
```

The ERC20 contract's `constructor`, `approve`, and `transfer` methods have all been specified based on test names. The `transferFrom`, `permit`, `DOMAIN_SEPARATOR`, `computeDomainSeparator`, `_mint`, and `_burn` methods do not have tests, so they show up as red in the output.

The order of functions matches the source ERC20 contract, and the order of requirements matches the order of tests.