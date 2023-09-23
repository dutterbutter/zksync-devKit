# CI/CD

A GitHub Action is available for integrating `era-test-node` into your CI/CD environments. This action offers high configurability and streamlines the process of testing your applications in an automated way.

You can find this GitHub Action in the marketplace [here](https://github.com/marketplace/actions/era-test-node-action).

#### Example Usage <a href="#user-content--example-usage" id="user-content--example-usage"></a>

Below is an example `yaml` configuration to use the `era-test-node` GitHub Action in your workflow:

```
name: Run Era Test Node Action

on:
  push:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Run Era Test Node
      uses: dutterbutter/era-test-node-action@latest
```
