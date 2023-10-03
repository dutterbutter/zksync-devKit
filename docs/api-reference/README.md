# 🌐 API REFERENCE

Welcome to the zkSync Era API reference documentation! This page provides you with a high-level overview of our API capabilities and essential information.

### API Purpose

zkSync Era is tailored to seamlessly integrate with the Ethereum ecosystem. To achieve this, we not only support the standard Ethereum JSON-RPC API but have also L2 specific features to enhance its functionality.

### RPC Endpoint URLs

When interacting with the zkSync Era, there are different endpoints for Mainnet and Testnet. Be sure to choose the right one depending on your purpose:

#### Mainnet Network Info:

* **Network Name**: zkSync Era Mainnet
* **RPC URL**: [https://mainnet.era.zksync.io](https://mainnet.era.zksync.io/)
* **WebSocket URL**: wss://mainnet.era.zksync.io/ws
* **Chain ID**: 324
* **Currency Symbol**: ETH
* **Block Explorer URL**: [https://explorer.zksync.io/](https://explorer.zksync.io/)

#### Testnet Network Info:

* **Network Name**: zkSync Era Testnet
* **RPC URL**: [https://testnet.era.zksync.dev](https://testnet.era.zksync.dev/)
* **WebSocket URL**: wss://testnet.era.zksync.dev/ws
* **Chain ID**: 280
* **Currency Symbol**: ETH
* **Block Explorer URL**: [https://goerli.explorer.zksync.io/](https://goerli.explorer.zksync.io/)

#### Rate Limiting

For a smooth experience, there are rate limits to both HTTPS and WebSocket APIs. Typically, these limits are generous, currently 10s to 100s RPS per client.
