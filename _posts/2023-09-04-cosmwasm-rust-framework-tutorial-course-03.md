---
title:  "CosmWasm: A Rust Framework Tutorial/Course 03"
---
![](/assets/images/hello-world.avif)
After [installing and setting up Rust](https://engineerhead.github.io/2023/09/03/cosmwasm-rust-framework-tutorial-course-02.html) in the previous chapter, it is time to dive into basics. Cargo can create a new Rust app for us with initial plumbing done already.

    $ cargo new hello-world
    Created binary (application) `hello-world` package
    
    $cd hello-world
    $ tree
    ├── Cargo.toml
	└── src
	    └── main.rs
	1 directory, 2 files

Cargo has created one directory and two files. Cargo.toml contains the meta data of the app and is called the manifest.

    [package]
    name = "hello-world"
    version = "0.1.0"
    edition = "2021"
    # See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html
    [dependencies]

Cargo.toml file is self explanatory for now. It tells the project name, project's version, rust edition to compile with, and package's dependencies which are empty at initial stage.

Second file is *main.rs* under the *src* directory and contains traditional *hello-world* code.

    fn main() {
	    println!("Hello, world!");
    }

Let's dig through classic hello-world code. First keyword of the code is **fn** which is pronounced as fun. It tells that a function definition is starting. **main**  is the function's name. **main** is a special function as it is the first code that runs in each Rust program. Following are parentheses which contain the function's parameters. Empty parentheses means that there are no parameters . Opening curly brace **{** indicates start of the function's body and closing curly brace **}** means end of function.
In between the curly braces, there is only one line. First, let's see
what it does by running the following command in terminal.

    $ cargo run
    Compiling hello-world v0.1.0 (/Users/umairbussi/workspace/prog_rust/hello-world)
    Finished dev [unoptimized + debuginfo] target(s) in 2.53s
    Running `target/debug/hello-world`
    Hello, world!

It prints *Hello, world!* to the screen. **println!** is a macro which outputs the given parameter to screen. **!** at the end of println! indicates that it is a macro, not a function as these two are different things and will be explained later. *Hello, world!* is a string provided as an argument to macro. At the end, there is semicolon which terminates the expression.