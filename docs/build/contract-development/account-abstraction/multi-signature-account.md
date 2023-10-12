# Multi-Signature Account

## Introduction

This guide walks you through the process of constructing, deploying, and interacting with a 2-of-2 multisignature account using a factory contract on zkSync Era.

### Prerequisites

* **Knowledge Base**: You should be familiar with Solidity and Hardhat.
* **Wallet Setup**: Ensure your zkSync testnet wallet holds a sufficient balance.&#x20;
* **Tooling**: This guide utilizes [`zksync-cli`](../../../tooling/zksync-cli.md). Ensure you have it accessible or installed in your environment.

### Step 1 - Understanding Multi-Signature Account

The `TwoUserMultisig` contract implements both `IAccount` and `IERC1271` interfaces, inheriting zkSync's account logic and the [EIP-1271](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/83277ff916ac4f58fec072b8f28a252c1245c2f1/contracts/interfaces/IERC1271.sol#L12) standard for signatures. It supports transactions from two owners.

**Key Components**

* **`validateTransaction`:** Called by the bootloader, triggers internal function `_validateTransaction` for actual validation.
  * Validates transaction nonce.
  * Checks account balance for sufficient funds.
  * Confirms that the transaction signature is valid.
* **`isValidSignature`**: Validates if a given hash and signature correspond to the account's owners.
  * Utilizes ECDSA for signature recovery and validation.

### Step 2 - Environment setup

Using `zksync-cli` create a new project with the required dependencies and boilerplate paymaster implementations:

<pre class="language-bash"><code class="lang-bash"><strong>npx zksync-cli@latest create-project multi-sig-account --template hardhat_solidity
</strong></code></pre>

Add the zkSync and OpenZeppelin contract libraries:

```sh
yarn add -D @matterlabs/zksync-contracts @openzeppelin/contracts
```

Include the **`isSystem: true`** setting in the `hardhat.config.ts` configuration file to allow interaction with system contracts:

<details>

<summary>hardhat.config.ts</summary>

```typescript
import { HardhatUserConfig } from "hardhat/config";

import "@matterlabs/hardhat-zksync-deploy";
import "@matterlabs/hardhat-zksync-solc";

import "@matterlabs/hardhat-zksync-verify";

const config: HardhatUserConfig = {
  zksolc: {
    version: "latest", // Uses latest available in https://github.com/matter-labs/zksolc-bin/
    settings: {
      isSystem: true, // make sure to include this line
    },
  },
  defaultNetwork: "zkSyncTestnet",
  networks: {
    zkSyncTestnet: {
      url: "https://testnet.era.zksync.dev",
      ethNetwork: "goerli", // Can also be the RPC URL of the network (e.g. `https://goerli.infura.io/v3/<API_KEY>`)
      zksync: true,
    },
  },
  solidity: {
    version: "0.8.17",
  },
};

