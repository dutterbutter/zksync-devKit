---
description: >-
  This documentation provides an overview of the main components of the zkSync
  Era External Node (EN).
---

# Component Breakdown

### API

The EN can serve both the **HTTP** and the **WS Web3 API**, as well as **PubSub**.

**Features**:

* Provides data based on local state.
* Exceptions include submitting transactions (proxied to the main node) and querying transactions (if a local lookup fails, the query is made on the main node).
* Continued service even if the main node is temporarily unavailable.

The API does not depend on the main node. Even if the main node is temporarily unavailable, the EN can continue to serve the state it has locally.

### Fetcher

Responsible for maintaining synchronization between the EN and the main node.

**Key Responsibilities**:

* Fetching new blocks.
* Monitoring L1 batch statuses (committed, proven, or executed on L1).
* Retrieving L1 gas price from the main node.

Ensuring that gas estimations are performed in the same manner as the main node reduces the chances of a transaction not being included in a block.

### State Keeper / VM

Serves as the sequencer component. It shares most functionalities with the main node.

**Features**:

* Retrieves transactions from the queue populated by the Fetcher.
* Executes batches within the VM.

The actual execution of batches happens in the VM, which is identical in both the Main and External nodes.

### Reorg Detector

Tracks L1 batches not yet finalized and handles rollbacks when necessary.

**Features**:

* Compares locally obtained state root hashes with those from the main node's API.
* If there's a mismatch, it identifies the L1 batch responsible, rolls back the local state, and restarts the EN.

In rare situations where L1 batches get reverted before they're final, the Reorg Detector ensures the EN handles it correctly.

### Consistency Checker

Cross-checks each L1 batch against the L1 smart contract for enhanced security.

**Features**:

* Recalculates block commitments for L1 transactions.
* Compares local data with the actual L1 commitment.
* If there's a discrepancy, it indicates potential issues, and the EN enters a crash loop until resolved.

The main node API is the primary source for the EN. But to ensure security, the L1 smart contract is the primary source of truth. Thus, the Consistency Checker plays a vital role.

### Health Check Server

Monitors the health of the EN and provides feedback via HTTP responses.

**Features**:

* Returns HTTP 200 when the EN operates normally.
* Returns HTTP 503 if health checks fail (e.g., EN not fully initialized).

This server can be useful, for example, when implementing a readiness probe in an orchestration solution.
