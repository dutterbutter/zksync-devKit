---
description: >-
  Experiencing issues with your External Node (EN)? Dive into this
  troubleshooting guide to understand and address the common challenges
  associated with the EN.
---

# Troubleshooting

### EN Behavior

The EN aims to strictly adhere to the "fail-fast" principle, meaning that when it identifies an anomaly, it typically restarts rather than attempting to recover the state. Occasionally, this might seem like the EN is crashing. A singular incident shouldn't be cause for alarm, but if the node is caught in a continuous crash loop or acts erratically, it hints at either a bug or a configuration issue.

### Common Issues and Solutions

#### **1. Panics**

In Rust, "Panics" denote irrecoverable errors, often leading to an abrupt application crash. Here's how to decipher some common panics:

* **Database Communication Issues**:
  * Panic Message: `Result::unwrap() on an Err value: Database(PgDatabaseError: problem...`
  * Solution: This implies communication trouble with PostgreSQL. Ensure active database connections.
* **Storage Space Issues**:
  * Panic Message: `failed to init rocksdb: Error { message: "IO error: No space left on device...`
  * Solution: Your SSD storage is running out of space. Ensure you have sufficient storage available.
* **Poison Errors**:
  * Panic Message: Contains "Poison Error".
  * Solution: This is a subsequent panic following an initial one. Inspect the logs to identify the primary panic that preceded this one.

If you encounter panics not mentioned above, even if the state recovers post-restart, it's crucial to report these to Matter Labs.

#### **2. Genesis Issues**

If you spot genesis-related errors, it's an indication that the EN started without an applied database dump. Always ensure a dump is applied before initializing the EN.

#### **3. Logs**

Logs play an instrumental role in diagnosing EN issues. Key logs are automatically sent to Sentry (if configured). However, if you identify unnecessary alerts, you can customize the log settings.

| Level | Log Substring                                         | Interpretation                                                                                                  |
| ----- | ----------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| ERROR | "One of the tokio actors unexpectedly finished"       | A component crashed, causing the node to restart.                                                               |
| WARN  | "Stop signal received, is shutting down"              | A supplementary log related to the aforementioned error.                                                        |
| ERROR | "A lot of requests to the remote API failed in a row" | The remote API updating token lists might be down. It should resolve when the API is up again.                  |
| WARN  | Various messages related to main node or API issues   | These signify issues with the main node or API connections. Ensure connectivity and configurations are correct. |

Repeated occurrences of WARN+ level logs can be indicative of a persistent issue.

#### **4. Metrics Anomalies**

After the tree aligns with the DB snapshot, these metrics anomalies can be telling:

* **Fetcher's Slow Block Retrieval**:
  * Symptoms: `external_node_sync_lag` remains unchanged and `external_node_action_queue_action_queue_size` is near 0.
  * Solution: Enhance network connection speed.
* **State Keeper's Slow Data Processing**:
  * Symptoms: `external_node_sync_lag` remains static, and `external_node_action_queue_action_queue_size` is notably high.
  * Solution: Upgrade to a more robust CPU.
* **SQL Connection Pool Exhaustion**:
  * Symptom: `sql_connection_acquire` metric spikes significantly.
  * Solution: Increase the connection pool size to meet the demand.