export default config;
```

</details>

**Update the Environment File**:

* Modify the `.env-example` file with your private key.
* Ensure your account has a sufficient balance.

### Step 3 - Writing `TwoUserMultisig.sol` contract

1. Create a new Solidity file named `TwoUserMultisig.sol` in the `/contracts` directory.
2. Insert the provided code into `TwoUserMultisig.sol`.

<details>

<summary>TwoUserMultisig.sol</summary>

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

import "@matterlabs/zksync-contracts/l2/system-contracts/interfaces/IAccount.sol";
import "@matterlabs/zksync-contracts/l2/system-contracts/libraries/TransactionHelper.sol";

import "@openzeppelin/contracts/interfaces/IERC1271.sol";

// Used for signature validation
import "@openzeppelin/contracts/utils/cryptography/ECDSA.sol";

// Access zkSync system contracts for nonce validation via NONCE_HOLDER_SYSTEM_CONTRACT
import "@matterlabs/zksync-contracts/l2/system-contracts/Constants.sol";
// to call non-view function of system contracts
import "@matterlabs/zksync-contracts/l2/system-contracts/libraries/SystemContractsCaller.sol";

contract TwoUserMultisig is IAccount, IERC1271 {
    // to get transaction hash
    using TransactionHelper for Transaction;

    // state variables for account owners
    address public owner1;
    address public owner2;

    bytes4 constant EIP1271_SUCCESS_RETURN_VALUE = 0x1626ba7e;

    modifier onlyBootloader() {
        require(
            msg.sender == BOOTLOADER_FORMAL_ADDRESS,
            "Only bootloader can call this function"
        );
        // Continue execution if called from the bootloader.
        _;
    }

    constructor(address _owner1, address _owner2) {
        owner1 = _owner1;
        owner2 = _owner2;
    }

    function validateTransaction(
        bytes32,
        bytes32 _suggestedSignedHash,
        Transaction calldata _transaction
    ) external payable override onlyBootloader returns (bytes4 magic) {
        return _validateTransaction(_suggestedSignedHash, _transaction);
    }

    function _validateTransaction(
        bytes32 _suggestedSignedHash,
        Transaction calldata _transaction
    ) internal returns (bytes4 magic) {
        // Incrementing the nonce of the account.
        // Note, that reserved[0] by convention is currently equal to the nonce passed in the transaction
        SystemContractsCaller.systemCallWithPropagatedRevert(
            uint32(gasleft()),
            address(NONCE_HOLDER_SYSTEM_CONTRACT),
            0,
            abi.encodeCall(INonceHolder.incrementMinNonceIfEquals, (_transaction.nonce))
        );

        bytes32 txHash;
        // While the suggested signed hash is usually provided, it is generally
        // not recommended to rely on it to be present, since in the future
        // there may be tx types with no suggested signed hash.
        if (_suggestedSignedHash == bytes32(0)) {
            txHash = _transaction.encodeHash();
        } else {
            txHash = _suggestedSignedHash;
        }

        // The fact there is enough balance for the account
        // should be checked explicitly to prevent user paying for fee for a
        // transaction that wouldn't be included on Ethereum.
        uint256 totalRequiredBalance = _transaction.totalRequiredBalance();
        require(totalRequiredBalance <= address(this).balance, "Not enough balance for fee + value");

        if (isValidSignature(txHash, _transaction.signature) == EIP1271_SUCCESS_RETURN_VALUE) {
            magic = ACCOUNT_VALIDATION_SUCCESS_MAGIC;
        } else {
            magic = bytes4(0);
        }
    }

    function executeTransaction(
        bytes32,
        bytes32,
        Transaction calldata _transaction
    ) external payable override onlyBootloader {
        _executeTransaction(_transaction);
    }

    function _executeTransaction(Transaction calldata _transaction) internal {
        address to = address(uint160(_transaction.to));
        uint128 value = Utils.safeCastToU128(_transaction.value);
        bytes memory data = _transaction.data;

        if (to == address(DEPLOYER_SYSTEM_CONTRACT)) {
            uint32 gas = Utils.safeCastToU32(gasleft());

            // Note, that the deployer contract can only be called
            // with a "systemCall" flag.
            SystemContractsCaller.systemCallWithPropagatedRevert(gas, to, value, data);
        } else {
            bool success;
            assembly {
                success := call(gas(), to, value, add(data, 0x20), mload(data), 0, 0)
            }
            require(success);
        }
    }

    function executeTransactionFromOutside(Transaction calldata _transaction)
        external
        payable
    {
        _validateTransaction(bytes32(0), _transaction);
        _executeTransaction(_transaction);
    }

    function isValidSignature(bytes32 _hash, bytes memory _signature)
        public
        view
        override
        returns (bytes4 magic)
    {
        magic = EIP1271_SUCCESS_RETURN_VALUE;

        if (_signature.length != 130) {
            // Signature is invalid anyway, but we need to proceed with the signature verification as usual
            // in order for the fee estimation to work correctly
            _signature = new bytes(130);

            // Making sure that the signatures look like a valid ECDSA signature and are not rejected rightaway
            // while skipping the main verification process.
            _signature[64] = bytes1(uint8(27));
            _signature[129] = bytes1(uint8(27));
        }

        (bytes memory signature1, bytes memory signature2) = extractECDSASignature(_signature);

        if(!checkValidECDSASignatureFormat(signature1) || !checkValidECDSASignatureFormat(signature2)) {
            magic = bytes4(0);
        }

        address recoveredAddr1 = ECDSA.recover(_hash, signature1);
        address recoveredAddr2 = ECDSA.recover(_hash, signature2);

        // Note, that we should abstain from using the require here in order to allow for fee estimation to work
        if(recoveredAddr1 != owner1 || recoveredAddr2 != owner2) {
            magic = bytes4(0);
        }
    }

    // This function verifies that the ECDSA signature is both in correct format and non-malleable
    function checkValidECDSASignatureFormat(bytes memory _signature) internal pure returns (bool) {
        if(_signature.length != 65) {
            return false;
        }

        uint8 v;
		bytes32 r;
		bytes32 s;
		// Signature loading code
		// we jump 32 (0x20) as the first slot of bytes contains the length
		// we jump 65 (0x41) per signature
		// for v we load 32 bytes ending with v (the first 31 come from s) then apply a mask
		assembly {
			r := mload(add(_signature, 0x20))
			s := mload(add(_signature, 0x40))
			v := and(mload(add(_signature, 0x41)), 0xff)
		}
		if(v != 27 && v != 28) {
            return false;
        }

		// EIP-2 still allows signature malleability for ecrecover(). Remove this possibility and make the signature
        // unique. Appendix F in the Ethereum Yellow paper (https://ethereum.github.io/yellowpaper/paper.pdf), defines
        // the valid range for s in (301): 0 < s < secp256k1n ÷ 2 + 1, and for v in (302): v ∈ {27, 28}. Most
        // signatures from current libraries generate a unique signature with an s-value in the lower half order.
        //
        // If your library generates malleable signatures, such as s-values in the upper range, calculate a new s-value
        // with 0xFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFEBAAEDCE6AF48A03BBFD25E8CD0364141 - s1 and flip v from 27 to 28 or
        // vice versa. If your library also generates signatures with 0/1 for v instead 27/28, add 27 to v to accept
        // these malleable signatures as well.
        if(uint256(s) > 0x7FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF5D576E7357A4501DDFE92F46681B20A0) {
            return false;
        }

        return true;
    }

    function extractECDSASignature(bytes memory _fullSignature) internal pure returns (bytes memory signature1, bytes memory signature2) {
        require(_fullSignature.length == 130, "Invalid length");

        signature1 = new bytes(65);
        signature2 = new bytes(65);

        // Copying the first signature. Note, that we need an offset of 0x20
        // since it is where the length of the `_fullSignature` is stored
        assembly {
            let r := mload(add(_fullSignature, 0x20))
			let s := mload(add(_fullSignature, 0x40))
			let v := and(mload(add(_fullSignature, 0x41)), 0xff)

            mstore(add(signature1, 0x20), r)
            mstore(add(signature1, 0x40), s)
            mstore8(add(signature1, 0x60), v)
        }

        // Copying the second signature.
        assembly {
            let r := mload(add(_fullSignature, 0x61))
            let s := mload(add(_fullSignature, 0x81))
            let v := and(mload(add(_fullSignature, 0x82)), 0xff)

            mstore(add(signature2, 0x20), r)
            mstore(add(signature2, 0x40), s)
            mstore8(add(signature2, 0x60), v)
        }
    }

    function payForTransaction(
        bytes32,
        bytes32,
        Transaction calldata _transaction
    ) external payable override onlyBootloader {
        bool success = _transaction.payToTheBootloader();
        require(success, "Failed to pay the fee to the operator");
    }

    function prepareForPaymaster(
        bytes32, // _txHash
        bytes32, // _suggestedSignedHash
        Transaction calldata _transaction
    ) external payable override onlyBootloader {
        _transaction.processPaymasterInput();
    }

    fallback() external {
        // fallback of default account shouldn't be called by bootloader under no circumstances
        assert(msg.sender != BOOTLOADER_FORMAL_ADDRESS);

        // If the contract is called directly, behave like an EOA
    }

    receive() external payable {
        // If the contract is called directly, behave like an EOA.
        // Note, that is okay if the bootloader sends funds with no calldata as it may be used for refunds/operator payments
    }
}

```

