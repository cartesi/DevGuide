
## Table of Contents

1. [Introduction](#introduction)
2. [Setting up Your Development Environment](#setting-up-your--development-environment)
   1. [Installing & Configuring Docker](#installing--configuring-docker)
   2. [Installing Python](#installing-python)
   3. [Installing Node.js/NPM](#installing-nodejsnpm)
   4. [Installing Yarn](#installing-yarn)
   5. [Check for RISC-V support](#check-for-risc-v-support)
3. [Build your backend | Python Tutorial](#build-your-backend--python-tutorial)
   1. [Step 1: Creating your backend](#step-1-creating-your-backend)
   2. [Step 2: Run the environment locally (Host Mode)](#step-2-run-the-environment-locally-host-mode)
   3. [Step 3: Run the backend](#step-3-run-the-backend)
   4. [Step 4: Interacting with the backend(Frontend Interaction)](#step-4-interacting-with-the-backendfrontend-interaction)
   5. [Step 5: Shutting down the local node](#step-5-shutting-down-the-local-node)
4. [Build your backend | JavaScript Tutorial](#build-your-backend--javascript-tutorial)
   1. [Step 1: Creating your backend](#step-1-creating-your-backend-1)
   2. [Step 2: Run the environment locally (Host Mode)](#step-2-run-the-environment-locally-host-mode-1)
   3. [Step 3: Run the backend](#step-3-run-the-backend-1)
   4. [Step 4: Interacting with the backend(Frontend Interaction)](#step-4-interacting-with-the-backendfrontend-interaction-1)
   5. [Step 5: Shutting down the local node](#step-5-shutting-down-the-local-node-1)
5. [Deploying your backend](#deploying-your-backend)
   1. [Step 1: Build your machine to deploy](#step-1-build-your-machine-to-deploy)
   2. [Step 2: Deploying your app's backend](#step-2-deploying-your-apps-backend)
   3. [Step 3: Run a validator node](#step-3-run-a-validator-node)
6. [Client Interaction with Deployed dApps](#client-interaction-with-deployed-dapps)
   1. [Sending data](#sending-data)
   2. [Sending inputs](#sending-inputs)
   3. [Deposit tokens](#deposit-tokens)

## Introduction

This guide provides detailed instructions tailored for macOS users

## Setting up Your  Development Environment

Here are the general requirements:
- Python3
- Node.js
- Yarn
- Docker Desktop

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


### Check for RISC-V support

1. Launch Docker Desktop to start the engine and then run this command to check if your Docker supports the RISCV platform:

   ```
   docker buildx ls
   ```

   If you do not see linux/riscv64 in the platforms list, install QEMU which will be used by Docker to emulate RISC-V instructions to build a Cartesi Machine with the command below: 

   ```
   sudo apt install qemu-user-static
   ```

   After installing QEMU, the platform `linux/riscv64` should appear in the platforms list. Docker now supports the RISCV platform

## Build your backend | Python Tutorial

This section is the Python tutorial for the dApp

### Step 1: Creating your backend 

1. Fork and Clone the [rollups-examples](https://github.com/cartesi/rollups-examples) repository for sample project examples 


2. Change the directory 

   `
   cd calculator
   `

    The backend code can be found in the `calculator.py` file. The backend receives mathematical expressions as inputs and generates notices with the result of the corresponding evaluations. 


### Step 2: Run the environment locally (Host Mode)

1. Make sure you still have the Docker engine up and running

2. Build the Docker images from the pre-configured Docker configuration for each template by running the command (in the `calculator` directory): 

   ```
   docker buildx bake --load
   ```

3. Start your local node from the images you just built by running:

   ```
   docker compose -f ../docker-compose.yml -f ./docker-compose.override.yml -f ../docker-compose-host.yml up
   ```

### Step 3: Run the backend

Next, we will run the application's backend inside the running local node. 

Inside the root directory of `calculator` folder, enter the following commands to run the backend:

```
python3 -m venv .venv
. .venv/bin/activate
pip install -r requirements.txt
ROLLUP_HTTP_SERVER_URL="http://127.0.0.1:5004" python3 calculator.py
```

You can use also "entr" to automatically restart the backend whenever it detects changes:

```
ls *.py | ROLLUP_HTTP_SERVER_URL="http://127.0.0.1:5004" entr -r python3 calculator.py
```


### Step 4: Interacting with the backend(Frontend Interaction)

Now we can send input to our backend (a transaction) via the frontend. You can build your custom frontend or use the example [frontend console](https://github.com/cartesi/rollups-examples/tree/main/frontend-console). 

In the rollups-examples repo, we cloned earlier.  

1. Open a new terminal and change the directory to frontend-console:

   ```
   cd frontend-console
   ```


2. Install and build the necessary dependencies by running:

   ```
    yarn
    yarn build
   ```

3. Send your input/transaction to the running node by running:


   ```
   yarn start input send --payload "3+5"
   ```

4. To check these notices, run the following command:

   ```
   yarn start notice list
   ```

Congratulations! You just built and ran your first dApp on Cartesi Rollups! 


### Step 5: Shutting down the local node

To shut down the node and eliminate all active Docker containers, you can follow these steps

1. Terminate the environment.

2. Run the command below in your dApp directory:

   ```
   docker compose -f ../docker-compose.yml -f ./docker-compose.override.yml down -v
   ```


## Build your backend | JavaScript Tutorial

This section is the JavaScript tutorial for the dApp.


### Step 1: Creating your backend 

1. Fork and clone the [rollups-examples](https://github.com/cartesi/rollups-examples) repository for sample project examples 


2. Change the directory 

   `
   cd echo-js
   `

    The backend code can be found in the `dapp.js` file. The backend copies (or "echoes") each input received as a corresponding output notice. 


### Step 2: Run the environment locally (Host Mode)

1. Make sure you still have the Docker engine up and running

2. Build the Docker images from the pre-configured Docker configuration for each template by running the command (in the `echo-js` directory): 

   ```
   docker buildx bake --load
   ```


3. Start your local node from the images you just built by running:

   ```
   docker compose -f ../docker-compose.yml -f ./docker-compose.override.yml -f ../docker-compose-host.yml up
   ```


### Step 3: Run the backend
Next, we will run the application's backend inside the running local node. 

Inside the root directory of `echo-js` folder, enter the following commands to run the backend:

```
yarn
yarn start
```

You can use also "entr" to automatically restart the backend whenever it detects changes:

```
ls *.js | entr -r yarn start
```


### Step 4: Interacting with the backend(Frontend Interaction)

Now we can send input to our backend (a transaction) via the frontend. You can build your custom frontend or use the example [frontend console](https://github.com/cartesi/rollups-examples/tree/main/frontend-console). 

In the rollups-examples repo, we cloned earlier.  

1. Open a new terminal and change the directory to frontend-console:

   ```
   cd frontend-console
   ```


2. Install and build the necessary dependencies by running:

   ```
    yarn
    yarn build
   ```

3. Send your input/transaction to the running node by running:


   ```
   yarn start input send --payload "Hello World"
   ```

4. To check these notices, run the following command:

   ```
   yarn start notice list
   ```

Congratulations! You just built and ran your first dApp on Cartesi Rollups! 


### Step 5: Shutting down the local node

To shut down the node and eliminate all active Docker containers, you can follow these steps

1. Terminate the environment.

2. Run the command below in your backend directory:

   ```
   docker compose -f ../docker-compose.yml -f ./docker-compose.override.yml down -v
   ```

## Deploying your backend

Until now, we’ve been deploying on a localhost testnet chain and every transaction so far has been to contracts on our local chain.

Here are the supported public testing networks with the Cartesi Rollups infrastructure:

```
gnosis_chiado
arbitrum_goerli
optimism_goerli
polygon_mumbai
sepolia
```
### Step 1: Build your machine to deploy

You can specify which blockchain network you're building the back-end machine for by providing the network information. 

Build the back-end by running:

```
docker buildx bake machine --load --set *.args.NETWORK=<network>
```
Replace `<network>`  with your prefered network.


### Step 2: Deploying your app's backend

Great, we're all set for deployment with our Docker image. Here's what you need to do next:


1. Set your Metamask 12-word phrase as an environment variable:   
      ```
      export MNEMONIC=<your 12 words here>
      ```


2. Set your Sepolia RPC URL as an environment variable: 
     ```
	  export RPC_URL=<your RPC URL here>
	  ```


3. Deploy to the Cartesi dApp factory contract on the target network:
     ```
     DAPP_NAME=<example> docker compose --env-file ../env.<network> -f ../deploy-testnet.yml up
     ```

     Replace `<example>` with your project name and `<network>` with the target network(ie sepolia)

4. Once the deployment is successful, stop the docker containers and remove the volumes created by running:
    ```
    DAPP_NAME=<example> docker compose --env-file ../env.<network> -f ../deploy-testnet.yml down -v
    ```


5. After successful deployment, a file will be created at `../deployments/<network>/<example>.json` with the deployed contract's address. Alternatively you run the command below for the address to be printed in your console: 

   ```
   cat ../deployments/<network>/<example>.json
   ```

### Step 3: Run a validator node
Before setting up a Cartesi Validator Node, you should have already deployed your smart contract to a target blockchain network

1. Additional environment variables: You'll need a secure WebSocket endpoint for the RPC gateway (WSS URL). For a Sepolia RPC from [Alchemy](https://www.alchemy.com/), here is how you can set it:

   ```
   export WSS_URL=wss://eth-sepolia.g.alchemy.com/v2/****
   ```


2. Build a server manager for your target network: 
   ```
   docker buildx bake server --load --set *.args.NETWORK=<network>
   ```



3. Start the Validator Node

   ```
   DAPP_NAME=<example> docker compose --env-file ../env.<network> -f ../docker-compose-testnet.yml -f ./docker-compose.override.yml up
   ```



4. Alternative (HOST MODE)

   If you prefer to, you can run the validator node in host mode by executing this command:

    ```
    DAPP_NAME=<example> docker compose --env-file ../env.<network> -f ../docker-compose-testnet.yml -f ./docker-compose.override.yml -f ../docker-compose-host-testnet.yml up
    ```
    This process ensures your Cartesi Validator Node is up and running, ready to interact with your dApp's smart contract and handle the back-end logic effectively


## Client Interaction with Deployed dApps

### Sending data

When sending data, the frontend console application needs to communicate with the underlying layer-1 blockchain. 

Here is what you need to configure to make that happen:
- An RPC gateway
- The address of the Rollups smart contract on that blockchain 
- An account with sufficient funds.

A cheat sheet that can help you properly configure your requests for sending data:

``` 
--index         Notice or Voucher index within its associated Input
--input         Input index
--rpc           URL of the RPC gateway to use
--address       DApp Rollups contract address
--addressFile   Path to a file containing the DApp Rollups contract address
--dapp          DApp name
--mnemonic      Wallet mnemonic(12 words)
--accountIndex  Account index from mnemonic
--url           Reader URL
```

### Sending inputs

From your frontend console, you can send input to an instance of the calculator app/echo-python backend already deployed to Sepolia, using your private key and your RPC gateway. You can run these commands from the `rollups-examples/frontend-console` directory.

```
export MNEMONIC=your 12 words here
export RPC_URL=https://eth-sepolia.g.alchemy.com/v2/<USER_KEY>
yarn start input send --payload "my message" --dapp echo-python
```

For other contracts, you can simply specify the address of the recipient contract to carry out the transaction.  
```
yarn start input send --payload "my message" --address 0x70ac08179605AF2D9e75782b8DEcDD3c22aA4D0C
```


### Deposit tokens

Every deployed contract in the Cartesi rollups dApp factory can send and receive ERC-20 & ERC-721 tokens. 
```
Here is a cheat sheet of all the options available
--erc20         ERC-20 address
--rpc           URL of the RPC gateway to use
--address       DApp Rollups contract address
--addressFile   Path to a file containing the DApp Rollups contract address
--dapp          DApp name
--mnemonic      Wallet mnemonic
--accountIndex  Account index from mnemonic
--erc721        ERC-721 contract address
--tokenId       The ID of the ERC-721 token being transferred
```

*An example*:
From the frontend console, you can deposit some ERC-20 tokens from your wallet to a smart contract with this address 
`0x70ac08179605AF2D9e75782b8DEcDD3c22aA4D0C` deployed on Sepolia. 
```
export MNEMONIC=your 12 words here
export RPC_URL=https://eth-sepolia.g.alchemy.com/v2/****
yarn start erc20 deposit --amount 10000000000000000000 --address 0x70ac08179605AF2D9e75782b8DEcDD3c22aA4D0C
```

You can also deposit ERC-721 tokens from your wallet to the smart contract. 
```
export MNEMONIC=your 12 words here
export RPC_URL=https://eth-sepolia.g.alchemy.com/v2/****
yarn start erc721 deposit --tokenId 4 --address 0x70ac08179605AF2D9e75782b8DEcDD3c22aA4D0C
```

> This deposits the token with id 4 to this address 0x70ac08179605AF2D9e75782b8DEcDD3c22aA4D0C on Sepolia

*Here are several operations you can perform from the frontend console when you run `yarn start --help`*
```
yarn start --help

Commands:
  index.ts erc20 <command>    Operations with ERC-20 tokens
  index.ts erc721 <command>   Operations with ERC-721 tokens
  index.ts input <command>    Operations with inputs
  index.ts inspect            Inspect the state of the DApp
  index.ts notice <command>   Operations with notices
  index.ts report <command>   Operations with reports
  index.ts voucher <command>  Operations with vouchers

Options:
  --help     Show help                                                 [boolean]
  --version  Show version number                                       [boolean]
```
