---
title:  "CosmWasm: A Rust Framework Tutorial/Course 05"
---
![](/assets/images/rust-lang.png)
As we continue [building our "Hello World" contract in CosmWasm](https://engineerhead.github.io/2023/09/05/cosmwasm-rust-framework-tutorial-course-04.html), we will discuss the following imports as their usage come up.

    use cosmwasm_std::{Binary, Deps, DepsMut, Env, MessageInfo, Response, StdResult};
	// use cw2::set_contract_version;

	use crate::error::ContractError;
	use crate::msg::{ExecuteMsg, InstantiateMsg, QueryMsg};

The next code portion to be examined is 

    #[cfg_attr(not(feature = "library"), entry_point)]
	pub fn instantiate(
	    _deps: DepsMut,
	    _env: Env,
	    _info: MessageInfo,
	    _msg: InstantiateMsg,
	) -> Result<Response, ContractError> {
	    unimplemented!()
	}

`cfg_attr`is a different version of `cfg` macro as it is used to apply multiple attributes conditionally to an item. In the above code, it conditionally applies `entry_point` attribute to a function when `library` feature is not enabled in crate's configuration. 

`instantiate` function takes multiple inputs and returns a single output `Result`which is an enum type. It represents the result of an operation that can either succeed (`Ok`) or fail (`Err`). In case of success, `Resposne` type is returned which typically contains the response of contract's execution. In typical cases, it would be success and failure messages, attributes, or some other data. In case of error, `ContractError` is returned. It is a custom Error type that provides information why some logic in contract failed.

As far as inputs are concerned!

 - `deps` represents the dependencies required by the contract. In
   CosmWasm, dependencies provide access to various blockchain-related
   functionality, such as reading and writing data to the contract's
   storage. `deps` of type `DepsMut` is usually used when the contract needs to modify the state of blockchain.
 - `env` input provides information about the execution environment of the contract. It contains details like the current block height, time, chain ID, and more.
 -  `info` of type `MessageInfo` contains information about the message that triggered the contract execution. This includes the sender's address, the funds attached to the message, and some other information.
 - `msg` of type `InstantiateMsg` is a custom message type that carries data specific to contract initialisation.

Last but not least `unimplemented!()` in function body is a macro placeholder that signifies that the `instantiate` function has not been implemented yet. Let's implement a basic contract by replacing the `unimplemented!()` macro with following code.

    Ok(
	    Response::new()
			    .add_message("message", "Hello, World")
    )

That's is it. We construct a new `Response` object by calling `new` method on it.  Then we chain the constructor call with an `add_message` method which takes a key and message as inputs. Key in this case is `message` while `message` is "Hello, World". In next chapter, we are going to write test this basic functionality.
