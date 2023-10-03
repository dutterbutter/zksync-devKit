---
description: Use LayerZero to send a bytes payload from zkSync to Optimism.
---

# LayerZero

### Introduction

This guide aims to provide an insight into the LayerZero messaging service, which is an omnichain interoperability protocol designed for lightweight message passing across chains, and how to set up and utilize the OmniCounter contract to demonstrate cross-chain message passing. Below is a table showcasing the zkSync Era and zkSync (Testnet) endpoint details for LayerZero:

<table><thead><tr><th width="176">Network</th><th width="91.33333333333331">ChainID</th><th>Endpoint</th></tr></thead><tbody><tr><td>zkSync Era</td><td>165</td><td>0x9b896c0e23220469C7AE69cb4BbAE391eAa4C8da</td></tr><tr><td>zkSync Testnet</td><td>10165</td><td>0x093D2CF57f764f09C3c2Ac58a42A2601B8C79281</td></tr></tbody></table>

### Prerequisites

* **Knowledge Base**: Familiarity with blockchain concepts and smart contract development.
* **Node.js**: If not installed, [download it here](https://nodejs.org/).

### Step 1 — Understanding LayerZero messaging service

LayerZero enables authentic and guaranteed message delivery between different blockchain networks with configurable trustlessness. Messages in LayerZero are managed by LayerZero Endpoints, which comprise versioned messaging libraries and a proxy to route messages to the appropriate library version. Message states are maintained across versions, allowing for seamless library upgrades.&#x20;

Messages are sent from the [User Application (UA)](broken-reference) at source `srcUA` to the UA at the destination `dstUA`. Once the message is received by `dstUA`, the message is considered delivered (transitioning from `INFLIGHT` to either `SUCCESS` or `STORED`)

More details can be found [here](https://layerzero.gitbook.io/docs/faq/messaging-properties).

### Step 2 — Environment setup

Clone the GitHub repository containing the OmniCounter contract, and navigate to the repository directory:

```bash
git clone git@github.com:LayerZero-Labs/solidity-examples.git
```

Install dependencies:

```bash
yarn install
```

### Step 3 — Understanding OmniCounter

OmniCounter is a smart contract that increments a counter on another chain via LayerZero messaging service. It utilizes `_lzSend()` within `incrementCounter()` to send messages, and `_nonblockingLzReceive()` to receive messages on the destination chain.

### Step 4 — Deploy OmniCounter

Deploy OmniCounter contract on at least two different chains. In this example, we'll use Binance Smart Chain Testnet (bsc-testnet) and Avalanche Fuji (fuji):

```bash
bashCopy codenpx hardhat --network bsc-testnet deploy --tags OmniCounter
npx hardhat --network fuji deploy --tags OmniCounter
```

### Step 5 — Configure and Send Cross-Chain Message

Set the remote addresses to allow each contract to receive messages:

```bash
bashCopy codenpx hardhat --network bsc-testnet setTrustedRemote --target-network fuji --contract OmniCounter
npx hardhat --network fuji setTrustedRemote --target-network bsc-testnet --contract OmniCounter
```

Send a cross-chain message from bsc-testnet to fuji to increment the counter:

```bash
bashCopy codenpx hardhat --network bsc-testnet incrementCounter --target-network fuji
```

To watch the counter increment in real-time, use the following command in a separate terminal:

```bash
bashCopy codenpx hardhat --network fuji ocPoll
```

### Conclusion

You have now set up and demonstrated cross-chain messaging using LayerZero with the OmniCounter contract. This simplistic example serves as a gateway to understand and explore the potential of LayerZero messaging service in building interoperable blockchain applications.

&#x20;