</details>

Let's explain the `TwoUserMultisig` contract.

**State Variables**

* `owner1`: Address of the first account owner.
* `owner2`: Address of the second account owner.

```solidity
address public owner1;
address public owner2;
```

**Functions & Modifiers**

**Modifiers**

* `onlyBootloader`: Ensures only the bootloader can call specific functions.

**Transaction Functions**

* `isValidSignature`: Validates transaction signatures.
* `_validateTransaction`: Validates and processes a transaction.
* `_executeTransaction`: Executes a transaction.

**Helper Functions**

* `checkValidECDSASignatureFormat`: Validates the ECDSA signature format.
* `extractECDSASignature`: Extracts ECDSA signatures from a concatenated byte array.

**Constructor**

```solidity
constructor(address _owner1, address _owner2) {
    owner1 = _owner1;
    owner2 = _owner2;
}
```

Sets the initial state variables for account owners.

**Signature Validation**

It is recommended to make use of OpenZeppelin's ECDSA library for signature validation.

```solidity
function isValidSignature(bytes32 _hash, bytes memory _signature)
    public
    view
    override
    returns (bytes4 magic)
{
    // Implementation
}
```

* Validates the length of received signatures.
* Uses helper functions for ECDSA signature extraction and format validation.
* Uses `ECDSA.recover` to recover addresses from signatures and compares them with account owners.
* Check if the addresses extracted match with the owners of the account.
* Return the `EIP1271_SUCCESS_RETURN_VALUE` value on success or `bytes4(0)` if validation fails.

