# Best Practices

This guide covers essential best practices for contract development on zkSync Era, focusing on security and optimization.

### Using `call` over `.send` and `.transfer`

```solidity
// Avoid
payable(addr).send(x);
payable(addr).transfer(x);

// Recommended
(bool success, ) = addr.call{value: x}("");
require(success);
```

{% hint style="info" %}
**Note:** Using `call` exposes contracts to reentrancy attacks. Follow checks-effects-interactions pattern and utilize reentrancy guards.
{% endhint %}

### Early-Stage Proxy Pattern

zkSync Era is based on the zk-friendly VM. Thus, we offer [a dedicated compiler](https://era.zksync.io/docs/tools/compiler-toolchain/) responsible for transforming conventional Solidity and Vyper code into zkEVM bytecode.

To integrate a compiler bug fix, you need to recompile and upgrade your smart contract. We recommend using the Proxy pattern for a few months after your first deployment on zkSync Era, even if you plan to migrate to an immutable contract in the future.

* Utilize the [hardhat-zksync-upgradeable plugin](https://era.zksync.io/docs/api/hardhat/hardhat-zksync-upgradable.html) to manage proxy deployments.

### zkSync Gas Logic

zkSync Era uses different gas mechanics. Two key factors:

1. State-diff-based data availability
2. Unique computational trade-offs in zkEVM

{% hint style="danger" %}
**Warning:** Fee models are subject to change. Avoid hardcoding constants related to gas.
{% endhint %}

### Consider `gasPerPubdataByte`

zkSync Era transactions include a `gasPerPubdataByte` constant. Adjust your contracts to account for this, particularly for relayer-like logic.

**Example:**&#x20;

Gnosis Safe's `execTransaction` function should consider `gasPerPubdataByte` along with `gasleft()`.

```solidity
// We require some gas to emit the events (at least 2500) after the execution and some to perform code until the execution (500)
// We also include the 1/64 in the check that is not send along with a call to counteract potential shortcomings because of EIP-150
require(gasleft() >= ((safeTxGas * 64) / 63).max(safeTxGas + 2500) + 500, "GS010");
// Use scope here to limit variable lifetime and prevent `stack too deep` errors
{
    uint256 gasUsed = gasleft();
    // If the gasPrice is 0 we assume that nearly all available gas can be used (it is always more than safeTxGas)
    // We only substract 2500 (compared to the 3000 before) to ensure that the amount passed is still higher than safeTxGas
    success = execute(to, value, data, operation, gasPrice == 0 ? (gasleft() - 2500) : safeTxGas);
    gasUsed = gasUsed.sub(gasleft());

    // ...
}
```

While the contract does enforce the correct `gasleft()`, it does not enforce the correct `gasPerPubdata`, since there was no such parameter on Ethereum. This means that a malicious user could call this wallet when the `gasPerPubdata` is high and make the transaction fail, hence making it spend artificially more gas than required.

### Account Abstraction vs `ecrecover`

Use zkSync Era’s native account abstraction for signature validation instead of `ecrecover`. We recommend not relying on the fact that an account has an ECDSA private key, since the account may be governed by multisig and use another signature scheme.

### Local Testing

Before mainnet deployment, test contracts locally to minimize latency and costs. Check out the [testing](../../test-and-debug/) section for more info.
