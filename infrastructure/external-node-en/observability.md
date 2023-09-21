# Observability

While the configuration for logs and Sentry is already tackled in the previous sections, this guide zooms in on the metrics offered by the External Node (EN). For better comprehension of this material, being acquainted with [Prometheus](https://prometheus.io/docs/introduction/overview/) and [Grafana](https://grafana.com/docs/) is beneficial.

### Default Latency Histogram Buckets

The default latency histograms are categorized in these predefined time buckets (expressed in seconds):

```
[0.001, 0.005, 0.025, 0.1, 0.25, 1.0, 5.0, 30.0, 120.0]
```

### Key Metrics Overview

The EN lays out an array of metrics; however, not all of them might be vital outside of the development pipeline. The focus here is to spotlight metrics potentially useful for external setups.

{% hint style="warning" %}
**Important**: If you opt not to scrape Prometheus metrics, ensure you clear the `EN_PROMETHEUS_PORT` environment variable to avert memory leaks.
{% endhint %}

| Metric Name                                    | Type      | Labels                              | Description                                                                         |
| ---------------------------------------------- | --------- | ----------------------------------- | ----------------------------------------------------------------------------------- |
| `external_node_synced`                         | Gauge     | -                                   | Reflects synchronization: 1 for synced, 0 otherwise (parallels eth\_call behavior). |
| `external_node_sync_lag`                       | Gauge     | -                                   | Indicates the lag in blocks compared to the main node.                              |
| `external_node_fetcher_requests`               | Histogram | `stage, actor`                      | Duration of requests by different fetcher components.                               |
| `external_node_fetcher_cache_requests`         | Histogram | -                                   | Time span of fetcher cache layer requests.                                          |
| `external_node_fetcher_miniblock`              | Gauge     | `status`                            | Number of the recent L2 block update fetched from the main node.                    |
| `external_node_fetcher_l1_batch`               | Gauge     | `status`                            | Number of the latest batch update fetched from the main node.                       |
| `external_node_action_queue_action_queue_size` | Gauge     | -                                   | Volume of fetched items in queue awaiting processing.                               |
| `server_miniblock_number`                      | Gauge     | `stage=sealed`                      | The most recent locally processed L2 block number.                                  |
| `server_block_number`                          | Gauge     | `stage=sealed`                      | The most recent locally processed L1 batch number.                                  |
| `server_block_number`                          | Gauge     | `stage=tree_lightweight_mode`       | Last L1 batch number assessed by the Merkle tree.                                   |
| `server_processed_txs`                         | Counter   | `stage=mempool_added, state_keeper` | Useful for visualizing incoming and transaction processing TPS rates.               |
| `api_web3_call`                                | Histogram | `method`                            | Duration of Web3 API requests.                                                      |
| `sql_connection_acquire`                       | Histogram | -                                   | Time required to fetch an SQL connection from the pool.                             |

### Data Interpretation

After a database dump is applied, the EN has to reconstruct the Merkle tree to confirm the PostgreSQL state's accuracy. In this phase, `server_block_number { stage='tree_lightweight_mode' }` elevates from 0 to `server_block_number { stage='sealed' }`. The latter remains constant since the EN requires an up-to-date tree for further progression.

Subsequently, the EN needs synchronization with the primary node. This is evident as `server_block_number { stage='sealed' }` amplifies and `external_node_sync_lag` diminishes.

On successful synchronization, the status is conveyed by the `external_node_synced` metric.
