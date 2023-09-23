# Running Node

Before diving into the setup and operational details, ensure you have the necessary dependencies installed. You can find the detailed prerequisites in the [Installing dependencies](https://chat.openai.com/c/setup-dev.md) guide.

### Setup

#### Initial Setup

1.  Install and initialize the environment:

    ```bash
    bashCopy codezk 
    zk init
    ```

    During the first initialization, you'll need to download approximately 8 GB of setup files. This is a one-time step.
2. If facing issues during initialization, refer to the `zk run plonk-setup` command help or the [Troubleshooting](https://chat.openai.com/c/bbd6cd82-dec2-4f11-9e2d-3a2fd63b2f0b#Troubleshooting) section.

#### Starting and Stopping Services

*   **To Start Services**:

    ```bash
    bashCopy codezk up
    ```
*   **To Stop Services**:

    ```bash
    bashCopy codezk down
    ```

If you ever need to reset the dev environment, stop the services and repeat the initial setup.

### Running Servers and Services

#### Database and Contracts

*   Deploy or redeploy:

    ```bash
    bashCopy codezk contract redeploy
    ```

#### Environment Configurations

Config files are located in `etc/env/`.

| Command             | Description                    |
| ------------------- | ------------------------------ |
| `zk env`            | List configurations.           |
| `zk env <ENV_NAME>` | Switch between configurations. |

The default configuration is `dev.env`, which is auto-generated from `dev.env.example` during the `zk init` command.

#### Running Servers

Refer to the table below for a quick understanding of each server and how to run them:

| Command                                                                                           | Description                                                                               |
| ------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| `zk server`                                                                                       | Regular server setup.                                                                     |
| `zk server` (After setting up `service_account.json` for GCP)                                     | Server using Google Cloud Storage instead of the default store.                           |
| `zk f cargo +nightly run --release --bin zksync_prover`                                           | Prover server on machine without GPU.                                                     |
| `zk f cargo +nightly run --features gpu --release --bin zksync_prover`                            | Prover server on machine with GPU.                                                        |
| `cargo run --release --bin zksync_verification_key_generator`                                     | Running the verification key generator.                                                   |
| \[Additional commands for setup key generator with/without GPU]                                   | Check documentation for details on running the setup key generator on different machines. |
| `cargo run --release --bin zksync_json_to_binary_vk_converter -- -o /path/to/output-binary-vk`    | For binary verification keys from existing JSON keys.                                     |
| `cargo run --release --bin zksync_commitment_generator`                                           | Generate commitment for existing verification keys.                                       |
| `cargo run --release --bin zksync_contract_verifier -- --jobs-number X` or `zk contract_verifier` | Run the contract verifier.                                                                |

### Troubleshooting

Facing issues during setup or operation? Check out the common problems and their solutions below:

1. **SSL error: certificate verify failed**: Ensure that the version of `axel` on your computer is `2.17.10` or higher.
2. **rmSync is not a function**: Ensure the version of `node.js` installed on your computer is `14.14.0` or higher.
3. **Invalid bytecode: ()**: \[Sample error output]

For more specific or uncommon issues, please consult the advanced troubleshooting guide or contact the support team.
