# Installation

### **Prerequisites**

#### Supported Operating Systems

* **\*nix OS**: Any Linux distribution or MacOS is supported.
* **Windows**: Use WSL 2 (WSL 1 can cause issues). Ensure projects are in the Linux filesystem due to NTFS partition access limitations.
* **MacOS with ARM (M1/M2)**: Ensure tools and IDEs run natively and not via Rosetta.
* **NixOS Users**: For a reproducible environment, refer to the `nix` section below.

### **Setting Up Development Tools**

#### Git

For SSH key authentication with Github, configure git to always use SSH:

```bash
git config --global url."ssh://git@github.com/".insteadOf https://github.com/
```

[More on our git usage](https://www.notion.so/matterlabs/Working-with-dependencies-in-private-repositories-697620178338452798a0ea5ac0d8e56a).

#### Docker & Docker-compose

* Install from the [official site](https://docs.docker.com/install/).
  * _Note_: Avoid Docker Desktop for Linux due to its quirks. Instead, opt for `docker-ce` ([Guide for Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04)).
  * `docker-compose` comes with Docker Desktop.
* Address potential Linux errors: Ensure your user is in the `docker` group.

#### Node & Yarn

* Use `Node v18.18.0`. Consider using [nvm](https://github.com/nvm-sh/nvm) for easy Node.js version management.
* Install `yarn v1.22.19`. Follow the [official installation guide](https://classic.yarnpkg.com/en/docs/install/).

#### Additional Dependencies

**Axel** (>=2.17.10): For downloading keys:

{% tabs %}
{% tab title="MacOS" %}
```bash
brew install axel
```
{% endtab %}

{% tab title="Linux" %}
```bash
sudo apt-get install axel
```
{% endtab %}
{% endtabs %}

**clang:** Needed for RocksDB.

{% tabs %}
{% tab title="MacOS" %}
```bash
# Requires an updated Xcode from the App Store.
# OR
xcode-select --install
```
{% endtab %}

{% tab title="Linux" %}
```bash
sudo apt-get install build-essential pkg-config cmake clang lldb lld
```
{% endtab %}
{% endtabs %}

**OpenSSL**

{% tabs %}
{% tab title="MacOS" %}
```bash
brew install openssl
```
{% endtab %}

{% tab title="Linux" %}
```bash
sudo apt-get install libssl-dev
```
{% endtab %}
{% endtabs %}

**Rust:** Install the latest version from the [official site](https://www.rust-lang.org/tools/install).

{% tabs %}
{% tab title="Rust" %}
```bash
# ARM Mac users: Ensure aarch64 toolchain is in use.
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
{% endtab %}
{% endtabs %}

#### Database and Language Support

* **Postgres**: `brew install postgresql`
*   **Cargo nextest**: Next-gen test runner for Rust projects.

    ```bash
    cargo install cargo-nextest
    ```
*   **SQLx CLI**: Interact with Postgres in Rust.

    ```bash
    cargo install sqlx-cli --version 0.5.13
    ```
* **Solidity compiler `solc`**: Install via `brew install solidity` or get a [precompiled version](https://github.com/ethereum/solc-bin).
* **Python**: Ensure it's installed in your environment.

#### Tips and Performance Enhancements

* **`mold`**: Optimize build time using the modern linker [`mold`](https://github.com/rui314/mold).
* **RocksDB**: Speed up builds by using precompiled versions of the library.

### **Easier Method Using `nix`**

Nix fetches dependencies specified via hashes.

* Install `nix`, enable command and flakes.
* Continue with Docker, Rustup, SQLx CLI setup.
* If on NixOS, enable nix-ld.
* In the zksync folder, run: `nix develop --impure`.

### **Environment Setup**

Add the following to your shell profile (e.g., `~/.bash_profile`, `~/.zshrc`):

```bash
export ZKSYNC_HOME=/path/to/zksync
export PATH=$ZKSYNC_HOME/bin:$PATH
```
