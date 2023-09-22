# hardhat-zksync-solc

### **What is this Plugin?**

The [`hardhat-zksync-solc`](https://www.npmjs.com/package/@matterlabs/hardhat-zksync-solc) plugin provides an interface for compiling Solidity smart contracts for deployment on the zkSync Era.

{% hint style="info" %}
To gain an understanding **why** solc plugin is needed for zkSync compilation please refer to the documentation [here](https://era.zksync.io/docs/tools/compiler-toolchain/overview.html).
{% endhint %}

### **Installation**

Add the latest version of the plugin to your project:

{% tabs %}
{% tab title="yarn" %}
```bash
yarn add -D @matterlabs/hardhat-zksync-solc
```
{% endtab %}

{% tab title="npm" %}
```bash
npm i -D @matterlabs/hardhat-zksync-solc
```
{% endtab %}
{% endtabs %}

### Configuration

Import the package in your `hardhat.config.ts`:

```typescript
import "@matterlabs/hardhat-zksync-solc";
```

**Default configuration**&#x20;

From version `0.4.0` onwards, a default configuration has been introduced, making all parameters optional. Override default configurations by modifying values inside the `hardhat.config.ts` file.

<details>

<summary>Minimal config with default configuration</summary>

```typescript
import { HardhatUserConfig } from "hardhat/config";

import "@matterlabs/hardhat-zksync-deploy";
import "@matterlabs/hardhat-zksync-solc";

const config: HardhatUserConfig = {
  zksolc: {},
  defaultNetwork: "zkSyncTestnet",
  networks: {
    hardhat: {
      zksync: false,
    },
    zkSyncTestnet: {
      url: "http://localhost:3050",
      ethNetwork: "http://localhost:8545",
      zksync: true,
    };
  },
  solidity: {
    version: "0.8.20",
  },
};

export default config;
```

</details>

#### Advanced configuration

Inside the `hardhat.config.ts` file, add configuration parameters within a `zksolc` property.

<details>

<summary>Example advanced config</summary>

```typescript
zksolc: {
    version: "latest", // optional.
    settings: {
      compilerPath: "zksolc",  // optional. Ignored for compilerSource "docker". Can be used if compiler is located in a specific folder
      libraries:{}, // optional. References to non-inlinable libraries
      missingLibrariesPath: "./.zksolc-libraries-cache/missingLibraryDependencies.json" // optional. This path serves as a cache that stores all the libraries that are missing or have dependencies on other libraries. A `hardhat-zksync-deploy` plugin uses this cache later to compile and deploy the libraries, especially when the `deploy-zksync:libraries` task is executed
      isSystem: false, // optional.  Enables Yul instructions available only for zkSync system contracts and libraries
      forceEvmla: false, // optional. Falls back to EVM legacy assembly if there is a bug with Yul
      optimizer: {
        enabled: true, // optional. True by default
        mode: '3' // optional. 3 by default, z to optimize bytecode size
      },
    }
},

```

</details>

Configuration options and their details:

* `version`: Specifies the zksolc compiler version. [View available versions](https://chat.openai.com/c/bbd6cd82-dec2-4f11-9e2d-3a2fd63b2f0b).
* `compilerSource`: Choose between binary (default) and docker (deprecated).
* `libraries`: Define any non-inlinable libraries used by your contract. [Learn more](https://chat.openai.com/c/bbd6cd82-dec2-4f11-9e2d-3a2fd63b2f0b).
* `missingLibrariesPath`: Cache for missing libraries and dependencies.
* `isSystem`: For contracts utilizing zkSync-specific Yul instructions.
* `forceEvmla`: Use EVM legacy assembly if there are Yul IR issues.
* `optimizer`: Compiler optimization settings.
* `metadata`: Metadata settings. If the option is omitted, the metadata hash appends by default:
  * `bytecodeHash`: Can only be `none`. It removes metadata hash from the bytecode.

{% hint style="danger" %}
**`forceEvmla`**

Setting the `forceEvmla` field to `true` can have the following negative impacts:

* No support for recursion.
* No support for internal function pointers.
* Possible contract size and performance impact.
* For Solidity <0.8, it's the default and only mode.
{% endhint %}

#### Network configuration

Configure the `zksync` parameter in the networks to enable the zksolc compiler:

```ts
defaultNetwork: "zkSyncTestnet",
networks: {
  goerli: {
    url: "https://goerli.infura.io/v3/<API_KEY>", // The Ethereum Web3 RPC URL (optional).
    zksync: false, // disables zksolc compiler
  },
  zkSyncTestnet: {
    url: "https://testnet.era.zksync.dev", // The testnet RPC URL of zkSync Era network.
    ethNetwork: "goerli", // The Ethereum Web3 RPC URL, or the identifier of the network (e.g. `mainnet` or `goerli`)
    zksync: true, // enables zksolc compiler
  }
},
```

The `zksync`network option determines if the `zksolc` compiler is active for a particular network. It is set to `false` by default. This option is especially beneficial for multi-chain projects, allowing you to selectively enable `zksync` for specific networks.

### Usage

{% tabs %}
{% tab title="yarn" %}
```bash
yarn hardhat compile
```
{% endtab %}

{% tab title="npm" %}
```bash
npx hardhat compile
```
{% endtab %}
{% endtabs %}

Compiles all the smart contracts in the `contracts` directory and creates the `artifacts-zk` folder with all the compilation artifacts, including factory dependencies for the contracts, which could be used for contract deployment.

### **Troubleshooting**

* **Issue**: Error from `@matterlabs/hardhat-zksync-solc`: Invalid zksolc compiler version.
  * **Solution**: Check the version in `hardhat.config.ts` isn't below minimum. [Check versions](https://github.com/matter-labs/zksolc-bin/blob/main/version.json).
* **Issue**: Unexpected end of JSON input during compilation.
  * **Solution**:
    1. Update `@matterlabs/hardhat-zksync-solc`.
    2. Recompile.
    3. If "Library not found" error appears,[ ](https://era.zksync.io/docs/tools/hardhat/compiling-libraries.html)[follow these instructions.](https://era.zksync.io/docs/tools/hardhat/compiling-libraries.html)
