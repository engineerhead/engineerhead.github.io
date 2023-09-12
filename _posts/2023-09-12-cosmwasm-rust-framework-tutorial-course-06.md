---
title:  "CosmWasm: A Rust Framework Tutorial/Course 06"
---
![](/assets/images/rust-lang.png)
In previous chapters [04](https://engineerhead.github.io/2023/09/05/cosmwasm-rust-framework-tutorial-course-04.html) and [05](https://engineerhead.github.io/2023/09/11/cosmwasm-rust-framework-tutorial-course-05.html), we wrote a basic "Hello, World" contract in CosmWasm. Let's examine its working by writing a  basic test. In the template  scaffolded, following section of code exists at bottom of  `contract.rs`

    #[cfg(test)]
	mod tests {}

`#[cfg(test)]` attribute means that code in module tests would be only compiled while running tests. `mod` means the a module is being defined. In Rust, modules are used to organize code into separate namespaces and improve code organization. This allows us to include test-specific code, such as test functions and test-related dependencies, without affecting the production code. 

    #[cfg(test)]
	mod tests {
	    use super::*;
	    use cosmwasm_std::testing::{mock_dependencies, mock_env, mock_info};
	    use cosmwasm_std::coins;

	    #[test]
	    fn initialize() {
	        let mut deps = mock_dependencies();

	        let env = mock_env();
	        let info = mock_info("creator", &coins(1000, "earth"));

	        let msg = InstantiateMsg {};

	        let res = instantiate(deps.as_mut(), env, info, msg).unwrap();

	        assert_eq!(1, res.attributes.len());
	    }
	}

Above piece of code is a simple test function that checks if the contract is instantiated properly and returns the intended response. Let's inspect the code piece by piece.

    use super::*;
    use cosmwasm_std::testing::{mock_dependencies, mock_env, mock_info};
    use cosmwasm_std::coins;

 - `use super::*` bring items (functions, types, modules, etc.) from the
   parent module into the current module's scope.
 - 2nd line imports test related utility functions provided by `cosmwasm_std::testing` module.
 - `use cosmwasm_std::coins` function is used in construction of `mock_info`

After importing the required items, we construct them one by one in `initialize` function like this

	 let mut deps = mock_dependencies();

	 let env = mock_env();
	 let info = mock_info("creator", &coins(1000, "earth"));

The initialisation of the contract is tested by passing required items to `instantiate` function.

    let res = instantiate(deps.as_mut(), env, info, msg).unwrap();

In the end, we check the response of the contract's instantiation by checking that it returns exactly one attribute.

    assert_eq!(1, res.attributes.len());

To run the tests, execute following command  in root directory of the project

    cargo test

The last lines of output from above command should result in success like this.

    running 1 test
	test contract::tests::initialize ... ok

	test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s

