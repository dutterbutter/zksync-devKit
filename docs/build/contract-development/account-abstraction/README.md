# Account Abstraction

### Introduction

Account Abstraction (AA) in zkSync is a transformative feature that augments account capabilities in the zkSync Era. Unlike traditional Externally Owned Accounts (EOAs) that can only initiate transactions, accounts in zkSync with AA can also host custom logic akin to smart contracts. This document provides an overview of AA's implementation in zkSync, highlights the key differences from Ethereum's EIP4337, and explores its various use cases.

### Core Concepts

#### Smart Accounts and Paymasters

At the heart of zkSync's AA are Smart Accounts and Paymasters. Smart Accounts are fully programmable, providing native support for custom signature schemes, multi-signatures, spending limits, and application-specific logic. Paymasters enable transactions to be paid by third parties, thereby improving user experience.

#### IAccount Interface

It's recommended for accounts to implement the `IAccount` interface, which consists of five key methods:

* `validateTransaction`: Validates if the transaction should proceed.
* `executeTransaction`: Executes the transaction after fees are charged.
* `payForTransaction`: Optionally pays for the transaction when no Paymaster is involved.
* `prepareForPaymaster`: Prepares the account for interaction with a Paymaster.
* `executeTransactionFromOutside`: Allows transactions to be initiated from external sources.

#### Fees

zkSync employs a unified `gasLimit` field for transaction fees. This single field covers verification, ERC20 transfer for fees, and transaction execution. The `estimateGas` method automatically includes constants to cover these aspects for EOAs.

### Differences from EIP 4337

zkSync's AA and Ethereum's EIP 4337 both aim to improve account flexibility, but they differ in key ways:

* **Implementation Level**: zkSync integrates AA at the protocol level, while EIP 4337 avoids this.
* **Account Types**: In zkSync, all accounts, including EOAs, behave like Smart Accounts and support Paymasters.
* **Transaction Processing**: zkSync uses a unified mempool for both EOAs and Smart Accounts, unlike EIP 4337's dual transaction flows.
* **Paymaster Support**: zkSync supports Paymasters for both EOAs and Smart Accounts, while EIP 4337 supports Paymasters only for Smart Accounts.

### Use Cases

Account Abstraction in zkSync can be leveraged for (non-exhaustive list):

* **Spending Rules**: Define custom rules for spending tokens.
* **Social Recovery**: Implement mechanisms for account recovery.
* **Two-Factor Authentication (2FA)**: Add an additional layer of security.
* **Email/Phone Number Authentication**: Use alternative auth methods.
* **Account Inheritance**: Enable seamless token transfer upon account inactivity.
* **Account Automations**: Perform actions like auto-sell or auto-stake.
* **Session Keys**: Create temporary keys for limited account access.
* **Scheduled Transactions**: Automate future token transfers or contract interactions.

### Example Implementations

{% embed url="https://github.com/porco-rosso-j/zksync-account-webauthn/tree/main" %}
WebAuth AA
{% endembed %}

{% embed url="https://github.com/matter-labs/era-system-contracts/blob/main/contracts/DefaultAccount.sol" %}
Default Account zkSync
{% endembed %}

{% embed url="https://github.com/porco-rosso-j/zksync-account-trade-limit" %}
Account Trade Limit&#x20;
{% endembed %}

{% embed url="https://github.com/porco-rosso-j/zksync-aa-wallet-paymaster/tree/main" %}
AA Wallet With Paymaster
{% endembed %}
