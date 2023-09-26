# Polkadot Substrate Development Bootcamp - Final Project

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
  - [2.3 Stop the node and front-end template](#23-stop-the-node-and-front-end-template) 

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

a. Add the `nightly` release and the `nightly` WebAssembly (wasm) targets to your development environment by running the following commands:

```bash
rustup default stable
rustup update nightly
rustup target add wasm32-unknown-unknown --toolchain nightly
```

b. Clone the node template repository and navigate to the root

```bash
git clone https://github.com/substrate-developer-hub/substrate-node-template
cd substrate-node-template/
```

c. Create a new branch for save the changes

```bash
git switch -c bootcamp
```

d. Compile the node template

```bash
cargo build --release
```

## 2. Building a blockchain

### 2.1. Node

a. Start the node in development node

```bash
./target/release/node-template --dev
```

Console output should look like the picture  

<img src="images/output-start-node-dev.png" width="750" alt="Console output: Start the node in development mode"/> 

### 2.2. Frontend

a. In new terminal, clone the front-end template and navigate to the root

```bash
git clone https://github.com/substrate-developer-hub/substrate-front-end-template
cd substrate-front-end-template
```

b. Install the dependencies

```bash
yarn install
```

c. Start the front-end template

```bash
yarn start
```

Go to the http://localhost:8000/substrate-front-end-template.  
The site should look like the image

<img src="images\output-front-end-start.png" width="750" alt="localhost:8000"/> 

### 2.3 Stop the node and front-end template

1. Return to the terminal shell where the node output is displayed.
2. Press Control-C to terminate the running process.
3. Return to the terminal shell where the yarn output is displayed then repeat the **step 2**.
