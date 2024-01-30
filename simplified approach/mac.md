
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

 > Sunodo is the recommended tool for building dApps on Cartesi. However, you may choose to skip this if you prefer to use the more primitive approach.

1. You can install Sunodo with Homebrew by running this command:

   ```
    brew install sunodo/tap/sunodo
   ```

2. Alternatively, you can install Sunodo with Node.js by running:

   ```
    npm install -g @sunodo/cli
   ```


### Check for RISC-V support

1. Launch Docker Desktop to start the engine and then run this command to check if your Docker supports the RISCV platform:

   ```
   docker buildx ls | grep linux/riscv64
   ```

   If you do not see linux/riscv64 in the platforms list, install QEMU which will be used by Docker to emulate RISC-V instructions to build a Cartesi Machine with the command below: 

   ```
   sudo apt install qemu-user-static
   ```

   After installing QEMU, the platform `linux/riscv64` should appear in the platforms list. Docker now supports the RISCV platform


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

### Deploying your application

Deplopyment options with Sunodo is under development and it is expected to be made available to the public soon.
