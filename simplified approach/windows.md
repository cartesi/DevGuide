
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

 > Sunodo is the recommended tool for building dApps on Cartesi. However, you may choose to skip this if you prefer to use the more primitive approach.

1. Install WSL2 and the Ubuntu distro from Microsoft Store and install Sunodo with:

    ```
    npm install -g @sunodo/cli
    ```


### Check for RISC-V support

1. In the Windows terminal, launch Docker Desktop to start the engine and then run this command to check if your Docker supports the RISCV platform:

   ```
   docker buildx ls
   ```

   If you installed docker Desktop correctly, you will see the linux/riscv64 in the plataforms list. But if you do not see it there, install QEMU which will be used by Docker to emulate RISC-V instructions to build a Cartesi Machine from this [Link](https://www.qemu.org/download/)

   After installing QEMU, the platform `linux/riscv64` should appear in the platforms list. Docker now supports the RISCV platform

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