**Transaction Validation & Execution**

The transaction validation process is responsible for validating the signature of the transaction and incrementing the nonce.

```solidity
function _validateTransaction(bytes32 _suggestedSignedHash, Transaction calldata _transaction) internal returns (bytes4 magic) {
    // Implementation
}
```

* Calls a system contract (NONCE\_HOLDER\_SYSTEM\_CONTRACT) for nonce validation.
* Validates the account's balance for transaction fees and value.
* Calls `isValidSignature` to validate the transaction signature.
* Returns the constant `ACCOUNT_VALIDATION_SUCCESS_MAGIC` if the validation is successful, or an empty value `bytes4(0)` if it fails.

```solidity
function _executeTransaction(Transaction calldata _transaction) internal {
    // Implementation
}
```

* Extracts the transaction data and executes it.
* Requires successful transaction execution.

**Paying Fees for Transactions**

```solidity
function payForTransaction(bytes32, bytes32, Transaction calldata _transaction) external payable override onlyBootloader {
    // Implementation
}
```

* Calls a helper function to calculate and send the transaction fees to the bootloader.

**Paymaster Support**

```solidity
function prepareForPaymaster(bytes32, bytes32, Transaction calldata _transaction) external payable override onlyBootloader {
    // Implementation
}
```

* Processes the paymaster parameters.

### Step 4 - Creating Account Abstraction Factory

The `AAFactory.sol` contract is responsible for deploying instances of the `Account.sol` contract.

1. Create a new Solidity file named `AAFactory.sol` in the `/contracts` directory.
2. Insert the provided code into `AAFactory.sol`.

<details>

<summary>AAFactory.sol</summary>

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

import "@matterlabs/zksync-contracts/l2/system-contracts/Constants.sol";
import "@matterlabs/zksync-contracts/l2/system-contracts/libraries/SystemContractsCaller.sol";

