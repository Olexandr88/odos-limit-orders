# Odos Limit Order Smart Contract

## Summary

This product allows users to place orders that will be executed at a specified limit price. Orders are placed gaslessly via signed EIP-712 data, and support multiple inputs and multiple outputs.

### Limit order execution flow

1. Check if msg.sender is allowed
2. Check if order still valid
3. Check tokens, amounts
4. Get order hash
5. Recover order owner account [and validate the signature]
6. Extract previously filled amounts for order from storage, or create
7. Check if fill possible:
  - If partiallyFillable, total amount do not exceed
  - If not partiallyFillable - it was not filled previously
8. Transfer tokens from order owner
9. Update order filled amounts in storage
10. Get output token balances before
11. Execute path with the OdosExecutor
12. Get output token balances difference
13. Calculate and transfer referral fee if any
14. Check slippage, adjust amountOut
15. Check surplus
16. Transfer tokens to order owner account
17. Emit LimitOrderFilled (MultiLimitOrderFilled) event

## Deployment Addresses

| Chain | Limit Order Router Address |
| :-: | :-: |
| <img src="https://assets.odos.xyz/chains/optimism.png" width="50" height="50"><br>Optimism | [`0xDA381f498Ae67cBA020D0cD00d89bC7B0829e700`](https://optimistic.etherscan.io/address/0xda381f498ae67cba020d0cd00d89bc7b0829e700) |
| <img src="https://assets.odos.xyz/chains/base.png" width="50" height="50"><br>Base | [`0xb1949c1E89Ea487C378D85d76C07d3D113AA4EF2`](https://basescan.org/address/0xb1949c1e89ea487c378d85d76c07d3d113aa4ef2) |
| <img src="https://assets.odos.xyz/chains/mode.png" width="50" height="50"><br>Mode | [`0xcAaf61d47b9D3eCb1765B10F085E542CE0EecCEf`](https://explorer.mode.network/address/0xcAaf61d47b9D3eCb1765B10F085E542CE0EecCEf) |
| <img src="https://assets.odos.xyz/chains/arbitrum.png" width="50" height="50"><br>Arbitrum | [`0x09cF9441285CE79B4896F5a533d2C2eeB4D368Eb`](https://arbiscan.io/address/0x09cF9441285CE79B4896F5a533d2C2eeB4D368Eb) |

## Smart Contracts

### Build

```shell
forge build
```

### Test

```shell
forge test
```

Test the EIP-712 hash generation to ensure that the Solidity hash is generated correctly.

```shell
cd test/eip712test
chmod +x ./runTest.sh
./runTest.sh
```

### Gas report

```shell
forge test --gas-report
```

### Linter

```
npm install -g solhint
solhint 'contracts/*.sol' 'interfaces/*.sol'
```

### Coverage

```shell
forge coverage --report lcov
genhtml -o report lcov.info
```

Then open the `report/index.html` in the browser.


### Audit tools

Install [Mythril](https://github.com/Consensys/mythril)

```shell
solc-select install 0.8.19
export SOLC_VERSION=0.8.19
myth analyze contracts/OdosLimitOrderRouter.sol --solc-json ./mythril.config.json
```

## Audit

This contract was audited by [Halborn](https://www.halborn.com/) in April 2024. A link to the report can be found on the [Halborn Website](https://www.halborn.com/audits/odos/limit-orders). A copy of the report is included on this page.
