# Provider

## `Provider` Class

#### Overview

The `Provider` class in zkSync Era extends the functionalities of `ethers.providers.JsonRpcProvider`. It is designed to interact with zkSync-specific methods while retaining all features of the Ethers.js provider.

#### `constructor` Method

The constructor initializes a new `Provider` object.

**Parameters**

| Parameter  | Type                          | Description                | Example                            |
| ---------- | ----------------------------- | -------------------------- | ---------------------------------- |
| `url?`     | string or `ConnectionInfo`    | Network RPC URL (optional) | `"https://testnet.era.zksync.dev"` |
| `network?` | `ethers.providers.Networkish` | Network name (optional)    | `"testnet"`                        |

**Example Usage**

{% code overflow="wrap" lineNumbers="true" %}
```typescript
import { Provider } from "zksync-web3";

const provider = new Provider("https://testnet.era.zksync.dev");
```
{% endcode %}

### **Estimation Methods**

The following methods are used for estimating gas on zkSync Era.

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

### `estimateGasL1` Method

#### Overview

The `estimateGasL1` method returns a `Promise` that resolves to a `BigNumber`, providing an estimate of the amount of gas required to submit a transaction from Layer 1 (L1) to Layer 2 (L2).

#### Method Signature

```typescript
provider.estimateGasL1(transaction: <TransactionRequest>): Promise<BigNumber>
```

**Parameters**

| Parameter     | Type                 | Description                                                                           |
| ------------- | -------------------- | ------------------------------------------------------------------------------------- |
| `transaction` | `TransactionRequest` | An object containing the details of the L1 to L2 transaction you want to estimate for |

**TransactionRequest Object**

| Property     | Type        | Description                                | Example                                                                                                                                |
| ------------ | ----------- | ------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| `to`         | `string`    | The recipient's address on L2              | `"0xYourL2AddressHere"`                                                                                                                |
| `data`       | `string`    | The transaction data                       | `"0xYourDataHere"`                                                                                                                     |
| `value`      | `BigNumber` | The value being sent in the transaction    | `parseEther("1.0")`                                                                                                                    |
| `customData` | `any`       | Custom data for the transaction (optional) | <pre class="language-typescript" data-overflow="wrap"><code class="lang-typescript">customData: { gasPerPubdataByte: 0 }
</code></pre> |

#### Return Value

Returns a `Promise` that resolves to a `BigNumber` representing the estimated gas cost for the L1 to L2 transaction.

#### Example Usage

Here's how you can use `estimateGasL1` to estimate the gas for a transaction from L1 to L2:

```typescript
import { Provider, utils } from "zksync-web3";
import { utils } from "ethers";

// Initialize a new Provider instance
const provider = new Provider("https://testnet.era.zksync.dev");

// Define the transaction details
const transaction = {
  to: "0x1111111111111111111111111111111111111111",
  data: "0xffffffff",
  value: utils.parseEther("1.0"),
  customData: { gasPerPubdataByte: 0 }
};

// Estimate the gas
const estimate = await provider.estimateGasL1(transaction);

// Log the estimated gas
console.log(`Estimated gas for L1 to L2 transaction: ${estimate}`);
```

### `estimateGasTransfer` Method

#### Overview

The `estimateGasTransfer` method returns a `Promise` that resolves to a `BigNumber`, providing an estimate of the amount of gas required to execute a token transfer transaction.

#### Method Signature

```typescript
provider.estimateGasTransfer(transaction: {
  to: Address;
  amount: BigNumberish;
  from?: Address;
  token?: Address;
  overrides?: ethers.CallOverrides;
}): Promise<BigNumber>
```

**Parameters**

| Parameter     | Type     | Description                                                                                 |
| ------------- | -------- | ------------------------------------------------------------------------------------------- |
| `transaction` | `Object` | An object containing the details of the token transfer transaction you want to estimate for |

**Transaction Object**

| Property    | Type                              | Description                     | Example                    |
| ----------- | --------------------------------- | ------------------------------- | -------------------------- |
| `to`        | `Address`                         | The recipient's address         | `"0xRecipientAddressHere"` |
| `amount`    | `BigNumberish`                    | The amount of token to transfer | `1000`                     |
| `from`      | `Address` (Optional)              | The sender's address            | `"0xSenderAddressHere"`    |
| `token`     | `Address` (Optional)              | The token's contract address    | `"0xTokenAddressHere"`     |
| `overrides` | `ethers.CallOverrides` (Optional) | Ethers call overrides object    | `{ gasLimit: 21000 }`      |

#### Return Value

Returns a `Promise` that resolves to a `BigNumber` representing the estimated gas cost for the token transfer transaction.

#### Example Usage

Here's how you can use `estimateGasTransfer` to estimate the gas for a token transfer transaction:

```typescript
import { Provider } from "zksync-web3";
import { BigNumber } from "ethers";

// Initialize a new Provider instance
const provider = new Provider("https://testnet.era.zksync.dev");

// Define the transaction details
const transaction = {
  to: "0xRecipientAddressHere",
  amount: BigNumber.from("1000"),
  from: "0xSenderAddressHere",
  token: "0xTokenAddressHere",
  overrides: { gasLimit: 21000 }
};

// Estimate the gas
const estimate = await provider.estimateGasTransfer(transaction);

// Log the estimated gas
console.log(`Estimated gas for token transfer: ${estimate}`);
```



\


