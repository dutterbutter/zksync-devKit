# CLI

###

### Usage

```plaintext
Usage: era_test_node [OPTIONS] <COMMAND>
```

### Commands

<table><thead><tr><th width="188">Command</th><th>Description</th></tr></thead><tbody><tr><td><code>run</code></td><td>Starts a new empty local network</td></tr><tr><td><code>fork</code></td><td>Starts a local network that is a fork of another network</td></tr><tr><td><code>replay_tx</code></td><td>Starts a local network that is a fork of another network, and replays a given TX on it</td></tr><tr><td><code>help</code></td><td>Print this message or the help of the given subcommand(s)</td></tr></tbody></table>

### Options

<table><thead><tr><th width="215">Option</th><th width="313">Description</th><th>Default Value</th><th>Possible Values</th></tr></thead><tbody><tr><td><code>--port &#x3C;PORT></code></td><td>Port to listen on</td><td><code>8011</code></td><td></td></tr><tr><td><code>--show-calls &#x3C;SHOW_CALLS></code></td><td>Show call debug information</td><td><code>none</code></td><td><code>none</code>, <code>user</code>, <code>system</code>, <code>all</code></td></tr><tr><td><code>--show-storage-logs &#x3C;SHOW_STORAGE_LOGS></code></td><td>Show storage log information</td><td><code>none</code></td><td><code>none</code>, <code>read</code>, <code>write</code>, <code>all</code></td></tr><tr><td><code>--show-vm-details &#x3C;SHOW_VM_DETAILS></code></td><td>Show VM details information</td><td><code>none</code></td><td><code>none</code>, <code>all</code></td></tr><tr><td><code>--show-gas-details &#x3C;SHOW_GAS_DETAILS></code></td><td>Show Gas details information</td><td><code>none</code></td><td><code>none</code>, <code>all</code></td></tr><tr><td><code>--resolve-hashes</code></td><td>If true, the tool will try to contact openchain to resolve the ABI &#x26; topic names. It will make debug log more readable, but will decrease the performance</td><td></td><td></td></tr><tr><td><code>--dev-use-local-contracts</code></td><td>If true, will load the locally compiled system contracts (useful when doing changes to system contracts or bootloader)</td><td></td><td></td></tr><tr><td><code>--log &#x3C;LOG></code></td><td>Log filter level</td><td><code>info</code></td><td><code>trace</code>, <code>debug</code>, <code>info</code>, <code>warn</code>, <code>error</code></td></tr><tr><td><code>--log-file-path &#x3C;LOG_FILE_PATH></code></td><td>Log file path</td><td><code>era_test_node.log</code></td><td></td></tr><tr><td><code>--cache &#x3C;CACHE></code></td><td>Cache type, can be one of <code>none</code>, <code>memory</code>, or <code>disk</code></td><td><code>disk</code></td><td><code>none</code>, <code>memory</code>, <code>disk</code></td></tr><tr><td><code>--reset-cache</code></td><td>If true, will reset the local <code>disk</code> cache</td><td></td><td></td></tr><tr><td><code>--cache-dir &#x3C;CACHE_DIR></code></td><td>Cache directory location for <code>disk</code> cache</td><td><code>.cache</code></td><td></td></tr><tr><td><code>-h, --help</code></td><td>Print help</td><td></td><td></td></tr><tr><td><code>-V, --version</code></td><td>Print version</td><td></td><td></td></tr></tbody></table>
