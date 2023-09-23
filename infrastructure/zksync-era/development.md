---
description: >-
  This guide provides instructions and insights into the development aspects of
  zkSync.
---

# Development

### Project Initialization

#### 1.1. Setting up the Toolkit

To initialize the primary `zk` toolkit:

```bash
zk
```

Enable shell autocompletion with:

```bash
zk completion install
```

#### 1.2. Project Setup

Post dependency installation, initialize the project:

```bash
zk init
```

Running this will:

* Create a `$ZKSYNC_HOME/etc/env/dev.env` file with app settings.
* Set up Docker containers, including a `geth` Ethereum node for local development.
* Download, then unpack cryptographic backend files.
* Generate necessary smart contracts.
* Compile every smart contract.
* Deploy smart contracts to the local Ethereum network.
* Generate a server "genesis block".

**Note:** Initialization might be time-consuming. However, steps like downloading and unpacking keys, or setting up containers, are typically one-time actions.

Usually, after each merge to the `main` branch, it's recommended to execute `zk init` because the application setup could change.

For cleaning up generated data, the subcommand `zk clean` is provided:

```bash
zk clean --all                   # Erase generated configs, database, and backups.
zk clean --config                # Delete only configs.
zk clean --database              # Erase database.
zk clean --backups               # Delete backups.
zk clean --database --backups    # Erase both the database and backups but keep configs.
```

**When is Cleaning Required?**

1. Before running `zk init` on an already initialized database, ensure the database is deleted.
2. If newly merged features from the `main` branch cause issues and `zk init` doesn’t solve them, consider deleting `$ZKSYNC_HOME/etc/env/dev.env` and re-running `zk init`. This approach might resolve issues stemming from altered application configurations.

For tasks like starting or stopping containers without the full `zk init` functionality:

```bash
zk up    # Activate `geth` container
zk down  # Deactivate `geth` container
```

### 2. Reinitialization

While making infrastructural changes (e.g., altering contract code), the entire `init` process is unnecessary. For such situations, use:

```bash
zk reinit
```

It assumes you've already run `zk init` in your current environment. If `zk reinit` fails, consider using `zk init`.

### 3. Code Commit Process

zkSync employs pre-commit and pre-push git hooks for primary code integrity validations. These hooks, set up automatically during workspace initialization, ensure code meets several standards before committing:

* Rust code should always use `cargo fmt` for formatting.
* Other code types should be formatted using `zk fmt`.
* Committing the Dummy Prover (explained below) is prohibited.

### 4. Working with the Dummy Prover

By default, a "dummy" prover is in use. It doesn't compute proofs but uses mock-ups to bypass resource-intensive calculations in a dev environment.

Switch from the dummy to a real prover by setting `dummy_verifier` to `false` in your environment's `contracts.toml` (likely located in `etc/env/dev/contracts.toml`). Then, run `zk init` to redeploy smart contracts.

### 5. Testing Procedures

#### 5.1. Rust Unit-Tests

Execute:

```bash
zk test rust
```

For specific unit-tests:

```bash
zk test rust --package <package_name> --lib <mod>::tests::<test_fn_name> -- --exact
```

For example:

```bash
zk test rust --package zksync_core --lib eth_sender::tests::resend_each_block -- --exact
```

#### 5.2. Integration Testing

Use the following steps:

1. Launch the server in one terminal:

```bash
zk server
```

2. Execute the integration test in another terminal:

```bash
zk test i server
```

#### 5.3. Benchmarks and Load Testing

For benchmarks:

```bash
zk f cargo bench
```

For load tests:

1. Start the server:

```bash
zk server
```

2. If using the real prover, start it:

```bash
zk prover
```

Then, in a different terminal:

```bash
zk run loadtest
```

### 6. Smart Contracts

#### 6.1. Rebuilding Contracts

Execute:

```bash
zk contract build
```

#### 6.2. Publishing to Etherscan

Run:

```bash
zk contract publish
```
