---
description: >-
  Dive deep to understand the intricacies, components, and capabilities of the
  External Node.
---

# External Node (EN)

{% hint style="danger" %}
**Disclaimers**

* The external node is in its **alpha** phase and should be used with caution.
* During its alpha phase, the EN depends on a DB snapshot, which is not publicly available.
* The EN is a read-only replica of the main node. We're in the process of decentralizing our infrastructure with the introduction of a consensus node. However, note that the EN is _not_ the consensus node.
{% endhint %}

### What is the External Node?

The external node (often abbreviated as EN) is an external read-replica of the main centralized node. Its role is to fetch data from the zkSync API and re-apply transactions locally, starting from the genesis block. Given that it shares most of its codebase with the main node, transactions are re-applied in the same manner as done by the main node in the past.

In simple terms, the current state of the EN is akin to an Ethereum archive node, granting access to the blockchain's entire history.

#### High-Level Overview

The External Node can be envisioned as an application divided into several modules:

* **API Server**: Offers a publicly available Web3 interface.
* **Synchronization Layer**: Engages with the main node to retrieve blocks and transactions for re-execution.
* **Sequencer Component**: Responsible for executing and storing transactions obtained from the synchronization layer.
* **Checker Modules**: They ensure the consistency of the EN's state.

#### **Capabilities of the External Node (EN)**

| Capabilities of the EN                                            | Can the EN perform this? |
| ----------------------------------------------------------------- | :----------------------: |
| Recreate and verify the zkSync Era mainnet/testnet state locally. |             ✅            |
| Trustlessly interact with the recreated state.                    |             ✅            |
| Use the Web3 API without querying the main node.                  |             ✅            |
| Send L2 transactions (proxied to the main node).                  |             ✅            |
| Independently create L2 blocks or L1 batches.                     |             ❌            |
| Generate proofs.                                                  |             ❌            |
| Submit data to L1.                                                |             ❌            |

For a deeper dive into each component, visit the [components](component-breakdown.md) section.
