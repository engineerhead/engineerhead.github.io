---
title:  "CosmWasm: A Rust Framework Tutorial/Course 01"
---
![](/assets/images/cosmwasm-logo.jpeg)
The blockchain space has progressed tremendously over the years. One of the most significant developments has been the ability to build decentralised applications upon a blockchain. Ethereum was pioneer regarding dApps but other blockchain platforms have provided smart contract abilities in their own manner. Cosmos ecosystem has stood out by offering CosmWasm platform which promises to provide ability to build dApps in a secure and performant way.

Before we dive into details of CosmWasm, let's take a brief look at Cosmos. Cosmos, often described as "Internet of Blockchains" is a network of interconnected blockchains that can communicate with each other effortlessly.

CosmWasm, short for "Cosmos Web assembly", is a smart contract platform specifically designed for Cosmos ecosystem. It brings the power of Web Assembly to Cosmos chains and eventually allowing the developers to build smart contracts for any chain in the ecosystem using same framework.

## What is Web Assembly?
WebAssembly or simply Wasm is a binary instruction format that is designed to be fast and secure. It is a technology that enables the execution of code at near native speed on Web and other platforms. It is designed to be portable target for the compilation of high-level programming languages like Rust, C, C++,  and others. Simply, developers compile their source code to Wasm byte code using tools like Rust's native support for Wasm or Emscripten. The resulting Wasm module can be execute in the web browsers or runtimes that support Web Assembly.

## CosmWasm's Features
Security: It is a paramount concern when developing smart contracts for any chain as usually money is involved. CosmWasm provides robust security by relying on Wasm's security features and then making the smart contract execute in an isolated environment. 

Efficiency: By relying on Wasm's efficiency,  the smart contracts built using CosmWasm consume minimal resources and run smoothly. This efficiency helps developers in scaling the blockchain in a performant way.

Modularity: The well thought out modular architecture of CosmWasm allows developers to build complex dApps by combining different smart contracts.

Interoperability: CosmWasm's compatibility with multiple Cosmos chains enables developers to build dApps using same code.