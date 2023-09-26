---
description: >-
  zkSync CLI simplifies the process of developing applications and interacting
  with zkSync Era
---

# zksync-cli

The zkSync CLI simplifies the process of developing applications and interacting with zkSync Era. Out of the box it provides ready-templates to facilitate fast development and quick commands for convenience. &#x20;

{% hint style="info" %}
**Repository:** [https://github.com/matter-labs/zksync-cli](https://github.com/matter-labs/zksync-cli)\
**Learn to contribute:** [CONTRIBUTING.md](https://github.com/matter-labs/zksync-cli/blob/main/.github/CONTRIBUTING.md)
{% endhint %}

### Installation

Install the zkSync CLI globally with the following command:

{% tabs %}
{% tab title="yarn" %}
```bash
yarn add global zksync-cli@latest
```
{% endtab %}

{% tab title="npm" %}
```bash
npm install -g zksync-cli@latest
```
{% endtab %}

{% tab title="npx" %}
```bash
npx zksync-cli@latest [COMMAND]
```
{% endtab %}
{% endtabs %}

### Commands

| Command                                   | Description                                                                                                   |
| ----------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| `zksync-cli create-project {FOLDER_NAME}` | Creates a project from template in the specified folder                                                       |
| `zksync-cli deposit`                      | Deposits funds from Ethereum (L1) to zkSync (L2)                                                              |
| `zksync-cli withdraw`                     | Withdraws funds from zkSync (L2) to Ethereum (L1)                                                             |
| `zksync-cli withdraw-finalize`            | Finalizes withdrawal of funds from zkSync (L2) to Ethereum (L1)                                               |
| `zksync-cli help`                         | Provides information about all supported commands                                                             |
| `zksync-cli help {command}`               | Provides detailed information about how to use a specific command (Replace `{command}` with the command name) |
| `zksync-cli --version`                    | Returns the current version                                                                                   |

{% hint style="info" %}
Withdraws on zkSync Era mainnet have a **24h delay** during Alpha.
{% endhint %}

#### Project Templates

Use the `zksync-cli create-project` command to initiate a project from the following templates:

* [**Hardhat + Solidity**](https://github.com/matter-labs/zksync-hardhat-template)
* [**Hardhat + Vyper**](https://github.com/matter-labs/zksync-hardhat-vyper-template)

#### Supported Chains

The zkSync CLI defaults to Era Testnet and Era Mainnet. You can target different networks by specifying L1 and L2 RPC URLs:

```bash
zksync-cli deposit --l2-rpc=http://your_l2_url --l1-rpc=http://your_l1_url
```

For a local setup using a dockerized testing node and default RPC URLs:

1. Use the **Local Dockerized node** option in the CLI.
2. Alternatively, use:

```bash
zksync-cli [command] --chain local-dockerized
```
