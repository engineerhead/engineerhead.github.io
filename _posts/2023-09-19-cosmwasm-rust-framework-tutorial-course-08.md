---
title:  "CosmWasm: A Rust Framework Tutorial/Course 08"
---
![](/assets/images/rusty-macbook.png)
 Previously, we learnt how to [set an admin for a CosmWasm contract](https://engineerhead.github.io/2023/09/18/cosmwasm-rust-framework-tutorial-course-07.html). Let's implement a simple counter contract where only admin will be able to reset it. This is going to be our first contract where we are going to deal with messages that define contract's working and also deal with blockchain's underlying storage.

    #[cfg(not(feature = "library"))]
	use cosmwasm_std::entry_point;
	use cosmwasm_std::{to_binary, Binary, Deps, DepsMut, Env, MessageInfo, Response, StdResult};
	use cw2::set_contract_version;

	use crate::error::ContractError;
	use crate::msg::{ExecuteMsg, GetCountResponse, InstantiateMsg, QueryMsg};
	use crate::state::{State, STATE};
	
	use cw_controllers::Admin;

	// version info for migration info
	const CONTRACT_NAME: &str = "crates.io:counter";
	const CONTRACT_VERSION: &str = env!("CARGO_PKG_VERSION");
	
	const OWNER: Admin = Admin::new("owner");

Most of the imports in above piece of code are same from previous chapter. Let's discuss the different ones.

    use crate::msg::{ExecuteMsg, GetCountResponse, InstantiateMsg, QueryMsg};

`GetCountResponse` is a `struct` being imported from crate's `msg` module.  Its implementation is

    #[cw_serde]
	pub struct GetCountResponse {
	    pub count: i32,
	}
Its purpose is to define the format for response which is returned when contract is queried for current state of counter.

Next, new import is

    use crate::state::{State, STATE};

Following is the code which implements the above imports

    use schemars::JsonSchema;
	use serde::{Deserialize, Serialize};
	
	use cw_storage_plus::Item;

	#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, Eq, JsonSchema)]
	pub struct State {
	    pub count: i32,
	}

	pub const STATE: Item<State> = Item::new("state");

The top two imports from crate's `schemars` and `serde`. `JsonSchema` helps implement the schema for the applied `struct`. Meanwhile the names of next imports which are `Deserialize` and `Serialize` are sufficient enough to describe their functionality.

    use cw_storage_plus::Item;

`Item` gives access to underlying blockchain's storage and stores one typed item at the given key. This is an analog of Singleton.

    #[derive(Serialize, Deserialize, Clone, Debug, PartialEq, Eq, JsonSchema)]
	pub struct State {
	    pub count: i32,
	}

Rust derive attribute called `#[derive(...)]` is  being used to automatically generate implementations of several common traits and behaviours for the struct `State`. The struct has only one field named `counter` which is of type integer.

    #[cfg_attr(not(feature = "library"), entry_point)]
	pub fn instantiate(
	    mut deps: DepsMut,
	    _env: Env,
	    info: MessageInfo,
	    msg: InstantiateMsg,
	) -> Result<Response, ContractError> {
		let owner_addr = info.sender;
	    OWNER.set(deps.branch(), Some(owner_addr.clone()))?;
	    
	    let state = State {
	        count: msg.count,	
	    };
	    set_contract_version(deps.storage, CONTRACT_NAME, CONTRACT_VERSION)?;
	    
	    STATE.save(deps.storage, &state)?;

	    Ok(Response::new()
	        .add_attribute("method", "instantiate")
	        .add_attribute("owner", owner_addr)
	        .add_attribute("count", msg.count.to_string()))
	}

The above piece of code is executed on contract's initialisation. An `OWNER` is being set to the address which is instantiating the contract as follows.

    let owner_addr = info.sender;
	`enter code here`OWNER.set(deps, Some(owner_addr.clone()))?;

Next, we simply define a struct of type `State` where field `counter` is being set to the value coming in `InstantiateMsg`

    let state = State {
	        count: msg.count,	
	};

Last but not least, the initialised struct `state` is stored in item `STATE`

    STATE.save(deps.storage, &state)?;

In the end, a response is being sent with different attributes set to depict that contact has been initialised with specified values.

   	    Ok(Response::new()
        .add_attribute("method", "instantiate")
        .add_attribute("owner", owner_addr)
        .add_attribute("count", msg.count.to_string()))

As far as testing for instantiation is concerned, following is the implementation

    #[cfg(test)]
	mod tests {
	    use super::*;
	    use cosmwasm_std::testing::{mock_dependencies, mock_env, mock_info};
	    use cosmwasm_std::{coins, from_binary};

	    #[test]
	    fn proper_initialization() {
	        let mut deps = mock_dependencies();

	        let msg = InstantiateMsg { count: 17 };
	        let info = mock_info("creator", &coins(1000, "earth"));

	        // we can just call .unwrap() to assert this was a success
	        let res = instantiate(deps.as_mut(), mock_env(), info, msg).unwrap();
	        assert_eq!(3, res.attributes.len());
	    }
     }

We have covered [testing previously](https://engineerhead.github.io/2023/09/12/cosmwasm-rust-framework-tutorial-course-06.html). In `counter's` instantiation  case, the addition is that a value is being sent in `InstantiateMsg` which is stored later.

    let msg = InstantiateMsg { count: 17 };

 
