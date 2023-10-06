# Contract Development

### Overview

**zkSync Era** is a [ZK rollup](https://era.zksync.io/docs/reference/concepts/rollups.html#what-are-zk-rollups), a trustless protocol that uses cryptographic validity proofs to provide scalable and low-cost transactions on Ethereum. In zkSync Era, computation is performed off-chain and most data is stored off-chain as well. As all transactions are proven on the Ethereum mainchain, users enjoy the same security level as in Ethereum.

zkSync Era is made to look and feel like Ethereum, but with lower fees. Just like on Ethereum, smart contracts are written in Solidity/Vyper and can be called using the same clients as the other EVM-compatible chains.

You don't need to register a separate private key before usage; zkSync supports existing Ethereum wallets out of the box.

### Useful Addresses

### Mainnet

<table><thead><tr><th width="220">Contract</th><th>Address</th></tr></thead><tbody><tr><td>DiamondCutFacet</td><td><a href="https://etherscan.io/address/0x2a2d6010202B93E727b61a60dfC1d5CF2707c1CE#code">0x2a2d6010202B93E727b61a60dfC1d5CF2707c1CE</a></td></tr><tr><td>DiamondInit</td><td><a href="https://etherscan.io/address/0xb91d905A698c28b73C61aF60C63919b754FCF4DE#code">0xb91d905A698c28b73C61aF60C63919b754FCF4DE</a></td></tr><tr><td>DiamondProxy</td><td><a href="https://etherscan.io/address/0x32400084c286cf3e17e7b677ea9583e60a000324#code">0x32400084c286cf3e17e7b677ea9583e60a000324</a></td></tr><tr><td>DiamondUpgrade</td><td><a href="https://etherscan.io/address/0xf48b2a42712BFBBb95f4000AEe3873410DC0546F#code">0xf48b2a42712BFBBb95f4000AEe3873410DC0546F</a></td></tr><tr><td>ExecutorFacet</td><td><a href="https://etherscan.io/address/0x389a081BCf20e5803288183b929F08458F1d863D#code">0x389a081BCf20e5803288183b929F08458F1d863D</a></td></tr><tr><td>GettersFacet</td><td><a href="https://etherscan.io/address/0xF1fB730b7f8E8391B27B91f8f791e10E4a53CEcc#code">0xF1fB730b7f8E8391B27B91f8f791e10E4a53CEcc</a></td></tr><tr><td>GovernanceFacet</td><td><a href="https://etherscan.io/address/0x6df4A6D71622860dcc64C1FD9645d9a5BE96f088#code">0x6df4A6D71622860dcc64C1FD9645d9a5BE96f088</a></td></tr><tr><td>Verifier</td><td><a href="https://etherscan.io/address/0x020b26826C23142f2582733b2E6428EE31eAaB49#code">0x020b26826C23142f2582733b2E6428EE31eAaB49open in new window</a></td></tr><tr><td>MailboxFacet</td><td><a href="https://etherscan.io/address/0xb2097DBe4410B538a45574B1FCD767E2303c7867#code">0xb2097DBe4410B538a45574B1FCD767E2303c7867</a></td></tr><tr><td>AllowList</td><td><a href="https://etherscan.io/address/0x8ffd57A9B2dcc10327768b601468FA192adC5C86#code">0x8ffd57A9B2dcc10327768b601468FA192adC5C86</a></td></tr></tbody></table>

**Token bridge contract addresses:**

<table><thead><tr><th width="218">Contract</th><th>Address</th></tr></thead><tbody><tr><td>L1ERC20BridgeProxy</td><td><a href="https://etherscan.io/address/0x57891966931Eb4Bb6FB81430E6cE0A03AAbDe063#code">0x57891966931Eb4Bb6FB81430E6cE0A03AAbDe063</a></td></tr><tr><td>L1ERC20BridgeImpl</td><td><a href="https://etherscan.io/address/0x7e5E66B01fe43293545eaB98ec4D31784A5Efa84#code">0x7e5E66B01fe43293545eaB98ec4D31784A5Efa84</a></td></tr><tr><td>L2ERC20Bridge</td><td><a href="https://explorer.zksync.io/address/0x11f943b2c77b743AB90f4A0Ae7d5A4e7FCA3E102">0x11f943b2c77b743AB90f4A0Ae7d5A4e7FCA3E102</a></td></tr></tbody></table>

**Token addresses:**

For a full list of mainnet token addresses please refer to the table [here](https://explorer.zksync.io/tokenlist).

### Testnet

Contract addresses:

<table><thead><tr><th width="224">Contract</th><th>Address</th></tr></thead><tbody><tr><td>DiamondCutFacet</td><td><a href="https://goerli.etherscan.io/address/0x6F883c7DA8Ec1918fa83C5E57F239f47f03b135d#code">0x6F883c7DA8Ec1918fa83C5E57F239f47f03b135d</a></td></tr><tr><td>DiamondInit</td><td><a href="https://goerli.etherscan.io/address/0x81aE464127286C26f21495d053AA19Eec708055F#code">0x81aE464127286C26f21495d053AA19Eec708055F</a></td></tr><tr><td>DiamondProxy</td><td><a href="https://goerli.etherscan.io/address/0x1908e2BF4a88F91E4eF0DC72f02b8Ea36BEa2319#code">0x1908e2BF4a88F91E4eF0DC72f02b8Ea36BEa2319</a></td></tr><tr><td>DiamondUpgrade</td><td><a href="https://goerli.etherscan.io/address/0xFC88e9e4e11B1C083B40197500827E1894d55a83#code">0xFC88e9e4e11B1C083B40197500827E1894d55a83</a></td></tr><tr><td>ExecutorFacet</td><td><a href="https://goerli.etherscan.io/address/0x9B276BD8D84901a8e57F980C05A6aD7Fee5c241d#code">0x9B276BD8D84901a8e57F980C05A6aD7Fee5c241d</a></td></tr><tr><td>GettersFacet</td><td><a href="https://goerli.etherscan.io/address/0x71b3Ffda716Ef5df529FA89a8BBb8D16676fD47f#code">0x71b3Ffda716Ef5df529FA89a8BBb8D16676fD47f</a></td></tr><tr><td>GovernanceFacet</td><td><a href="https://goerli.etherscan.io/address/0xc288177781D3555822edB31D323aEcB6cFD849c7#code">0xc288177781D3555822edB31D323aEcB6cFD849c7</a></td></tr><tr><td>Verifier</td><td><a href="https://goerli.etherscan.io/address/0xc8517230276e0df51377ecc07b528cd3ee083132#code">0xc8517230276e0df51377ecc07b528cd3ee083132</a></td></tr><tr><td>MailboxFacet</td><td><a href="https://goerli.etherscan.io/address/0xd80eF7aCBEC07dbf10Eb84452b40D0a8882ADfB5#code">0xd80eF7aCBEC07dbf10Eb84452b40D0a8882ADfB5</a></td></tr><tr><td>AllowList</td><td><a href="https://goerli.etherscan.io/address/0xCbA757b4f0527b535bE80720325064058FC4A306#code">0xCbA757b4f0527b535bE80720325064058FC4A306</a></td></tr><tr><td>L2TestnetPaymaster</td><td><a href="https://goerli.explorer.zksync.io/address/0x8f0ea1312da29f17eabeb2f484fd3c112cccdd63#contract">0x8f0ea1312da29f17eabeb2f484fd3c112cccdd63</a></td></tr></tbody></table>

#### Testnet token bridge addresses: <a href="#testnet-token-bridge-contract-addresses" id="testnet-token-bridge-contract-addresses"></a>

These are the addresses that have been deployed and integrated with the token bridge on testnet.

<table><thead><tr><th width="229">Contract</th><th>Address</th></tr></thead><tbody><tr><td>L1ERC20BridgeProxy</td><td><a href="https://goerli.etherscan.io/address/0x927DdFcc55164a59E0F33918D13a2D559bC10ce7#code">0x927DdFcc55164a59E0F33918D13a2D559bC10ce7</a></td></tr><tr><td>L1ERC20BridgeImpl</td><td><a href="https://goerli.etherscan.io/address/0xE3E53270f3674965F12F70117B16736232604e12#code">0xE3E53270f3674965F12F70117B16736232604e12</a></td></tr><tr><td>L2ERC20Bridge</td><td><a href="https://goerli.explorer.zksync.io/address/0x00ff932A6d70E2B8f1Eb4919e1e09C1923E7e57b">0x00ff932A6d70E2B8f1Eb4919e1e09C1923E7e57b</a></td></tr></tbody></table>

**Token addresses:**

<table><thead><tr><th width="230.33333333333331">TOKEN NAME</th><th>TOKEN ADDRESS</th></tr></thead><tbody><tr><td>ETH</td><td><a href="https://goerli.explorer.zksync.io/address/0x000000000000000000000000000000000000800A">0x0000000000000000...800A</a></td></tr><tr><td>DAI</td><td><a href="https://goerli.explorer.zksync.io/address/0x3e7676937A7E96CFB7616f255b9AD9FF47363D4b">0x3e7676937A7E96CF...3D4b</a></td></tr><tr><td>LINK</td><td><a href="https://goerli.explorer.zksync.io/address/0x40609141Db628BeEE3BfAB8034Fc2D8278D0Cc78">0x40609141Db628BeE...Cc78</a></td></tr><tr><td>USDC</td><td><a href="https://goerli.explorer.zksync.io/address/0x0faF6df7054946141266420b43783387A78d82A9">0x0faF6df705494614...82A9</a></td></tr><tr><td>wBTC</td><td><a href="https://goerli.explorer.zksync.io/address/0x0BfcE1D53451B4a8175DD94e6e029F7d8a701e9c">0x0BfcE1D53451B4a8...1e9c</a></td></tr></tbody></table>