contract AAFactory {
    bytes32 public aaBytecodeHash;

    constructor(bytes32 _aaBytecodeHash) {
        aaBytecodeHash = _aaBytecodeHash;
    }

    function deployAccount(
        bytes32 salt,
        address owner1,
        address owner2
    ) external returns (address accountAddress) {
        (bool success, bytes memory returnData) = SystemContractsCaller
            .systemCallWithReturndata(
                uint32(gasleft()),
                address(DEPLOYER_SYSTEM_CONTRACT),
                uint128(0),
                abi.encodeCall(
                    DEPLOYER_SYSTEM_CONTRACT.create2Account,
                    (salt, aaBytecodeHash, abi.encode(owner1, owner2), IContractDeployer.AccountAbstractionVersion.Version1)
                )
            );
        require(success, "Deployment failed");

        (accountAddress) = abi.decode(returnData, (address));
    }
}
```

</details>

* To deploy the multisig smart contract, it is necessary to interact with the `DEPLOYER_SYSTEM_CONTRACT` and call the `create2Account` function.
* If the code doesn't do this, you may see errors like **`Validation revert: Sender is not an account`**

Contract deployments are not done via bytecode, but via bytecode hash. The bytecode itself is passed to the operator via the `factoryDeps` field. Note that the `_aaBytecodeHash` must be formed in the following manner:

* Firstly, it is hashed with `sha256`.
* Then, the first two bytes are replaced with the length of the bytecode in 32-byte words.

### Step 5 - Compile and deploy contracts

This step outlines the process of compiling your smart contracts and deploying a factory account.

Compile the contracts:

```sh
yarn hardhat compile
```

**Create Deployment Script**

1. Create a new TypeScript file in the `/deploy` directory, for example, `deployFactoryAccount.ts`.
2. Populate `deployFactoryAccount.ts` with the provided script, replacing `<DEPLOYER_PRIVATE_KEY>` with your own private key.

<details>

<summary><code>deployFactoryAccount.ts</code></summary>

```typescript
import { utils, Wallet } from "zksync-web3";
import * as ethers from "ethers";
import { HardhatRuntimeEnvironment } from "hardhat/types";
import { Deployer } from "@matterlabs/hardhat-zksync-deploy";

export default async function (hre: HardhatRuntimeEnvironment) {
  // Private key of the account used to deploy
  const wallet = new Wallet("<WALLET_PRIVATE_KEY>");
  const deployer = new Deployer(hre, wallet);
  const factoryArtifact = await deployer.loadArtifact("AAFactory");
  const aaArtifact = await deployer.loadArtifact("TwoUserMultisig");

  // Getting the bytecodeHash of the account
  const bytecodeHash = utils.hashBytecode(aaArtifact.bytecode);

  const factory = await deployer.deploy(factoryArtifact, [bytecodeHash], undefined, [
    // Since the factory requires the code of the multisig to be available,
    // we should pass it here as well.
    aaArtifact.bytecode,
  ]);

  console.log(`AA factory address: ${factory.address}`);
}
```

</details>

Run the script:

```sh
yarn hardhat deploy-zksync --script deployFactoryAccount.ts
```

You should see something like this:

```txt
AA factory address: 0x9db333Cb68Fb6D317E3E415269a5b9bE7c72627Ds
```

**Deploying and funding an account**

Now, let's deploy an account and use it to initiate a new transaction.

1. Create a new TypeScript file in the `/deploy` directory, for example, `deploy-multisig.ts`.
2. Populate `deploy-multisig.ts` with the provided script, replacing `<FACTORY-ADDRESS>` with the factory address previously deployed and `<WALLET-PRIVATE-KEY>` with a funded account.

<details>

<summary>deploy-multisig.ts</summary>

```typescript
import { utils, Wallet, Provider, EIP712Signer, types } from "zksync-web3";
import * as ethers from "ethers";
import { HardhatRuntimeEnvironment } from "hardhat/types";

// Put the address of your AA factory
const AA_FACTORY_ADDRESS = "<FACTORY-ADDRESS>";

