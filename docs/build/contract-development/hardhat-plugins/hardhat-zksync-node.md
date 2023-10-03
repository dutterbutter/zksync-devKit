# hardhat-zksync-node

## `hardhat-zksync-node` <a href="#user-content-hardhat-zksync-node" id="user-content-hardhat-zksync-node"></a>

This plugin is used to provide a convenient way to run zkSync Era [In-memory node](https://github.com/matter-labs/zksync-web-era-docs/blob/mpopovac-txfusion-hardhat-zksync-node/docs/tools/testing/era-test-node.md) locally using hardhat.

### Installation <a href="#user-content-installation" id="user-content-installation"></a>

[@matterlabs/hardhat-zksync-node](https://www.npmjs.com/package/@matterlabs/hardhat-zksync-node)

Add the latest version of this plugin to your project with the following command:

::: code-tabs

@tab:active yarn

```
yarn add -D @matterlabs/hardhat-zksync-node
```

@tab npm

```
npm i -D @matterlabs/hardhat-zksync-node
```

:::

#### Configuration <a href="#user-content-configuration" id="user-content-configuration"></a>

Import the plugin in the `hardhat.config.ts` file:

```
import "@matterlabs/hardhat-zksync-node";
```

#### Commands <a href="#user-content-commands" id="user-content-commands"></a>

```
yarn hardhat node-zksync
```

This command runs a local zkSync In-memory node by initiating a JSON-RPC server. It uses the provided or default configurations to set up and run the zkSync node, allowing for blockchain operations in a local environment. The command also handles tasks such as downloading the necessary JSON-RPC server binary if it's not already present.

* `--port` - Port on which the server should listen. Defaults to 8011.
* `--log` - Log filter level. Accepted values are: error, warn, info, and debug. Defaults to info.
* `--log-file-path` - Path to the file where logs should be written. Defaults to `era_test_node.log`.
* `--cache` - Type of cache to use. Accepted values are: none, disk, and memory. Defaults to disk.
* `--cache-dir` - Directory location for the `disk` cache. Defaults to `.cache`.
* `--reset-cache` - Flag to reset the local `disk` cache.
* `--show-calls` - Determines which call debug information to show. Accepted values are: none, user, system, and all. Defaults to none.
* `--show-storage-logs` - Determines which storage logs to show. Accepted values are: none, read, write, and all. Defaults to none.
* `--show-vm-details` - Specifies the level of Virtual Machine (VM) details to show. Accepted values are: none and all. Defaults to none.
* `--show-gas-details` - Specifies the level of gas details to show. Accepted values are: none and all. Defaults to none.
* `--resolve-hashes` - Flag to try contacting openchain to resolve the ABI & topic names. When enabled, it makes the debug log more readable but might decrease performance.
* `--dev-use-local-contracts` - Flag to load locally compiled system contracts. Useful when making changes to system contracts or bootloader.
* `--fork` - Starts a local network that is a fork of another network. Accepted values are: testnet, mainnet, or a specific URL.
* `--fork-block-number` - Specifies the block height at which to fork.
* `--replay-tx` - Transaction hash to replay.

::: warning Parameter Restrictions

The `--replay-tx` and `--fork-block-number` parameters cannot be specified simultaneously. The `--replay-tx` is used for replaying a remote transaction locally for deep debugging, while `--fork-block-number` is used for forking the blockchain at a specified block number. Combining these actions is not supported.\
Additionally, if either `--replay-tx` or `--fork-block-number` is specified, the `--fork` parameter must also be provided.

:::

::: tip Learn More

If you wish to learn more about replaying transactions or forking, check out the [In-memory node documentation](https://github.com/matter-labs/zksync-web-era-docs/blob/mpopovac-txfusion-hardhat-zksync-node/docs/tools/testing/era-test-node.md).

:::

::: tip Supported APIs

To see a list of all supported APIs, visit [this link ](https://github.com/matter-labs/era-test-node/blob/main/SUPPORTED\_APIS.md).

:::
