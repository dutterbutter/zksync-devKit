# PubSub JSON-RPC API

Clients can subscribe to specific events and receive notifications, thus avoiding the need to poll. zkSync is fully compatible with [Geth's pubsub API](https://geth.ethereum.org/docs/interacting-with-geth/rpc/pubsub), except for the `syncing` subscription.

The WebSocket URL is `wss://testnet.era.zksync.dev/ws`.

### Methods

### `eth_subscribe`

Creates a new subscription for events.

**Parameters**

| Parameter          | Type   | Description                | Required |
| ------------------ | ------ | -------------------------- | -------- |
| `subscriptionName` | String | Name of the subscription   | Yes      |
| `options`          | Object | Optional filter conditions | No       |

**Example Request**

```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "method": "eth_subscribe",
  "params": ["newHeads"]
}
```

**Example Response**

```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": "0x9cef478923ff08bf67fde6c64013158d"
}
```

### `eth_unsubscribe`

Cancels an existing subscription.

**Parameters**

| Parameter        | Type   | Description            | Required |
| ---------------- | ------ | ---------------------- | -------- |
| `subscriptionId` | String | ID of the subscription | Yes      |

**Example Request**

```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "method": "eth_unsubscribe",
  "params": ["0x9cef478923ff08bf67fde6c64013158d"]
}
```

**Example Response**

```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": true
}
```

### Supported Subscriptions

### `newHeads`

Emits a new header each time a new block is appended to the chain.

**Parameters: None**

### `logs`

Returns logs matching specific filter criteria.

**Parameters**

| Parameter | Type      | Description      | Required |
| --------- | --------- | ---------------- | -------- |
| `address` | String    | Contract address | No       |
| `topics`  | String\[] | Array of topics  | No       |

### `newPendingTransactions`

Returns the hash for all transactions that are added to the pending state.

**Parameters: None**
