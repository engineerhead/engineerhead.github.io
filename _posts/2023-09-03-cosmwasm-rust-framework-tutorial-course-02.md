---
title: "CosmWasm: A Rust Framework Tutorial/Course 02"
---
![](/assets/images/rust-logo.png)
Introduction to CosmWasm and Web Assembly was made in [previous post](https://engineerhead.github.io/2023/09/02/cosmwasm-rust-framework-tutorial-course-01.html) . Let's introduce you to Rust programming language. A brief introduction  to salient features of Rust will be made. Then instructions to setup Rust on your device are discussed.

Rust is a general purpose programming language which supports multiple programming paradigms. Rust promises type safety, memory safety, performance, and concurrency. To ensure memory safety, Rust doesn't use garbage collector or reference counting like other programming languages. 

The ownership system of Rust enforces that there can be only one owner of each value at a time. Rust employs borrow checker to enforce this. By following a set of rules, the borrow checker  keeps track of the objects throughout the program. During compile time, it determines where objects are to be initialised and where they need to be dropped. Rust's mechanics ensure that problems in other languages like data races, null or dangling pointers don't make it to final product. The borrow checker and type system of Rust also help mitigate concurrency problems.

Rust code compiles to native machine code across different platforms. Resulting binaries are self contained and the generated code is meant to perform as fast as code written in C or C++.

Rust is mostly used as systems programming language for development of operating systems, compiler, browser engines, embedded systems and etc. However, it also presents other high level features like function programming constructs.

The recommended way to install Rust on your system is rustup. Go to [rust-lang.org](rust-lang.org) and follow the instructions there. There are also options to install pre built binaries for different operating systems. rustup is recommended because it enables installation and usage of multiple rust versions on one system. Once the installation is complete, following commands are available. 

    $ rustup --version
	rustup 1.26.0 (5af9b9484 2023-04-05)
	info: This is the version for the rustup toolchain manager, not 			the rustc compiler.
	info: The currently active `rustc` version is `rustc 1.72.0 (5680fa18f 2023-08-23)` 

**rustup** is a toolchain manager which manages installation and up gradation of different rust versions.

    $ rustc --version
    rustc 1.72.0 (5680fa18f 2023-08-23)

**rustc** is Rust compiler which produces the binary code. Usually, cargo is used to invoke the compiler.

    $ cargo --version
    cargo 1.72.0 (103a7ff2e 2023-08-15)

**Cargo** is the Rust's package manager and build tool. It manages project's dependencies, compiles the dependencies, and creates distributable packages.
    
    $ rustdoc --version
    rustdoc 1.72.0 (5680fa18f 2023-08-23)

**rustdoc** is Rust's documentation tool. It creates a nicely formatted HTML version of the documentation of your source code provided that appropriate comments are written.