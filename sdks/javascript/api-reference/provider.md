# Provider

### `Provider` Class

#### Overview

The `Provider` class in zkSync Era extends the functionalities of `ethers.providers.JsonRpcProvider`. It is designed to interact with zkSync-specific methods while retaining all features of the Ethers.js provider.

#### `constructor` Method

The constructor initializes a new `Provider` object.

**Parameters**

| Parameter  | Type                          | Description                | Example                            |
| ---------- | ----------------------------- | -------------------------- | ---------------------------------- |
| `url?`     | string or `ConnectionInfo`    | Network RPC URL (optional) | `"https://testnet.era.zksync.dev"` |
| `network?` | `ethers.providers.Networkish` | Network name (optional)    | `"testnet"`                        |

{% code overflow="wrap" lineNumbers="true" %}
```typescript
// Constructor method signature
constructor(url?: ConnectionInfo | string, network?: ethers.providers.Networkish) {
    super(url, network);  // Call to the superclass constructor
    this.pollingInterval = 500;  // Set the polling interval

    // Customize the blockTag formatter
    const blockTag = this.formatter.blockTag.bind(this.formatter);
    this.formatter.blockTag = (tag: any) => {
        if (tag === 'committed' || tag === 'finalized') {
            return tag;
        }
        return blockTag(tag);
    };

    this.contractAddresses = {};  // Initialize contract addresses
    this.formatter.transaction = parseTransaction;  // Set the transaction formatter
}

```
{% endcode %}

**Example Usage**

{% code overflow="wrap" lineNumbers="true" %}
```typescript
import { Provider } from "zksync-web3";

const provider = new Provider("https://testnet.era.zksync.dev");
```
{% endcode %}

### `estimateGas` Method

#### Overview

The `estimateGas` method returns a `Promise` that resolves to a `BigNumber` representing an estimate of the amount of gas required to submit a given transaction to the network.

{% hint style="info" %}
Keep in mind that the estimate may not be entirely accurate. Network conditions, such as other transactions being mined, can affect the actual gas required.
{% endhint %}

#### Method Signature

```typescript
provider.estimateGas(transaction: TransactionRequest): Promise<BigNumber>
```

**Parameters**

| Parameter     | Type                 | Description                                                                  |
| ------------- | -------------------- | ---------------------------------------------------------------------------- |
| `transaction` | `TransactionRequest` | An object containing the details of the transaction you want to estimate for |

**TransactionRequest Object**

| Property | Type        | Description                             | Example                                        |
| -------- | ----------- | --------------------------------------- | ---------------------------------------------- |
| `to`     | `string`    | The recipient's address                 | `"0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2"` |
| `data`   | `string`    | The transaction data                    | `"0xd0e30db0"`                                 |
| `value`  | `BigNumber` | The value being sent in the transaction | `parseEther("1.0")`                            |

#### Return Value

Returns a `Promise` that resolves to a `BigNumber` representing the estimated gas cost.

#### Example Usage&#x20;

Here's how you can use `estimateGas` to estimate the gas for a deposit transaction:

{% code overflow="wrap" lineNumbers="true" %}
```typescript
import { Provider } from "zksync-web3";

const provider = new Provider("https://testnet.era.zksync.dev");

const estimate = await provider.estimateGas({
  // Wrapped ETH address
  to: "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",

  // `function deposit() payable`
  data: "0xd0e30db0",

  // 1 ether
  value: parseEther("1.0")
});

console.log(estimate);
```
{% endcode %}

#### Try it out!

{% embed url="https://stackblitz.com/edit/typescript-m4nvot?file=index.ts" %}
