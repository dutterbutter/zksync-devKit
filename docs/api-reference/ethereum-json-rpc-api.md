# Ethereum JSON-RPC API

zkSync Era supports the standard [Ethereum JSON-RPC API](https://ethereum.org/en/developers/docs/apis/json-rpc/).

### `eth_protocolVersion`

Returns the current Ethereum protocol version.

**Request:**

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_protocolVersion","params":[],"id":1}' -H "Content-Type: application/json" https://mainnet.era.zksync.io
```

**Response:**

```json
{"jsonrpc":"2.0","id":1,"result":"0x3f"}
```

### `eth_syncing`

**Description:**\
Returns the sync status. This will always return false in zkSync.

**Request:**

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_syncing","params":[],"id":1}' -H "Content-Type: application/json" https://mainnet.era.zksync.io
```

**Response:**

```json
{"jsonrpc":"2.0","id":1,"result":false}
```

### `eth_gasPrice`

Returns the current gas price in the default EVM denomination parameter.

**Request:**

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_gasPrice","params":[],"id":1}' -H "Content-Type: application/json" https://mainnet.era.zksync.io
```

**Response:**

```json
{"jsonrpc":"2.0","id":1,"result":"0x0"}
```

### `eth_getBalance`

Returns the account balance for a given account address and Block Number.

**Parameters:**

| Name            | Type                | Description          |
| --------------- | ------------------- | -------------------- |
| Account Address | String (Hex)        | The address          |
| Block Number    | String (Hex) / Hash | Block number or hash |

**Request:**

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBalance","params":["0x0f54f47bf9b8e317b214ccd6a7c3e38b893cd7f0", "0x0"],"id":1}' -H "Content-Type: application/json" https://mainnet.era.zksync.io
```

**Response:**

```json
{"jsonrpc":"2.0","id":1,"result":"0x36354d5575577c8000"}
```

### `eth_getTransactionCount`

Returns the number of transactions sent from an address.

**Parameters:**

| Name            | Type         | Description                                 |
| --------------- | ------------ | ------------------------------------------- |
| Account Address | String (Hex) | The Ethereum address                        |
| Block Tag       | String (Hex) | Block tag ('latest', 'earliest', 'pending') |

**Request:**

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getTransactionCount","params":["0x0f54f47bf9b8e317b214ccd6a7c3e38b893cd7f0", "latest"],"id":1}' -H "Content-Type: application/json" https://mainnet.era.zksync.io
```

**Response:**

```json
{"jsonrpc":"2.0","id":1,"result":"0x5"}
```

### `eth_getBlockByHash`

Returns information about a block by hash.

**Parameters:**

| Name       | Type         | Description                               |
| ---------- | ------------ | ----------------------------------------- |
| Block Hash | String (Hex) | The block hash                            |
| Full Tx    | Boolean      | If true, returns full transaction objects |

**Request:**

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBlockByHash","params":["0xb906c6d72ec26a31a4fc27bc147db3ce58e7ea420e0a8b8c19f24a07c6d5c109", false],"id":1}' -H "Content-Type: application/json" https://mainnet.era.zksync.io
```

**Response:**

```json
{"jsonrpc":"2.0","id":1,"result": {...}}
```

### `eth_getBlockTransactionCountByNumber`

Returns the total transaction count for a given block number.

**Parameters**:

| Name         | Type       | Description                      |
| ------------ | ---------- | -------------------------------- |
| Block number | Hex String | The block number as a hex string |

**Request**:

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBlockTransactionCountByNumber","params":["0x1"],"id":1}' -H "Content-Type: application/json" https://mainnet.era.zksync.io
```

**Response:**

```json
{"jsonrpc":"2.0","id":1,"result":{"difficulty":null,"extraData":"0x0","gasLimit":"0xffffffff","gasUsed":"0x0","hash":"0x8101cc04aea3341a6d4b3ced715e3f38de1e72867d6c0db5f5247d1a42fbb085","logsBloom":"0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000","miner":"0x0000000000000000000000000000000000000000","nonce":null,"number":"0x17d","parentHash":"0x70445488069d2584fea7d18c829e179322e2b2185b25430850deced481ca2e77","sha3Uncles":null,"size":"0x1df","stateRoot":"0x269bb17fe7adb8dd5f15f57b717979f82078d6b7a675c1ba1b0da2d27e415fcc","timestamp":"0x5f5ba97c","totalDifficulty":null,"transactions":[],"transactionsRoot":"0x","uncles":[]}}
```

### `eth_getBlockTransactionCountByHash`

Returns the total transaction count for a given block hash.

**Parameters**:

| Name       | Type       | Description                    |
| ---------- | ---------- | ------------------------------ |
| Block Hash | Hex String | The block hash as a hex string |

**Request**:

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBlockTransactionCountByHash","params":["0x8101cc04aea3341a6d4b3ced715e3f38de1e72867d6c0db5f5247d1a42fbb085"],"id":1}' -H "Content-Type: application/json" https://mainnet.era.zksync.io
```

**Response:**

```json
{"jsonrpc":"2.0","id":1,"result":"0x3"}
```

### `eth_getCode`

Returns the code for a given account address and Block Number.

**Parameters**:

| Name                 | Type       | Description                          |
| -------------------- | ---------- | ------------------------------------ |
| Account Address      | Hex String | The account address                  |
| Block Number or Hash | Hex String | Block number or hash as per EIP-1898 |

**Request**:

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getCode","params":["0x7bf7b17da59880d9bcca24915679668db75f9397", "0x0"],"id":1}' -H "Content-Type: application/json" https://mainnet.era.zksync.io
```

**Response:**

```json
{"jsonrpc":"2.0","id":1,"result":"0xef616c92f3cfc9e92dc270d6acff9cea213cecc7020a76ee4395af09bdceb4837a1ebdb5735e11e7d3adb6104e0c3ac55180b4ddf5e54d022cc5e8837f6a4f971b"}
```

### `eth_sign`

Calculates a specific signature.

**Parameters**:

| Name            | Type       | Description              |
| --------------- | ---------- | ------------------------ |
| Account Address | Hex String | The address to sign with |
| Message         | Hex String | The message to sign      |

**Request**:

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_sign","params":["0x3b7252d007059ffc82d16d022da3cbf9992d2f70", "0xdeadbeaf"],"id":1}' -H "Content-Type: application/json" https://mainnet.era.zksync.io
```

**Response:**

```json
{"jsonrpc":"2.0","id":1,"result":"0x909809c76ed2a5d38733de39207d0f411222b9b49c64a192bf649cb13f63f37b45acb4f6939facb4f1c277bc70fb00407564140c0f18600ac44388f2c1dfd1dc1b"}
```

### `eth_sendTransaction`

Sends transaction from a given account to another.

**Parameters**:

| Name     | Type       | Description                             |
| -------- | ---------- | --------------------------------------- |
| from     | Hex String | Sender address                          |
| to       | Hex String | Receiver address                        |
| gas      | Hex String | Gas provided (optional, default: 90000) |
| gasPrice | Hex String | Gas price (optional)                    |
| value    | Hex String | Transaction value                       |
| data     | Hex String | Contract code or method hash            |
| nonce    | Hex String | Transaction nonce (optional)            |

**Request**:

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_sendTransaction","params":[{"from":"0x3b7252d007059ffc82d16d022da3cbf9992d2f70", "to":"0x0f54f47bf9b8e317b214ccd6a7c3e38b893cd7f0", "value":"0x16345785d8a0000", "gasLimit":"0x5208", "gasPrice":"0x55ae82600"}],"id":1}' -H "Content-Type: application/json" https://mainnet.era.zksync.io
```

**Response:**

```json
{"jsonrpc":"2.0","id":1,"result":"0x33653249db68ebe5c7ae36d93c9b2abc10745c80a72f591e296f598e2d4709f6"}
```

### `eth_sendRawTransaction`

**Description**:\
Creates new message call transaction or contract for signed transactions.

**Parameters**:

| Name                    | Type       | Description                 |
| ----------------------- | ---------- | --------------------------- |
| Signed Transaction Data | Hex String | The signed transaction data |

**Request**:

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_sendRawTransaction","params":["0xf9ff74c86aefeb5f6019d77280bbb44fb695b4d45cfe97e6eed7acd62905f4a85034d5c68ed25a2e7a8eeb9baf1b8401e4f865d92ec48c1763bf649e354d900b1c"],"id":1}' -H "Content-Type: application/json" https://mainnet.era.zksync.io
```

**Response:**

```json
{"jsonrpc":"2.0","id":1,"result":"0x0000000000000000000000000000000000000000000000000000000000000000"}
```

### `eth_call`

Executes a new message call immediately without creating a transaction on the block chain.

**Parameters:**

| Name      | Type   | Description                                 |
| --------- | ------ | ------------------------------------------- |
| Tx Object | Object | The transaction object                      |
| Block Tag | String | Block tag ('latest', 'earliest', 'pending') |

**Request:**

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_call","params":[{"to":"0xc94770007dda54cF92009BFF0dE90c06F603a09f", "data":"0x012345..."}, "latest"],"id":1}' -H "Content-Type: application/json" https://mainnet.era.zksync.io
```

**Response:**

```json
{"jsonrpc":"2.0","id":1,"result":"0x"}
```

### `eth_estimateGas`

Provides an estimate of how much gas is necessary for a given transaction to succeed.

**Parameters:**

| Name      | Type   | Description            |
| --------- | ------ | ---------------------- |
| Tx Object | Object | The transaction object |

**Request:**

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_estimateGas","params":[{"to":"0xc94770007dda54cF92009BFF0dE90c06F603a09f", "data":"0x012345..."}],"id":1}' -H "Content-Type: application/json" https://mainnet.era.zksync.io
```

**Response:**

```json
{"jsonrpc":"2.0","id":1,"result":"0x5208"}
```

### `eth_getBlockByNumber`

Returns information about a block by its block number.

#### Parameters

<table><thead><tr><th width="172">Parameter</th><th width="144.33333333333331">Type</th><th>Description</th></tr></thead><tbody><tr><td>Block Number</td><td>String</td><td>The hexadecimal block number</td></tr><tr><td>Full Tx</td><td>Boolean</td><td>If <code>true</code>, returns full transaction objects; if <code>false</code>, only returns transaction hashes</td></tr></tbody></table>

#### Request

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBlockByNumber","params":["0x1", false],"id":1}' -H "Content-Type: application/json" https://mainnet.era.zksync.io
```

#### Response Example

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "difficulty": null,
    "extraData": "0x0",
    "gasLimit": "0xffffffff",
    "gasUsed": null,
    "hash": "0xabac6416f737a0eb54f47495b60246d405d138a6a64946458cf6cbeae0d48465",
    "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
    "miner": "0x0000000000000000000000000000000000000000",
    "nonce": null,
    "number": "0x1",
    "parentHash": "0x",
    "sha3Uncles": null,
    "size": "0x9b",
    "stateRoot": "0x",
    "timestamp": "0x5f5bd3e5",
    "totalDifficulty": null,
    "transactions": [],
    "transactionsRoot": "0x",
    "uncles": []
  }
}
```

### `eth_getBlockByHash`

Returns information about a block by its block hash.

#### Parameters

| Parameter  | Type    | Description                                                                              |
| ---------- | ------- | ---------------------------------------------------------------------------------------- |
| Block Hash | String  | The hexadecimal block hash                                                               |
| Full Tx    | Boolean | If `true`, returns full transaction objects; if `false`, only returns transaction hashes |

#### Request

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBlockByHash","params":["0x1b9911f57c13e5160d567ea6cf5b545413f96b95e43ec6e02787043351fb2cc4", false],"id":1}' -H "Content-Type: application/json" https://mainnet.era.zksync.io
```

#### Response Example

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "difficulty": null,
    "extraData": "0x0",
    "gasLimit": "0xffffffff",
    "gasUsed": null,
    "hash": "0x1b9911f57c13e5160d567ea6cf5b545413f96b95e43ec6e02787043351fb2cc4",
    "logsBloom": "0x00000000100000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000040000000000000000000000000200000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000000000000000000000004000000000000002000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000000000000000000",
    "miner": "0x0000000000000000000000000000000000000000",
    "nonce": null,
    "number": "0xc",
    "parentHash": "0x404e58f31a9ede1b614b98701d6b0fbf1450f186842dbcf6426dd16811a5ca0d",
    "sha3Uncles": null,
    "size": "0x307",
    "stateRoot": "0x599ccdb111fc62c6398dc39be957df8e97bf8ab72ce6c06ff10641a92b754627",
    "timestamp": "0x5f5fdbbd",
    "totalDifficulty": null,
    "transactions": ["0xae64961cb206a9773a6e5efeb337773a6fd0a2085ce480a174135a029afea615"],
    "transactionsRoot": "0x4764dba431128836fa919b83d314ba9cc000e75f38e1c31a60484409acea777b",
    "uncles": []
  }
}
```

### `eth_getTransactionByHash`

Returns transaction details given the Ethereum transaction hash.

**Parameters**

| Parameter | Description           | Type          |
| --------- | --------------------- | ------------- |
| `tx_hash` | Hash of a transaction | `0x`-prefixed |

**Request**

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getTransactionByHash","params":["0xec5fa15e1368d6ac314f9f64118c5794f076f63c02e66f97ea5fe1de761a8973"],"id":1}' -H "Content-Type: application/json" https://mainnet.era.zksync.io
```

**Result**

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "blockHash": "0x7a7398cc11d9c4c8e6f53e0c73824297aceafdab62db9e4b867a0da694384864",
    "blockNumber": "0x188",
    "from": "0x3b7252d007059ffc82d16d022da3cbf9992d2f70",
    "gas": "0x147ee",
    "gasPrice": "0x3b9aca00",
    "hash": "0xec5fa15e1368d6ac314f9f64118c5794f076f63c02e66f97ea5fe1de761a8973",
    "input": "0x6dba746c",
    "nonce": "0x18",
    "to": "0xa655256f589060437e5ffe2246dec385d040f148",
    "transactionIndex": "0x0",
    "value": "0x0",
    "v": "0xa96",
    "r": "0x6db399d694a452fb4106419140a6e5dbbe6817743a0f6f695a651e6576e59a5e",
    "s": "0x25dd6ab1f936d0280d2fed0caeb0ebe5b9a46de6d8cb08ad8fd2c88deb55fc31"
  }
}
```

### `eth_getTransactionByBlockHashAndIndex`

Returns transaction details given the block hash and the transaction index.

**Parameters**

| Parameter         | Description                | Type          |
| ----------------- | -------------------------- | ------------- |
| `block_hash`      | Hash of a block            | `0x`-prefixed |
| `transaction_idx` | Transaction index position | Hexadecimal   |

**Request**

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getTransactionByBlockHashAndIndex","params":["0x1b9911f57c13e5160d567ea6cf5b545413f96b95e43ec6e02787043351fb2cc4", "0x0"],"id":1}' -H "Content-Type: application/json" https://mainnet.era.zksync.io
```

**Result**

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "blockHash": "0x1b9911f57c13e5160d567ea6cf5b545413f96b95e43ec6e02787043351fb2cc4",
    "blockNumber": "0xc",
    "from": "0xddd64b4712f7c8f1ace3c145c950339eddaf221d",
    "gas": "0x4c4b40",
    "gasPrice": "0x3b9aca00",
    "hash": "0xae64961cb206a9773a6e5efeb337773a6fd0a2085ce480a174135a029afea615",
    "input": "0x4f2be91f",
    "nonce": "0x0",
    "to": "0x439c697e0742a0ddb124a376efd62a72a94ac35a",
    "transactionIndex": "0x0",
    "value": "0x0",
    "v": "0xa96",
    "r": "0xced57d973e58b0f634f776d57daf41d3d3387ceb347a3a72ca0746e5ec2b709e",
    "s": "0x384e89e209a5eb147a2bac3a4e399507400ac7b29cd155531f9d6203a89db3f2"
  }
}
```

### `eth_getTransactionReceipt`

Returns the receipt of a transaction by transaction hash.

**Parameters**

| Parameter | Description           | Type          |
| --------- | --------------------- | ------------- |
| `tx_hash` | Hash of a transaction | `0x`-prefixed |

**Request**

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getTransactionReceipt","params":["0xae64961cb206a9773a6e5efeb337773a6fd0a2085ce480a174135a029afea614"],"id":1}' -H "Content-Type: application/json" https://mainnet.era.zksync.io
```

**Result**

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "blockHash": "0x1b9911f57c13e5160d567ea6cf5b545413f96b95e43ec6e02787043351fb2cc4",
    "blockNumber": "0xc",
    "contractAddress": "0x0000000000000000000000000000000000000000",
    "cumulativeGasUsed": null,
    "from": "0xddd64b4712f7c8f1ace3c145c950339eddaf221d",
    "gasUsed": "0x5289",
    "logs": [
      {
        "address": "0x439c697e0742a0ddb124a376efd62a72a94ac35a",
        "topics": [
          "0x64a55044d1f2eddebe1b90e8e2853e8e96931cefadbfa0b2ceb34bee36061941"
        ],
        "data": "0x0000000000000000000000000000000000000000000000000000000000000002",
        "blockNumber": "0xc",
        "transactionHash": "0xae64961cb206a9773a6e5efeb337773a6fd0a2085ce480a174135a029afea615",
        "transactionIndex": "0x0",
        "blockHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "logIndex": "0x0",
        "removed": false
      },
      {
        "address": "0x439c697e0742a0ddb124a376efd62a72a94ac35a",
        "topics": [
          "0x938d2ee5be9cfb0f7270ee2eff90507e94b37625d9d2b3a61c97d30a4560b829"
        ],
        "data": "0x0000000000000000000000000000000000000000000000000000000000000002",
        "blockNumber": "0xc",
        "transactionHash": "0xae64961cb206a9773a6e5efeb337773a6fd0a2085ce480a174135a029afea615",
        "transactionIndex": "0x0",
        "blockHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "logIndex": "0x1",
        "removed": false
      }
    ],
    "logsBloom": "0x00000000100000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000040000000000000000000000000200000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000000000000000000000004000000000000002000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000000000000000000",
    "status": "0x1",
    "to": "0x439c697e0742a0ddb124a376efd62a72a94ac35a",
    "transactionHash": "0xae64961cb206a9773a6e5efeb337773a6fd0a2085ce480a174135a029afea615",
    "transactionIndex": "0x0"
  }
}
```

### `eth_newFilter`

Creates a new filter based on specified topics.

**Parameters**

| Parameter | Type   | Description           |
| --------- | ------ | --------------------- |
| `hash`    | String | Hash of a transaction |

**Request**

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_newFilter","params":[{"topics":["0x0000000000000000000000000000000000000000000000000000000012341234"]}],"id":1}' -H "Content-Type: application/json" https://mainnet.era.zksync.io
```

**Result**

```json
{"jsonrpc":"2.0","id":1,"result":"0xdc714a4a2e3c39dc0b0b84d66a3ccb00"}
```

### `eth_newBlockFilter`

Creates a filter to notify when a new block arrives.

**Parameters**

None

**Request**

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_newBlockFilter","params":[],"id":1}' -H "Content-Type: application/json" https://mainnet.era.zksync.io
```

**Result**

```json
{"jsonrpc":"2.0","id":1,"result":"0x3503de5f0c766c68f78a03a3b05036a5"}
```

### `eth_newPendingTransactionFilter`

Creates a filter to notify when new pending transactions arrive.

**Parameters**

None

**Request**

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_newPendingTransactionFilter","params":[],"id":1}' -H "Content-Type: application/json" https://mainnet.era.zksync.io
```

**Result**

```json
{"jsonrpc":"2.0","id":1,"result":"0x9daacfb5893d946997d3801ea18e9902"}
```

### `eth_uninstallFilter`

Removes a filter based on the filter ID.

**Parameters**

| Parameter   | Type   | Description   |
| ----------- | ------ | ------------- |
| `filter_id` | String | The filter ID |

**Request**

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_uninstallFilter","params":["0xb91b6608b61bf56288a661a1bd5eb34a"],"id":1}' -H "Content-Type: application/json" https://mainnet.era.zksync.io
```

**Result**

Returns `true` if successfully uninstalled, otherwise `false`.

```json
{"jsonrpc":"2.0","id":1,"result":true}
```

### `eth_getFilterChanges`

Polls for logs based on filter ID.

**Parameters**

| Parameter   | Type   | Description   |
| ----------- | ------ | ------------- |
| `filter_id` | String | The filter ID |

**Request**

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getFilterChanges","params":["0x127e9eca4f7751fb4e5cb5291ad8b455"],"id":1}' -H "Content-Type: application/json" https://mainnet.era.zksync.io
```

**Result**

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": [
    "0xc6f08d183a81e149896fc5317c872f9092068e88e956ca1864e9bd4c81c09b44",
    "0x3ca6dfb5be15549d721d1b3d10c1bec50ed6217c9ac7b61df361fac9692a27e5",
    "0x776fffac134171acb1ebf2e59856625501ad5ccc5c4c8fe0359e0d4dff8919f2",
    "0x84123103704dbd738c089276ab2b04b5936330b24f6e78453c4ba8bf4848aaf9",
    "0x698905f06aff921a9e9fcef39b8b0d107747c3e6204d2ea79cf4c12debf8d253",
    "0x886b6ae365fec996be8a9a2c31cf4cda97ff8352908be2c83f17abd66ef1591e",
    "0xfd90df68706ef95a62b317de93d6899a9bd6c80416e42d007f5c30fcdedfce24",
    "0x7af8491fbb0373886d9032bb74e0ef52ed9e100f260b79bd15f46126b38cbede",
    "0x91d1e2cd55533cf7dd5de86c9aa73295e811b1279be193d429bbd6ba83810e16",
    "0x6b65b3128c2104005a04923288fe2aa33a2477a4962bef70532f94cab582f2a7"
  ]
}
```

### `eth_getFilterLogs`

Fetches all logs matching a given filter ID.

**Parameters**

| Parameter   | Type     | Description   |
| ----------- | -------- | ------------- |
| `filter_id` | Quantity | The filter ID |

**Request**

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getFilterLogs","params":["0x127e9eca4f7751fb4e5cb5291ad8b455"],"id":1}' -H "Content-Type: application/json" https://mainnet.era.zksync.io
```

**Result**

Array of all logs matching filter.

### `eth_getLogs`

Fetches logs based on filter parameters.

**Parameters**

| Parameter   | Type          | Description                     |
| ----------- | ------------- | ------------------------------- |
| `fromBlock` | Quantity      | Tag                             |
| `toBlock`   | Quantity      | Tag                             |
| `address`   | DATA          | Array                           |
| `topics`    | Array of DATA | Topics for filtering. Optional. |
| `blockhash` | DATA          | Block hash. Optional.           |

**Request**

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getLogs","params":[{"topics":["0x775a94827b8fd9b519d36cd827093c664f93347070a554f65e4a6f56cd738898","0x0000000000000000000000000000000000000000000000000000000000000011"], "fromBlock":"latest"}],"id":1}' -H "Content-Type: application/json" https://mainnet.era.zksync.io
```

**Result**

Array of all logs matching the filter.

```json
{"jsonrpc":"2.0","id":1,"result":[]}
```

### **`eth_getProof`**

Returns the account and storage values of a specified account or smart contract. It also includes the Merkle proof for the returned data.

**Parameters**

| Parameter         | Type                          | Description                                           |
| ----------------- | ----------------------------- | ----------------------------------------------------- |
| account\_address  | String                        | Address of the account or contract.                   |
| storage\_position | Array of Strings              | Integer positions in storage to include in the proof. |
| block\_identifier | String (Block Number or Hash) | Specifies the block to fetch the data from.           |

**Example Request**

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getProof","params":["0x1234567890123456789012345678901234567890",["0x0000000000000000000000000000000000000000000000000000000000000000","0x0000000000000000000000000000000000000000000000000000000000000001"],"latest"],"id":1}' -H "Content-Type: application/json" https://mainnet.era.zksync.io
```

**Example Result**

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "address": "0x1234567890123456789012345678901234567890",
    "accountProof": [...],
    "balance": "0x0",
    "codeHash": "0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470",
    "nonce": "0x0",
    "storageHash": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
    "storageProof": [...]
  }
}
```
