---
title:  "CosmWasm: A Rust Framework Tutorial/Course 10"
---
![](/assets/images/rusty-laptop.png)
In [previous chapter](https://engineerhead.github.io/2023/09/20/cosmwasm-rust-framework-tutorial-course-09.html), we learnt how increment the counter value by 1. Before learning to reset the counter, let's take a look at how to only read the underlying storage on the blockchain.

    #[cfg_attr(not(feature = "library"), entry_point)]
	pub fn query(deps: Deps, _env: Env, msg: QueryMsg) -> StdResult<Binary> {
	    match msg {
	        QueryMsg::GetCount {} => to_binary(&query::count(deps)?),
	    }
	}

The above code defines the 3rd entry point for the CosmWasm contract. It looks quite similar to the earlier entry point which was `execute`. The main difference is that return type has changed and we return the response in a binary. `Binary` is often used when working with low-level data. The possible values of the incoming message are defined in `msg.rs`

    #[cw_serde]
	#[derive(QueryResponses)]
	pub enum QueryMsg {
	    // GetCount returns the current count as a json-encoded number
	    #[returns(GetCountResponse)]
	    GetCount {},
	}

	// We define a custom struct for each query response
	#[cw_serde]
	pub struct GetCountResponse {
	    pub count: i32,
	}

A simple variant of enum `QueryMsg` is defined which is going to return response of type `GetCountResponse`. `GetCountResponse` has only one field i.e `count` of type `integer`

    pub mod query {
	    use super::*;

	    pub fn count(deps: Deps) -> StdResult<GetCountResponse> {
	        let state = STATE.load(deps.storage)?;
	        Ok(GetCountResponse { count: state.count })
	    }
    }

The function `count` responsible for dealing with `GetCount` message simply loads the value of `STATE` and returns the value wrapped in `Ok`. Last but not least let's take a look at the test to check whether correct value of `count` is not only being stored but returned as well. 

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

	        // should increase counter by 1
	        let res = query(deps.as_ref(), mock_env(), QueryMsg::GetCount {}).unwrap();
	        let value: GetCountResponse = from_binary(&res).unwrap();
	        assert_eq!(18, value.count);
	    }

 Following lines have been added to our previous written test.
 

		    // should increase counter by 1
	        let res = query(deps.as_ref(), mock_env(), QueryMsg::GetCount {}).unwrap();
	        let value: GetCountResponse = from_binary(&res).unwrap();
	        assert_eq!(18, value.count);

These lines simply send the `GetCount` variant of `QueryMsg` and then convert it from binary to compare it with desired value.