# Debug JSON-RPC API

### `debug_traceBlockByHash`

Returns debug trace of all executed calls contained in a block given by its L2 hash.

**Inputs**

| Parameter | Type           | Description                                                                                                                                         |
| --------- | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| `hash`    | `H256`         | The 32 byte hash defining the L2 block.                                                                                                             |
| `options` | `TracerConfig` | Optional arguments: see the [TraceConfig documentation](https://geth.ethereum.org/docs/interacting-with-geth/rpc/ns-debug#traceconfig) for details. |

**Example**

```bash
curl -X POST  -H "Content-Type: application/json"  \
--data '{"jsonrpc":"2.0", "id":2, "method": "debug_traceBlockByHash", "params": ["0x4bd0bd4547d8f8a4fc86a024e54558e156c1acf43d82e24733c6dac2fe5c5fc7"] }' \
"https://mainnet.era.zksync.io"
```

**Output snippet**

```json
{
    "calls": [],
    "error": null,
    "from": "0x0000000000000000000000000000000000008001",
    "gas": "0xe1e31a08",
    "gasUsed": "0x27e",
    "input": "0x6ef25c3a",
    "output": "0x000000000000000000000000000000000000000000000000000000000ee6b280",
    "revertReason": null,
    "to": "0x000000000000000000000000000000000000800b",
    "type": "Call",
    "value": "0x0"
},
```

### `debug_traceBlockByNumber`

Returns debug trace of all executed calls contained in a block given by its L2 block number.

**Inputs**

| Parameter | Type           | Description                                                                                                                                         |
| --------- | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| `block`   | `BlockNumber`  | Block number.                                                                                                                                       |
| `options` | `TracerConfig` | Optional arguments: see the [TraceConfig documentation](https://geth.ethereum.org/docs/interacting-with-geth/rpc/ns-debug#traceconfig) for details. |

**Example**

```bash
curl -X POST  -H "Content-Type: application/json"  \
--data '{"jsonrpc":"2.0", "id":2, "method": "debug_traceBlockByNumber", "params": [ "0x24b258" ] }' \
"https://mainnet.era.zksync.io"
```

**Output snippet**

```json
{
    "calls": [],
    "error": null,
    "from": "0x0000000000000000000000000000000000008001",
    "gas": "0xe1e31a08",
    "gasUsed": "0x27e",
    "input": "0x6ef25c3a",
    "output": "0x000000000000000000000000000000000000000000000000000000000ee6b280",
    "revertReason": null,
    "to": "0x000000000000000000000000000000000000800b",
    "type": "Call",
    "value": "0x0"
},
```

### `debug_traceCall`

Returns debug trace containing information on a specific calls given by the call request.

**Inputs**

| Parameter | Type           | Description                                                                                                                                         |
| --------- | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| `request` | `CallRequest`  | The transaction call request for debugging.                                                                                                         |
| `block`   | `BlockNumber`  | Block number by hash or number (optional).                                                                                                          |
| `options` | `TracerConfig` | Optional arguments: see the [TraceConfig documentation](https://geth.ethereum.org/docs/interacting-with-geth/rpc/ns-debug#traceconfig) for details. |

**Example**

```bash
curl -X POST  -H "Content-Type: application/json"  \
--data '{"jsonrpc":"2.0", "id":2, "method": "debug_traceCall", "params": [ { "from": "0x1111111111111111111111111111111111111111", "to":"0x2222222222222222222222222222222222222222", "data": "0xffffffff" }, "0x24b258" ] }' \
"https://mainnet.era.zksync.io"
```

**Output snippet**

```json
{
    "calls": [],
    "error": null,
    "from": "0x0000000000000000000000000000000000008001",
    "gas": "0x4b19b87",
    "gasUsed": "0x291",
    "input": "0x4de2e4680000000000000000000000001111111111111111111111111111111111111111",
    "output": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "revertReason": null,
    "to": "0x0000000000000000000000000000000000008002",
    "type": "Call",
    "value": "0x0"
},
```

### `debug_traceTransaction`

Uses the [EVM's `callTracer`](https://geth.ethereum.org/docs/developers/evm-tracing/built-in-tracers#call-tracer) to return a debug trace of a specific transaction given by its transaction hash.

**Inputs**

| Parameter | Type           | Description                                                                                                                                         |
| --------- | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| `hash`    | `H256`         | The 32 byte hash defining the L2 block.                                                                                                             |
| `options` | `TracerConfig` | Optional arguments: see the [TraceConfig documentation](https://geth.ethereum.org/docs/interacting-with-geth/rpc/ns-debug#traceconfig) for details. |

**Example**

```bash
curl -X POST  -H "Content-Type: application/json"  \
--data '{"jsonrpc":"2.0", "id":2, "method": "debug_traceTransaction", "params": [ "0x4b228f90e796de5a18227072745b0f28e0c4a4661a339f70d3bdde591d3b7f3a" ] }' \
"https://mainnet.era.zksync.io"
```

**Output snippet**

```json
{
    "calls": [],
    "error": null,
    "from": "0x0000000000000000000000000000000000008001",
    "gas": "0xdff51aa3",
    "gasUsed": "0x27e",
    "input": "0x6ef25c3a",
    "output": "0x000000000000000000000000000000000000000000000000000000000ee6b280",
    "revertReason": null,
    "to": "0x000000000000000000000000000000000000800b",
    "type": "Call",
    "value": "0x0"
}
```
