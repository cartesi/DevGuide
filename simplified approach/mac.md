
## Introduction

This guide provides detailed instructions tailored for macOS users using Sunodo.


## Setting up Your  Development Environment

Here are the general requirements:
- Python3
- Node.js
- Yarn
- Docker Desktop
- Sunodo

### Installing & Configuring Docker

1. Download Docker Desktop using any of the URLs below 
- [Docker Desktop for Mac with Apple Silicon](https://desktop.docker.com/mac/main/arm64/Docker.dmg?utm_source=docker&utm_medium=webreferral&utm_campaign=docs-driven-download-mac-arm64&_gl=1*1phjm84*_ga*MTA0MTIzOTI4LjE2OTcyNzA0OTY.*_ga_XJWPQMJYHQ*MTY5NzQ2ODc2Ny41LjEuMTY5NzQ2ODkyNy42MC4wLjA)

- [Docker Desktop for Mac with Intel Chip](https://desktop.docker.com/mac/main/amd64/Docker.dmg?utm_source=docker&utm_medium=webreferral&utm_campaign=docs-driven-download-mac-amd64&_gl=1*vl017u*_ga*MTA0MTIzOTI4LjE2OTcyNzA0OTY.*_ga_XJWPQMJYHQ*MTY5NzQ2ODc2Ny41LjEuMTY5NzQ2ODkyNy42MC4wLjA.)

2. After the `Docker.dmg` file is downloaded, open the installer, then drag the Docker icon to the Applications folder. 
Double-click on the Docker.app in the Applications folder to start Docker.

### Installing Python

1. Install Homebrew (if not already installed):

     ```
     /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
     ```

2. Install Python 3 using the following command:

     ```
     brew install python@3
     ``` 

### Installing Node.js/NPM

1. If you have Homebrew installed, open a terminal and run:

    ```
    brew install node
    ```

### Installing Yarn

1. If you have Homebrew installed, open a terminal and run:

    ```
    brew install yarn
    ``` 


### Installing Sunodo


1. You can install Sunodo with Homebrew by running this command:

   ```
    brew install sunodo/tap/sunodo
   ```

2. Alternatively, you can install Sunodo with Node.js by running:

   ```
    npm install -g @sunodo/cli
   ```



## Build a dApp using Sunodo

In this section, we will build a dApp using Sunodo. Subsequent sections will guide you through building dApps using the primitive approach.


### Creating an application

Run `sunodo create` to quickly start a Cartesi dApp from scratch. It sets up everything you need with template code.

Here are the available templates:

- `cpp`: A template for C++ development.
- `cpp-low-level`: C++ template using the low level API, instead of the HTTP server
- `go`: Go lang template
- `javascript`: A node.js 20 template tailored for JavaScript developers
- `lua`: Lua 5.4 template
- `python`: python 3 template
- `ruby`: ruby template
- `rust`: rust template
- `typescript`: TypeScript template

To create a new application from a basic Python template, run:

```
sunodo create dapp-name --template python
```
### Building the application

To build an application, run:

```
sunodo build
```

When you run the `sunodo build` command:

Your program's code gets compiled into the RISC-V architecture
The end result of this process is a Cartesi Machine snapshot, ready to receive inputs.
 
### Running the application

This executes a Cartesi node for the application previously built with `sunodo build`.

```
sunodo run
```

### Sending inputs to the application

Your applications can receive inputs by sending transactions with the input payload.

To send inputs, use the command:

```
sunodo send
```

This command guides you through the process of sending inputs interactively.

```
? Select send sub-command (Use arrow keys)
❯ Send DApp address input to the application.
  Send ERC-20 deposit to the application.
  Send ERC-721 deposit to the application.
  Send ether deposit to the application.
  Send generic input to the application.
```

### Deploying your application on testnet

Create a file containing the network configuration testnet.env:

```shell
#!/bin/sh

export MNEMONIC=""

export NETWORK=
export RPC_URL=
export WSS_URL=
export CHAIN_ID=
```

Names of the `NETWORK`s can be found on the [WAGMI-CHAINS packages](https://wagmi.sh/core/chains). Prefer to use these names, as the framework will try to load RPC, ChainID and other information from the Wagmi package. If you can't find yours be sure to fill correctly all the necessary network information, it will work just fine.

Source it

```shell
source testnet.env
```

Download file:

```shell
wget https://raw.githubusercontent.com/lynoferraz/sunodo-deploy-wo/main/testnet-deploy.yml
```

Run the following commands

```shell
docker compose -f testnet-deploy.yml run authority-deployer
docker compose -f testnet-deploy.yml run dapp-deployer
```

Create a .sunodo.env file containing the cartesi node network configurations (change values of everithing in `[]`):

```shell
### contract-address-file
DAPP_CONTRACT_ADDRESS_FILE=/usr/share/sunodo/dapp-[NETWORK].json

### chain-id
CHAIN_ID=[CHAIN_ID]
TX_CHAIN_ID=[CHAIN_ID]

# dispatcher
## uses redis
REDIS_ENDPOINT=${REDIS_ENDPOINT:-redis://127.0.0.1:6379}

## uses chain-id (TX_CHAIN_ID actually)
TX_CHAIN_IS_LEGACY: "false"
TX_SIGNING_MNEMONIC=[MNEMONIC]
DAPP_DEPLOYMENT_FILE=/usr/share/sunodo/dapp-[NETWORK].json
ROLLUPS_DEPLOYMENT_FILE=/usr/share/sunodo/[NETWORK].json

SC_DEFAULT_CONFIRMATIONS=10
S6_CMD_WAIT_FOR_SERVICES_MAXTIME=${SM_DEADLINE_MACHINE:-30000}
TX_PROVIDER_HTTP_ENDPOINT=[RPC_URL]
TX_DEFAULT_CONFIRMATIONS=11

# state-server
SF_GENESIS_BLOCK=[DAPP_DEPLOY_BLOCK]
SF_SAFETY_MARGIN=20
BH_HTTP_ENDPOINT=[RPC_URL]
BH_WS_ENDPOINT=[WSS_URL]
BH_BLOCK_TIMEOUT=120

# advance-runner
PROVIDER_HTTP_ENDPOINT=[RPC_URL]
```

Check where sunodo is installed on your system. E.g. linux you can check with

```shell
npm list -g
```

And define the sunodo path. E.g.:

```shell
sunodo_path=~/.nvm/versions/node/v18.18.0/lib/node_modules/@sunodo/cli/dist
```

Since Sunodo services depend on each other, start `anvil` & `database`

```shell
SUNODO_BIN_PATH=$sunodo_path docker compose -f $sunodo_path/node/docker-compose-validator.yaml -f $sunodo_path/node/docker-compose-database.yaml -f $sunodo_path/node/docker-compose-explorer.yaml -f $sunodo_path/node/docker-compose-anvil.yaml -f $sunodo_path/node/docker-compose-proxy.yaml -f $sunodo_path/node/docker-compose-prompt.yaml -f $sunodo_path/node/docker-compose-snapshot-volume.yaml -f $sunodo_path/node/docker-compose-envfile.yaml --project-directory . up -d anvil database
```

Get the name of the anvil container to send the deployment files to the volume:

```shell
container=$(SUNODO_BIN_PATH=$sunodo_path docker compose -f $sunodo_path/node/docker-compose-validator.yaml -f $sunodo_path/node/docker-compose-anvil.yaml -f $sunodo_path/node/docker-compose-proxy.yaml -f $sunodo_path/node/docker-compose-prompt.yaml -f $sunodo_path/node/docker-compose-snapshot-volume.yaml -f $sunodo_path/node/docker-compose-envfile.yaml --project-directory . ps | grep anvil | awk '{print $1}')
```

Then copy the rollups.json file generated on a previous command the volume, so the validator node has the correct addresses. In Linux the volume path should be something like `/var/lib/docker/volumes/${dapp}_blockchain-data/_data` (it should be run by superuser):

```shell
docker cp .deployments/$NETWORK/rollups.json ${container}:/usr/share/sunodo/$NETWORK.json
docker cp .deployments/$NETWORK/dapp.json ${container}:/usr/share/sunodo/dapp-$NETWORK.json
```

Then start the validator and prompt:

```shell
SUNODO_BIN_PATH=$sunodo_path docker compose -f $sunodo_path/node/docker-compose-validator.yaml -f $sunodo_path/node/docker-compose-anvil.yaml -f $sunodo_path/node/docker-compose-proxy.yaml -f $sunodo_path/node/docker-compose-prompt.yaml -f $sunodo_path/node/docker-compose-snapshot-volume.yaml -f $sunodo_path/node/docker-compose-envfile.yaml --project-directory . up --attach prompt --attach validator
```

Check the running services with:

```shell
SUNODO_BIN_PATH=$sunodo_path docker compose -f $sunodo_path/node/docker-compose-validator.yaml -f $sunodo_path/node/docker-compose-anvil.yaml -f $sunodo_path/node/docker-compose-proxy.yaml -f $sunodo_path/node/docker-compose-prompt.yaml -f $sunodo_path/node/docker-compose-snapshot-volume.yaml -f $sunodo_path/node/docker-compose-envfile.yaml --project-directory . ps
```

Then to stop all services run:

```shell
SUNODO_BIN_PATH=$sunodo_path docker compose -f $sunodo_path/node/docker-compose-validator.yaml -f $sunodo_path/node/docker-compose-anvil.yaml -f $sunodo_path/node/docker-compose-proxy.yaml -f $sunodo_path/node/docker-compose-prompt.yaml -f $sunodo_path/node/docker-compose-snapshot-volume.yaml -f $sunodo_path/node/docker-compose-envfile.yaml --project-directory . down -v
```

