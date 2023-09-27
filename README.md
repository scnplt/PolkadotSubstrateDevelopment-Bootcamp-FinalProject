# Polkadot Substrate Development Bootcamp

 Welcome to the **Polkadot Substrate Development Bootcamp Final Project.** In this project, the following requirements have been realized and explained supported by screen shots.

 - Building a blockchain
 - Simulating a substrate network
 - Adding trusted nodes to a network
 - Smart contracts

## Table of Contents

- [1. Getting Started](#1-getting-started)
  - [1.1. Prerequisites](#11-prerequisites) 
  - [1.2. Installation](#12-installation) 
- [2. Building a blockchain](#2-building-a-blockchain)
  - [2.1. Node](#21-node) 
  - [2.2. Frontend](#22-frontend) 
  - [2.3. Stop the node and front-end template](#23-stop-the-node-and-front-end-template) 
- [3. Simulating a substrate network](#3-simulating-a-substrate-network)
- [4. Adding trusted nodes to a network](#4-adding-trusted-nodes-to-a-network)
  - [4.1. Generate the accounts and keys](#41-generate-the-accounts-and-keys)
  - [4.2. Create a custom chain specification](#42-create-a-custom-chain-specification)
  - [4.3. Convert the chain specification to raw format](#43-convert-the-chain-specification-to-raw-format)
  - [4.4. Start the first node](#44-start-the-first-node)
  - [4.5. Start the second node](#45-start-the-second-node)

## 1. Getting Started

The project is built on the following system and application versions.

- **OS:** Ubuntu [Windows 10] x86_64
- **Kernel:** 5.15.90.1-microsoft-standard-WSL2
- **rust:** 1.72.0
- **node:** v18.17.1
- **yarn:** 1.22.19

### 1.1. Prerequisites

- Rust: [Installation](https://www.rust-lang.org/learn/get-started)
- Node.js: [Installation](https://nodejs.org/en/download)
- Yarn: [Installation](https://classic.yarnpkg.com/en/docs/install)

### 1.2. Installation

a. Add the `nightly` release and the `nightly` WebAssembly (wasm) targets to your development environment:

```bash
rustup default stable
rustup update nightly
rustup target add wasm32-unknown-unknown --toolchain nightly
```

b. Clone the node template repository and navigate to the root:

```bash
git clone https://github.com/substrate-developer-hub/substrate-node-template
cd substrate-node-template/
```

c. Create a new branch for save the changes:

```bash
git switch -c bootcamp
```

d. Compile the node template:

```bash
cargo build --release
```

## 2. Building a blockchain

### 2.1. Node

a. Start the node in development node:

```bash
./target/release/node-template --dev
```

Console output should look like the picture  

<img src="images/output-start-node-dev.png" width="750" alt="Console output: Start the node in development mode"/> 

### 2.2. Frontend

a. In new terminal, clone the front-end template and navigate to the root:

```bash
git clone https://github.com/substrate-developer-hub/substrate-front-end-template
cd substrate-front-end-template
```

b. Install the dependencies:

```bash
yarn install
```

c. Start the front-end template:

```bash
yarn start
```

Go to the http://localhost:8000/substrate-front-end-template.  
The site should look like the image

<img src="images\output-front-end-start.png" width="750" alt="localhost:8000"/> 

### 2.3. Stop the node and front-end template

1. Return to the terminal shell where the node output is displayed.
2. Press Control-C to terminate the running process.
3. Return to the terminal shell where the yarn output is displayed then repeat the **step 2**.

## 3. Simulating a substrate network

a. Purge old chain data:

```bash
# Working directory is substrate-node-template.
# After running this command, press 'y' to confirm.
./target/release/node-template purge-chain --base-path /tmp/bob --chain local
```

b. Start the local blockchain node using `bob` account:

```bash
./target/release/node-template \
--base-path /tmp/bob \
--chain local \
--bob \
--port 30333 \
--rpc-port 9945 \
--node-key 0000000000000000000000000000000000000000000000000000000000000001 \
--telemetry-url "wss://telemetry.polkadot.io/submit/ 0" \
--validator
```

Console output should look like the picture  

<img src="images/output-start-node-bob.png" width="750" alt="Console output: Start the node (Bob)"/>  

c. Add a second node to the network:

```bash
# Open new terminal and change the directory to substrate-node-template
# Purge old chain data
./target/release/node-template purge-chain --base-path /tmp/alice --chain local -y
# Start second node
./target/release/node-template \
--base-path /tmp/alice \
--chain local \
--alice \
--port 30334 \
--rpc-port 9946 \
--telemetry-url "wss://telemetry.polkadot.io/submit/ 0" \
--validator \
--bootnodes /ip4/127.0.0.1/tcp/30333/p2p/12D3KooWEyoppNCUx8Yx66oV9fJnriXwCcXwDDUA2kj6vnc6iDEp
```

Console output should look like the picture  

<img src="images/output-start-node-alice.png" width="750" alt="Console output: Start the node (Alice)"/> 

Go to the https://polkadot.js.org/apps/?rpc=ws%3A%2F%2F127.0.0.1%3A9946#/explorer  
This page should look like the picture

<img src="images/polkadot-js-after-alice-node.png" width="750" alt="Polkadot/Substrate Portal (Alice)"/> 

d. Stop the nodes and remove the old chain data:

```bash
# Before running the following command, 
# open the terminal where you started the Alice node and press Control+c
./target/release/node-template purge-chain --base-path /tmp/alice --chain local -y

# After previous command,
# open the other terminal (where you started the Bob node) and press Control+c
# then running the following command
./target/release/node-template purge-chain --base-path /tmp/bob --chain local -y
```

## 4. Adding trusted nodes to a network

### 4.1. Generate the accounts and keys

a. Generate a random secret pharse and keys:

```bash
# Change working directory to substrate-node-template
# After running this command, type a password and press enter
./target/release/node-template key generate --scheme Sr25519 --password-interactive
```

> Note the values that will be displayed in the console output. We will use the following public key for producing blocks using `aura` (authority round) for one node.

```bash
Secret phrase:       solution water resemble right twenty plastic team page photo say skill someone
  Network ID:        substrate
  Secret seed:       0xbf5d639e3dd4b7acf882577b4266a7baffb2247b0b7d589e1c92d1e2df5facbd
  Public key (hex):  0x9e7db40b6e91d411ad33e04a7b02b2bd55773f3afe5d0a60013e82dc0de94f32
  Account ID:        0x9e7db40b6e91d411ad33e04a7b02b2bd55773f3afe5d0a60013e82dc0de94f32
  Public key (SS58): 5FeWmJSsgoSCosn72i2Ygaf7bQtMpDWCaXUNYAEyo8t3Q7XL
  SS58 Address:      5FeWmJSsgoSCosn72i2Ygaf7bQtMpDWCaXUNYAEyo8t3Q7XL
```

b. Use the `secret phrase` value to derive keys using the Ed25519 signature scheme:

```bash
# After running this command, type the password and press enter
./target/release/node-template key inspect --password-interactive --scheme Ed25519 "solution water resemble right twenty plastic team page photo say skill someone"
```

> Note the values that will be displayed in the console output. We will use the following public key for finalizing blocks using `grandpa` for one node.

```bash
Secret phrase:       solution water resemble right twenty plastic team page photo say skill someone
  Network ID:        substrate
  Secret seed:       0xbf5d639e3dd4b7acf882577b4266a7baffb2247b0b7d589e1c92d1e2df5facbd
  Public key (hex):  0x164281c6966ab14e15f4613ff96823f1a633716b67f1dce7f32530a7bc8ca454
  Account ID:        0x164281c6966ab14e15f4613ff96823f1a633716b67f1dce7f32530a7bc8ca454
  Public key (SS58): 5CZtff5zSN6ahmGKhkut8Vxb7YTVZQNooG7jUGfvbWboE243
  SS58 Address:      5CZtff5zSN6ahmGKhkut8Vxb7YTVZQNooG7jUGfvbWboE243
```

> First set of keys:
> - Sr25519 (`aura`): `5FeWmJSsgoSCosn72i2Ygaf7bQtMpDWCaXUNYAEyo8t3Q7XL`
> - Ed25519 (`grandpa`): `5CZtff5zSN6ahmGKhkut8Vxb7YTVZQNooG7jUGfvbWboE243`

c. Repeat the step 1 and step 2 using a different identity on your local computer to generate a second set of keys:

> In this example, the second set of keys obtained after the steps using the different identity:
> - Sr25519 (`aura`): `5EWRQAJje3StoF6pvTQfYidiXNvCYqL48ZspBuumWLWcvqy8`
> - Ed25519 (`grandpa`): `5CMYv7pPtbwrJvjzj4AbRJHtWR46VABcdJcs8rtdePz9erno`

### 4.2. Create a custom chain specification

a. Export the local chain specification to a file named `customSpec.json`:

```bash
# Change working directory to substrate-node-template
./target/release/node-template build-spec --disable-default-bootnode --chain local > customSpec.json
```

b. Update the `customSpec.json`

b.1. Update `name` field if you want.

```json
"name": "Bootcamp Testnet"
```

b.2. Modify `aura` field, add the Sr25519 keys for each node.

```json
"aura": {
  "authorities": [
    "5FeWmJSsgoSCosn72i2Ygaf7bQtMpDWCaXUNYAEyo8t3Q7XL",
    "5EWRQAJje3StoF6pvTQfYidiXNvCYqL48ZspBuumWLWcvqy8"
  ]
}
```

b.3. Modify `grandpa` field, add the Ed25519 keys for each node.

```json
"grandpa": {
  "authorities": [
    [
      "5CZtff5zSN6ahmGKhkut8Vxb7YTVZQNooG7jUGfvbWboE243",
      1
    ],
    [
      "5CMYv7pPtbwrJvjzj4AbRJHtWR46VABcdJcs8rtdePz9erno",
      1
    ]
  ]
}
```

### 4.3. Convert the chain specification to raw format

Convert the `chainSpec.json` chain specification to the raw format:

```bash
# Change working directory to substrate-node-template
./target/release/node-template build-spec --chain=customSpec.json --raw --disable-default-bootnode > customSpecRaw.json
```

### 4.4. Start the first node

a. Start the node:

```bash
# After running this command, type the password and press enter
./target/release/node-template \
--base-path /tmp/node1 \
--chain ./customSpecRaw.json \
--port 30333 \
--rpc-port 9945 \
--telemetry-url "wss://telemetry.polkadot.io/submit/ 0" \
--validator \
--rpc-methods Unsafe \
--name Node1 \
--password-interactive
```

b. Insert the `aura` secret key generated from the `key` subcommand:

```bash
# Open new terminal and change working directory to substrate-node-template
# After running this command, type the password and press enter
./target/release/node-template key insert --base-path /tmp/node1 \
--chain customSpecRaw.json \
--scheme Sr25519 \
--suri "solution water resemble right twenty plastic team page photo say skill someone" \
--password-interactive \
--key-type aura
```

c. Insert the `grandpa` secret key generated from the `key` subcommand:

```bash
# After running this command, type the password and press enter
./target/release/node-template key insert \
--base-path /tmp/node1 \
--chain customSpecRaw.json \
--scheme Ed25519 \
--suri "solution water resemble right twenty plastic team page photo say skill someone" \
--password-interactive \
--key-type gran
```

d. Verify that your keys are in the keystore for `node1`:

```bash
ls /tmp/node1/chains/local_testnet/keystore
```

Console output should look like the picture  

<img src="images/output-ls-keystore-node1.png" width="750" alt="Console output: ls /tmp/node1/chains/local_testnet/keystore"/></br>

e. After you have added your keys to the keystore for the first node under `/tmp/node1`, you can restart the node (step a.)

### 4.5. Start the second node

a. Start the node:

```bash
# Open new terminal on different identity and change working directory to substrate-node-template
# After running this command, type the password and press enter
./target/release/node-template \
--base-path /tmp/node2 \
--chain ./customSpecRaw.json \
--port 30334 \
--rpc-port 9946 \
--telemetry-url "wss://telemetry.polkadot.io/submit/ 0" \
--validator \
--rpc-methods Unsafe \
--name MyNode2 \
--bootnodes /ip4/127.0.0.1/tcp/30333/p2p/12D3KooWKbqfV1kAugLgdye5fhU1wCeTiAymhc33MqAtBCgDHMgP \
--password-interactive
```

b. Insert the `aura` secret key generated from the `key` subcommand:

```bash
# Open new terminal and change working directory to substrate-node-template
# After running this command, type the password and press enter
./target/release/node-template key insert --base-path /tmp/node2 \
--chain customSpecRaw.json \
--scheme Sr25519 \
--suri "fever announce derive soap breeze master basket brand hood tenant lake what" \
--password-interactive \
--key-type aura
```

c. Insert the `grandpa` secret key generated from the `key` subcommand:

```bash
# After running this command, type the password and press enter
./target/release/node-template key insert \
--base-path /tmp/node2 \
--chain customSpecRaw.json \
--scheme Ed25519 \
--suri "fever announce derive soap breeze master basket brand hood tenant lake what" \
--password-interactive \
--key-type gran
```

d. Verify that your keys are in the keystore for `node2`:

```bash
ls /tmp/node2/chains/local_testnet/keystore
```

Console output should look like the picture  

<img src="images/output-ls-keystore-node2.png" width="750" alt="Console output: ls /tmp/node2/chains/local_testnet/keystore"/></br>

e. After you have added your keys to the keystore for the second node under `/tmp/node2`, you can restart the node (step a.)

f. You should see that each node has one peer, same genesis block and state root hashes like the picture.

<img src="images/polkadot-js-after-trusted-nodes.png" width="750" alt="Adding trusted nodes to a network" title="Node1"/></br>
<img src="images/polkadot-js-after-trusted-nodes-same-genesis-hash-state-node1.png" width="750" alt="Node1" title="Node1"/></br>
<img src="images/polkadot-js-after-trusted-nodes-same-genesis-hash-state-node2.png" width="750" alt="Node2" title="Node2"/></br>
