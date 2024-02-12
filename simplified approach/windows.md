
## Introduction

This guide provides detailed instructions tailored for Windows users using Sunodo.


## Setting up Your  Development Environment

> It's essential to have [Windows Subsystem for Linux 2 (WSL2)](https://learn.microsoft.com/en-us/windows/wsl/install) installed and correctly configured to enjoy a seamless dApp development experience with Cartesi Rollups. WSL2 provides a Linux-like environment that aligns with the Cartesi ecosystem.


Here are the general requirements:
- WSL2 and Ubuntu
- Python3
- Node.js
- Yarn
- Sunodo
- Docker Desktop

### Installing WSL2 and Ubuntu

1. Open Microsft Store and install WSL2

2. Open Microsoft Store and install the Ubuntu distro

### Installing & Configuring Docker

1. Download and run the Docker Desktop installer
- [Docker Desktop for Windows(Installer)](https://desktop.docker.com/win/main/amd64/Docker%20Desktop%20Installer.exe?_gl=1*q8ki63*_ga*MTA0MTIzOTI4LjE2OTcyNzA0OTY.*_ga_XJWPQMJYHQ*MTY5NzQ2ODc2Ny41LjEuMTY5NzQ3MDUyNi41OC4wLjA.)


### Installing Node.js/NPM

1. Open the Ubuntu terminal (installed earlier with WSL) 


2. Install nvm by running:

   ```
   curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
   ```


2. Activate nvm in your terminal

   ```
   source ~/.bashrc
   ```


3. Install the latest version of Node.js by running:

   ```
   nvm install node
   ```


4. Set the newly installed version as the default

   ```
   nvm alias default node
   ```

### Installing Python

1. In the Ubuntu terminal (installed earlier with WSL), run:

    ```
    sudo apt update
    sudo apt install python3
    ```

### Installing Yarn

1. In the Ubuntu terminal, add Yarn APT Repository by running:

   ```
   sudo apt update
   sudo apt install curl
   curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
   echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
   ```

2. Install Yarn

   ```
   sudo apt update
   sudo apt install yarn
   ```

### Installing Sunodo

1. Install WSL2 and the Ubuntu distro from Microsoft Store and install Sunodo with:

    ```
    npm install -g @sunodo/cli
    ```


### Important Note

To ensure optimal compatibility and performance, Docker Engine relies on WSL2 for its operation. Therefore, start WSL2 by launching Powershell and running the "wsl" command before launching the Docker Desktop. 

We will not run any more commands in the native Windows Powershell/terminal. Everything else will be done in the Ubuntu distro we installed. 

For a seamless development workflow, it is recommended not to execute Docker commands within Powershell or the WSL terminal. Instead, open the Ubuntu distribution that you have installed, and perform all coding and command execution within that Linux environment.


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

With your application running, you can send inputs by sending transactions with the input payload.


You can send inputs to your application using the sunodo send command, Cast, a command-line tool for making Ethereum RPC calls, or a  custom-built web interface.

#### 1. Using Sunodo

```shell
sunodo send
```
This guides you through sending inputs with Sunodo interactively.

```
? Select the send sub-command (Use arrow keys)
❯ Send DApp address input to the application.
  Send ERC-20 deposit to the application.
  Send ERC-721 deposit to the application.
  Send ether deposit to the application.
  Send generic input to the application.
```

#### 2. Using Cast

```shell
cast send 0xInputBoxAddress123 "addInput(address,bytes)" 0xDAppAddress456 0xEncodedPayload789 -mnemonic <MNEMONIC> 
``` 

This command sends an input payload to your application through the InputBox contract. 

> Replace placeholders like `0xInputBoxAddress123`, `0xDAppAddress456`, `0xEncodedPayload789`, and `<MNEMONIC>` with the actual addresses, payload, and mnemonic for your specific use case. If you are on the local anvil node, `0xDAppAddress456` is `0x70ac08179605AF2D9e75782b8DEcD3c22aA4D0C` and `0xInputBoxAddress123` is `0x59b22D57D4f067708AB0c00552767405926dc768`


Send “Hello World” which is hex-encoded into `0x48656c6c6f20776f726c64` to your application using Cast:

```shell
cast send 0x59b22D57D4f067708AB0c00552767405926dc768 "addInput(address,bytes)" 0x70ac08179605AF2D9e75782b8DEcDD3c22aA4D0C 0x48656c6c6f20776f726c64 --mnemonic 'test test test test test test test test test test test junk'
```


#### 3. Using a custom web interface
You can create a custom frontend that interacts with your application. 

Here is a [React.js + Typescript starter template](https://github.com/prototyp3-dev/frontend-web-cartesi) with all the major functionalities to build on Cartesi.

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

### Learn more

- [Sunodo Documentation](https://docs.sunodo.io)
- [Sunodo Starter Templates](https://github.com/cartesi/sunodo-examples)




