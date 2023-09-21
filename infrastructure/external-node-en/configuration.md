# Configuration

The EN provides a number of configuration options which primarily depend on environment variables. This document outlines each of these options, making your setup seamless.

### Default Configurations

To kickstart your setup process, we have provided default configurations tailored for both the zkSync Era [mainnet](https://github.com/matter-labs/zksync-era/blob/main/docs/external-node/prepared\_configs/mainnet-config.env) and [testnet](https://github.com/matter-labs/zksync-era/blob/main/docs/external-node/prepared\_configs/testnet-config.env). We recommend using these predefined configurations as a foundation and making necessary tweaks as required.

### Database Configuration

The EN integrates with two databases:

#### 1. **PostgreSQL**:

* **Role**: Acts as the primary repository for data, catering to API requests.
* **Configuration Variables**:
  * `DATABASE_URL`: Defines the connection to PostgreSQL.
  * `DATABASE_POOL_SIZE`: Determines the size of the connection pool.

#### 2. **RocksDB**:

* **Role**: Used in components such as the State Keeper and Merkle tree, primarily where IO operations are frequent.
* **Recommendation**: If feasible, opt for an NVME SSD to host RocksDB.
* **Configuration Variables**:
  * `EN_STATE_CACHE_PATH`: Directory path for the state cache.
  * `EN_MERKLE_TREE_PATH`: Directory path for the Merkle tree. This should differ from the state cache path.

### L1 Web3 Client Configuration

The EN necessitates a connection to an Ethereum node.

* **Configuration Variable**: `EN_ETH_CLIENT_URL` - Set this to match the corresponding L1 network (e.g., L1 mainnet for L2 mainnet and L1 goerli for L2 testnet).

{% hint style="info" %}
Typically, the EN makes two requests to L1 per batch. While this ensures relatively low usage for a synced node, the initial synchronization phase may see increased activity. If using services like Infura, monitor your request limits.
{% endhint %}

### Port Configuration

When leveraging the Docker version, the EN exposes multiple ports:

* **HTTP JSON-RPC**: 3060
* **WebSocket JSON-RPC**: 3061
* **Prometheus listener**: 3322
* **Healthcheck server**: 3081

{% hint style="info" %}
Due to a bug in an external metrics library, if the Prometheus port is activated, ensure you scrape it routinely to circumvent potential memory leaks. If you don't plan on using the metrics, refrain from configuring this port to disable metric collection.
{% endhint %}

### API Limits

The EN offers configuration variables enabling you to tailor RPC server limits, such as capping returned entries or setting transaction size limits. The default configurations we provide have been optimized for general use, but you might need to fine-tune them based on specific requirements.

### JSON-RPC API Namespaces

The EN currently supports seven API namespaces:

* Standard: `eth`, `net`, `web3`, `debug`
* zkSync-specific: `zks`
* Subscription-related: `pubsub` (a.k.a. `eth_subscribe`)
* Sync-related for external nodes: `en`

Use `EN_API_NAMESPACES` and list the namespace names, comma-separated, to activate or deactivate specific namespaces. By default, all namespaces (except `debug`) are active.

### Logging & Observability

* `MISC_LOG_FORMAT`: Dictates the display format of logs. Options include `plain` (human-readable) and `json` (ideal for deployments).
* `RUST_LOG`: This variable facilitates granular control over logging. More details on its formatting can be found here.
* `MISC_SENTRY_URL` and `MISC_OTLP_URL`: Used for integrating Sentry and OpenTelemetry exporters respectively. If you choose to set up Sentry, don't forget to also configure the `EN_SENTRY_ENVIRONMENT` variable, which labels the environment in events reported to Sentry.
