---
title:  "CosmWasm: A Rust Framework Tutorial/Course 09"
---
![](/assets/images/rusty-laptop.webp)
In [previous chapter](https://engineerhead.github.io/2023/09/19/cosmwasm-rust-framework-tutorial-course-08.html), we discussed how to initialise our smart contract by setting an owner and also by setting starting point for counter.

    #[cfg_attr(not(feature = "library"), entry_point)]
	pub fn execute(
	    deps: DepsMut,
	    _env: Env,
	    info: MessageInfo,
	    msg: ExecuteMsg,
	) -> Result<Response, ContractError> {
	    match msg {
	        ExecuteMsg::Increment {} => execute::increment(deps),
	        ExecuteMsg::Reset { count } => execute::reset(deps, info, count),
	    }
	}
Above section of code is what comes next in our contract. If you notice, it is annotated like `instantiate` function. This annotation defines second entry point of the contract for outer world. Like `instantiate`, the `execute` function works as any entry point if `library` feature is not enabled in project's configuration. The input and output parameters are same like `instantiate`. and have been described earlier. Body of the function `execute` contains following code.

    match msg {
	        ExecuteMsg::Increment {} => execute::increment(deps),
	        ExecuteMsg::Reset { count } => execute::reset(deps, info, count),
    }
The incoming message named `msg` of type `ExecuteMsg` is an enum defined in `msg.rs` as follows

    #[cw_serde]
	pub enum ExecuteMsg {
	    Increment {},
	    Reset { count: i32 },
	}
It is being specified that enum `ExecuteMsg` can either carry an `increment` variant without any data or `reset` variant with some counter value.

So when a `msg` of type `ExecuteMsg` is received, it is matched for either of two possible variants. After matching, the relevant function is executed which in current contract's case can be `increment` or `reset`

     pub fn increment(deps: DepsMut) -> Result<Response, ContractError> {
        STATE.update(deps.storage, |mut state| -> Result<_, ContractError> {
            state.count += 1;
            Ok(state)
        })?;

        Ok(Response::new().add_attribute("action", "increment"))
    }
The `increment` function only takes one parameter which is `deps` and return the response similar to defined by `execute` function.

    STATE.update(deps.storage, |mut state| -> Result<_, ContractError> {
            state.count += 1;
            Ok(state)
        })?;
The body of function `increment` is straightforward. `Item` of `cw_plus_storage` enables us to update the underlying state in one single call using `update` function. `STATE` is of type `Item` and its `update` function takes two parameters. One is dependencies's storage dubbed as `deps.storage` and second is a closure. Closure in Rust is an anonymous function that capture the surrounding variables and make them available in closure's scope. In current case, the closure is taking a mutable reference to the `state` object as an argument while it returns a `Result` with the updated state as its `Ok` variant if the update is successful, or a `ContractError` as its `Err` variant if there's an error. Clousre's body simply adds 1 to the previous counter value and returns the `state` encompassed in Ok.

    Ok(Response::new().add_attribute("action", "increment"))

In the end, a new `Response` is being returned to inform that `increment` action has succeeded.

To test that `increment` function is working without an error, we proceed as follows

    #[test]
    fn increment() {
        let mut deps = mock_dependencies();

        let msg = InstantiateMsg { count: 17 };
        let info = mock_info("creator", &coins(2, "token"));
        let _res = instantiate(deps.as_mut(), mock_env(), info, msg).unwrap();

        // beneficiary can release it
        let info = mock_info("anyone", &coins(2, "token"));
        let msg = ExecuteMsg::Increment {};
        let res = execute(deps.as_mut(), mock_env(), info, msg).unwrap();
		asserteq!(1, res.attributes.len())
    }

 