export default async function (hre: HardhatRuntimeEnvironment) {
  const provider = new Provider("https://testnet.era.zksync.dev");
  // Private key of the account used to deploy
  const wallet = new Wallet("<WALLET-PRIVATE-KEY>").connect(provider);
  const factoryArtifact = await hre.artifacts.readArtifact("AAFactory");

  const aaFactory = new ethers.Contract(AA_FACTORY_ADDRESS, factoryArtifact.abi, wallet);

  // The two owners of the multisig
  const owner1 = Wallet.createRandom();
  const owner2 = Wallet.createRandom();

  // For the simplicity of the tutorial, we will use zero hash as salt
  const salt = ethers.constants.HashZero;

  // deploy account owned by owner1 & owner2
  const tx = await aaFactory.deployAccount(salt, owner1.address, owner2.address);
  await tx.wait();

  // Getting the address of the deployed contract account
  // Always use the JS utility methods
  const abiCoder = new ethers.utils.AbiCoder();
  const multisigAddress = utils.create2Address(
    AA_FACTORY_ADDRESS,
    await aaFactory.aaBytecodeHash(),
    salt,
    abiCoder.encode(["address", "address"], [owner1.address, owner2.address])
  );
  console.log(`Multisig account deployed on address ${multisigAddress}`);

  console.log("Sending funds to multisig account");
  // Send funds to the multisig account we just deployed
  await (
    await wallet.sendTransaction({
      to: multisigAddress,
      // You can increase the amount of ETH sent to the multisig
      value: ethers.utils.parseEther("0.008"),
    })
  ).wait();

  let multisigBalance = await provider.getBalance(multisigAddress);

  console.log(`Multisig account balance is ${multisigBalance.toString()}`);

  // Transaction to deploy a new account using the multisig we just deployed
  let aaTx = await aaFactory.populateTransaction.deployAccount(
    salt,
    // These are accounts that will own the newly deployed account
    Wallet.createRandom().address,
    Wallet.createRandom().address
  );

  const gasLimit = await provider.estimateGas(aaTx);
  const gasPrice = await provider.getGasPrice();

  aaTx = {
    ...aaTx,
    // deploy a new account using the multisig
    from: multisigAddress,
    gasLimit: gasLimit,
    gasPrice: gasPrice,
    chainId: (await provider.getNetwork()).chainId,
    nonce: await provider.getTransactionCount(multisigAddress),
    type: 113,
    customData: {
      gasPerPubdata: utils.DEFAULT_GAS_PER_PUBDATA_LIMIT,
    } as types.Eip712Meta,
    value: ethers.BigNumber.from(0),
  };
  const signedTxHash = EIP712Signer.getSignedDigest(aaTx);

  const signature = ethers.utils.concat([
    // Note, that `signMessage` wouldn't work here, since we don't want
    // the signed hash to be prefixed with `\x19Ethereum Signed Message:\n`
    ethers.utils.joinSignature(owner1._signingKey().signDigest(signedTxHash)),
    ethers.utils.joinSignature(owner2._signingKey().signDigest(signedTxHash)),
  ]);

  aaTx.customData = {
    ...aaTx.customData,
    customSignature: signature,
  };

  console.log(`The multisig's nonce before the first tx is ${await provider.getTransactionCount(multisigAddress)}`);
  const sentTx = await provider.sendTransaction(utils.serialize(aaTx));
  await sentTx.wait();

  // Checking that the nonce for the account has increased
  console.log(`The multisig's nonce after the first tx is ${await provider.getTransactionCount(multisigAddress)}`);

  multisigBalance = await provider.getBalance(multisigAddress);

  console.log(`Multisig account balance is now ${multisigBalance.toString()}`);
}

```

</details>

1. **Deposits ETH**: Adds sufficient ETH to the multisig account to cover transaction fees.
2. **Deploys New Multisig Account**: Initiates the deployment of a new multisig account with two specified owners.
3. **Transaction Creation**: Constructs a new transaction for the desired operation.
4. **Transaction Signing**: Utilizes EIP712Signer to securely sign the transaction with the required credentials.
5. **Transaction Submission**: Sends the signed transaction to the network for execution.

Run the script from the `deploy` folder.

```sh
yarn hardhat deploy-zksync --script deploy-multisig.ts
```

The output should look something like this:

```txt
Multisig deployed on address 0xCEBc59558938bccb43A6C94769F87bBdb770E956
The multisig's nonce before the first tx is 0
The multisig's nonce after the first tx is 1
```

### Conclusion

The `TwoUserMultisig` contract provides a 2-of-2 multisig account. The usage guide provides insights into how account abstraction contracts can be developed and the power of customization. This serves a practical blueprint to create other intriguing AA contract implementations.&#x20;
