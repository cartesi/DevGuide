
## Table of Contents

1. [Introduction](#introduction)
2. [Setting up Your Development Environment](#setting-up-your-development-environment)
3. [Build your backend](#build-your-backend)  
   1. [Python Tutorial](#python-tutorial)
   2. [JavaScript Tutorial](#javascript-tutorial)


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

     ```/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"```

2. Install Python 3 using the following command:

     ```brew install python@3``` 

### Installing Node.js/NPM

1. If you have Homebrew installed, open a terminal and run:

    ```brew install node```

### Installing Yarn

1. If you have Homebrew installed, open a terminal and run:

    ```brew install yarn``` 


## Build your backend

This section contains 2 project examples in Python and JavaScript

### Python Tutorial 

#### Step 1: Creating your backend 

1. Launch Docker Desktop to start the engine and then run this command to check if your Docker supports the RISCV platform:

   ```docker buildx ls```

   If you do not see linux/riscv64 in the platforms list, install QEMU which will be used by Docker to emulate RISC-V instructions to build a Cartesi Machine with the command below: 

   ```sudo apt install qemu-user-static```

   After installing QEMU, the platform `linux/riscv64` should appear in the platforms list. Docker now supports the RISCV platform

2. Fork and Clone the [rollups-examples](https://github.com/cartesi/rollups-examples) repository for a sample project 


3. Change the directory 

   `cd echo-python`

    The backend code can be found in the `calculator.py` file. The backend receives mathematical expressions as inputs and generates notices with the result of the corresponding evaluations. 


#### Step 2: Run the environment locally (Host Mode)

1. Make sure you still have the Docker engine up and running

2. Build the Docker images from the pre-configured Docker configuration for each template by running the command (in the `calculator` directory): 

   ```docker buildx bake --load```


3. Start your local node from the images you just built by running:

   ```docker compose -f ../docker-compose.yml -f ./docker-compose.override.yml -f ../docker-compose-host.yml up```


#### Step 3: Run the backend
Next, we will run the application's backend inside the running local node. 

Inside the root directory of `calculator` folder, enter the following commands to run the backend:

```
python3 -m venv .venv
. .venv/bin/activate
pip install -r requirements.txt
ROLLUP_HTTP_SERVER_URL="http://127.0.0.1:5004" python3 echo.py
```

You can use also "entr" to automatically restart the backend whenever it detects changes:

```ls *.py | ROLLUP_HTTP_SERVER_URL="http://127.0.0.1:5004" entr -r python3 echo.py```


#### Step 4: Interacting with the backend(Frontend Interaction)

Now we can send input to our backend (a transaction) via the frontend. You can build your custom frontend or use the example [frontend console](https://github.com/cartesi/rollups-examples/tree/main/frontend-console). 

In the rollups-examples repo, we cloned earlier.  

1. Open a new terminal and change the directory to frontend-console:

   ```cd frontend-console```


2. Install and build the necessary dependencies by running:

   ```
    yarn
    yarn build
   ```

3. Send your input/transaction to the running node by running:


   ```yarn start input send --payload "3+5"```



4. To check these notices, run the following command:

   ```yarn start notice list```


Congratulations! You just built and ran your first dApp on Cartesi Rollups! 


#### Step 5: Shutting down the local node

To shut down the node and eliminate all active Docker containers, you can follow these steps

1. Terminate the environment by using the shortcut CTRL + C (or Command + C on macOS).

2. Run the command below in your dApp directory:

```docker compose -f ../docker-compose.yml -f ./docker-compose.override.yml down -v```

