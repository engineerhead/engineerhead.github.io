---
title:  "CosmWasm: A Rust Framework Tutorial/Course 04"
---
![](/assets/images/macros-rust.png)
In the [previous chapter](http://engineerhead.github.io/2023/09/04/cosmwasm-rust-framework-tutorial-course-03.html), we touched Rust's basics with a traditional "Hello World" program. Let's write a basic "Hello World" smart contract in CosmWasm. Before writing the smart contract, we have to setup our device. Run the following in terminal to install Wasm rust compiler backend. This will enable us to generate Wasm binaries.

    ~ rustup target add wasm32-unknown-unknown

Then cargo-generate and cargo-run-script need to be installed. So that smart contract template provided by CosmWasm team can be used.

    cargo install cargo-generate --features vendored-openssl
	cargo install cargo-run-script

CosmWasm team provides us with a template to structure our smart contract in a predefined manner. In your working directory, run the following command. 

    cargo generate --git https://github.com/CosmWasm/cw-template.git --name hello-world -d minimal=true

Following directories and files have been generated for us.

    ~tree hello-world
    hello-world
	├── Cargo.toml
	├── LICENSE
	├── NOTICE
	├── README.md
	└── src
	    ├── bin
	    │   └── schema.rs
	    ├── contract.rs
	    ├── error.rs
	    ├── helpers.rs
	    ├── lib.rs
	    ├── msg.rs
	    └── state.rs

Let's discuss the boilerplate generated for us.

    #[cfg(not(feature = "library"))]
    use cosmwasm_std::entry_point;
    use cosmwasm_std::{Binary, Deps, DepsMut, Env, MessageInfo, Response, StdResult};
    // use cw2::set_contract_version;
    
    use crate::error::ContractError;
    use crate::msg::{ExecuteMsg, InstantiateMsg, QueryMsg};
    
    /*
    // version info for migration info
    const CONTRACT_NAME: &str = "crates.io:hello-world";
    const CONTRACT_VERSION: &str = env!("CARGO_PKG_VERSION");
    */
    
    #[cfg_attr(not(feature = "library"), entry_point)]
    pub fn instantiate(
        _deps: DepsMut,
        _env: Env,
        _info: MessageInfo,
        _msg: InstantiateMsg,
    ) -> Result<Response, ContractError> {
        unimplemented!()
    }
    
    #[cfg_attr(not(feature = "library"), entry_point)]
    pub fn execute(
        _deps: DepsMut,
        _env: Env,
        _info: MessageInfo,
        _msg: ExecuteMsg,
    ) -> Result<Response, ContractError> {
        unimplemented!()
    }
    
    #[cfg_attr(not(feature = "library"), entry_point)]
    pub fn query(_deps: Deps, _env: Env, _msg: QueryMsg) -> StdResult<Binary> {
        unimplemented!()
    }
    
    #[cfg(test)]
    mod tests {}

This is a large piece of code. Let's dissect it in pieces

    #[cfg(not(feature = "library"))]
    use cosmwasm_std::entry_point;
    use cosmwasm_std::{Binary, Deps, DepsMut, Env, MessageInfo, Response, StdResult};

`#[cfg(...)]` is called an attribute in Rust and is used for conditional compilation. `cfg` stands for **configuration** and allows us to include or exclude code sections during compilation based on certain conditions. In the above case, the condition is `not(feature="library")` which checks the feature named library is not enabled.

In Rust, features are usually defined in **Cargo.toml** file of the project. If the `library` feature is not enabled in project's configuration, the code annotated by `#[cfg(not(feature = "library"))]` will be compiled. On the contrary, if `library` feature is enabled, the specific code section won't be compiled.

After the `cfg` attribute, following lines import some traits, macros and data structures from `cosmwas_std` crate.  

    use cosmwasm_std::entry_point;
    use cosmwasm_std::{Binary, Deps, DepsMut, Env, MessageInfo, Response, StdResult};

Rust employs the `use` keyword to bring external items, such as modules, functions, types, and traits, into scope, making them available for use in the current code file. Let's discuss `cosmwasm_std::entry_point`.`entrypoint` is an attribute macro that generates the boilerplate necessary to call into contract specific logic from entry points to Wasm modules. **macros** in Rust allow developers to define custom syntax extensions. `attribute macros` are distinguished from function-like macros (procedural macros) because attribute macros are applied to items, such as functions, structs, enums, or modules, and they modify or annotate those items with additional information or functionality.

The above mentioned imports are only available in the contract if `library` feature is not enabled in project's `Cargo.toml`.



