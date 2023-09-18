---
title:  "CosmWasm: A Rust Framework Tutorial/Course 07"
---
![](/assets/images/rust-lang.png)
Previously, a [simple](https://engineerhead.github.io/2023/09/05/cosmwasm-rust-framework-tutorial-course-04.html) [contract's functionality](https://engineerhead.github.io/2023/09/11/cosmwasm-rust-framework-tutorial-course-05.html) and [tests](https://engineerhead.github.io/2023/09/12/cosmwasm-rust-framework-tutorial-course-06.html) were written to get introduced with CosmWasm.  Before diving into somewhat complex operations, let's see how to handle contract instantiation for purposes like setting an admin for the contract. Setting an admin would enable us to upgrade the contract and safeguard access to some other operations. This can be written from scratch but CosmWasm team has written some utility contracts for such common cases. So! we are going to rely on them.

Run the following  command in root of our previously generated project.

    cargo add cw-controllers

Now at the top of `contract.rs`, let's import `Admin` struct like this

    use cw_controllers::Admin;

In Rust, a struct (short for "structure") is a composite data type that allows us to group together multiple values of different data types into a single unit. Let's construct an OWNER object after the import as follows.

    const OWNER: Admin = Admin::new("owner");

The above code declares a constant named `OWNER`. Constants in Rust are values that cannot be changed after they are assigned a value. They are always in uppercase by convention. `Admin::new("owner")` part of the code initialises the `OWNER` constant. It assigns an instance of the `Admin` type to `OWNER`. It can be deduced that `Admin` type has a constructor called `new` that takes a single argument, which is a string `"owner"` in this case.

After declaring and constructing `OWNER`, an address has to be assigned to it which happens in the `instantiate` function.

    #[cfg_attr(not(feature = "library"), entry_point)]
	pub fn instantiate(
	    deps: DepsMut,
	    _env: Env,
	    info: MessageInfo,
	    _msg: InstantiateMsg,
	) -> Result<Response, ContractError> {
		let owner_addr = info.sender;
	    OWNER.set(deps, Some(owner_addr.clone()))?;
	    Ok(Response::new().add_attribute("admin", owner_addr))
	}

If you notice, preceding `_` from `deps` and `info` have been removed as they are no longer going to be ignored. 

    let owner_addr = info.sender;

A new variable is being used to hold the value of `info.sender`. `info.sender` contains the value of address who is instantiating the contract. It can be set to some other value besides `info.sender`. Such value can be sent using the `InstantiateMsg`.  For now, `OWNER` is being set to the address instantiating the contract. `set` method of `OWNER` takes the `deps` and an `Option<addr>` as input parameters. `deps` have been explained [earlier](https://engineerhead.github.io/2023/09/11/cosmwasm-rust-framework-tutorial-course-05.html).

     OWNER.set(deps, Some(owner_addr.clone()))?;

`Option` is an enumeration type that represents either the presence of a value or the absence of a value. It is a fundamental type used for handling optional or nullable values. `?` at the end of the statement is used to propagate errors from functions that return a `Result` or `Option`. Usage of `?` will be explained in more details in future. Let's examine the tests now.

    assert_eq!("creator", res.attributes[0].value);

The last line of the test written in [previous chapter](https://engineerhead.github.io/2023/09/12/cosmwasm-rust-framework-tutorial-course-06.html) has been changed. It checks that instantiated contract sets the attribute in response to `creator`.
