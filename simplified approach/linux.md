## Introduction

This guide provides detailed instructions tailored for Linux users, using Sunodo.

## Setting up Your  Development Environment

Here are the general requirements:
- Python3
- Node.js
- Yarn
- Docker Desktop
- Sunodo

### Installing & Configuring Docker

1. Download Docker Desktop for Linux using the URL below 
- [Docker Desktop for Linux](https://docs.docker.com/desktop/install/ubuntu/)


### Installing Node.js/NPM

1. First install nvm by running:

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

```
sudo apt update
sudo apt install python3
```

### Installing Yarn

1. Open your terminal and add Yarn APT Repository

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
   docker buildx ls
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

change directory

```
cd dapp-name
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





