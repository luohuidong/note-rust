---
title: Rust Note
---
## 基础概念

[Hello, World!](https://doc.rust-lang.org/book/ch01-02-hello-world.html#hello-world)，[Hello, Cargo!](https://doc.rust-lang.org/book/ch01-03-hello-cargo.html#hello-cargo)，了解 rustc 和 cargo。

[Variables and Mutability](https://doc.rust-lang.org/book/ch03-01-variables-and-mutability.html#variables-and-mutability)，了解变量的不可变性与可变性、常量、变量 shadowing。

[Data Types](https://doc.rust-lang.org/book/ch03-02-data-types.html#data-types)，了解 Rust scalar 和 compound 类型数据。scalar 类型表示单个数值。而 compound 类型表示一组相同类型的值。

- 四个主要的 scalar type 类型数据：
	- integers
	- floating-point numbers
	- booleans
	- characters
- compound type 类型数据：
	- tuples
	- arrays

[Functions](https://doc.rust-lang.org/book/ch03-03-how-functions-work.html#functions)，了解 Rust 的函数定义、了解语句与表达式的区别。Rust 是一门基于表达式的语言，因此了解语句与表达式的区别比较重要。

[Control Flow](https://doc.rust-lang.org/book/ch03-05-control-flow.html#control-flow)，了解 Rust 中的控制流，了解 `if`、`loop`、`while`、`for` 的基本使用。`if` 与 `loop` 比较有意思，可以有返回值。

## 所有权

[What Is Ownership?](https://doc.rust-lang.org/book/ch04-01-what-is-ownership.html#what-is-ownership)，了解所有权。所有权是 Rust 独特的内存管理机制，能保证资源的及时释放，避免内存泄漏的问题。

所有权的规则：
- Each value in Rust has an _owner_.
- There can only be one owner at a time.
- When the owner goes out of scope, the value will be dropped.

[References and Borrowing](https://doc.rust-lang.org/book/ch04-02-references-and-borrowing.html#references-and-borrowing)，通过引用可以解决函数传参的时候所有权转移的问题。

[The Slice Type](https://doc.rust-lang.org/book/ch04-03-slices.html#the-slice-type)，以引用的方式访问一个集合中连续的一部分。通过 slice 可以安全地访问数据的一部分，避免越界访问。

## 结构体

rust 的结构体类似 JavaScript 中的类：

- [Defining and Instantiating Structs](https://doc.rust-lang.org/book/ch05-01-defining-structs.html#defining-and-instantiating-structs)
- [An Example Program Using Structs](https://doc.rust-lang.org/book/ch05-02-example-structs.html#an-example-program-using-structs)
- [Method Syntax](https://doc.rust-lang.org/book/ch05-03-method-syntax.html#method-syntax)

## 模块系统

- [Packages and Crates](https://doc.rust-lang.org/book/ch07-01-packages-and-crates.html#packages-and-crates)：了解 package 跟 crate 的关系。
- [Defining Modules to Control Scope and Privacy](https://doc.rust-lang.org/book/ch07-02-defining-modules-to-control-scope-and-privacy.html#defining-modules-to-control-scope-and-privacy)：了解模块的定义。
- [Paths for Referring to an Item in the Module Tree](https://doc.rust-lang.org/book/ch07-03-paths-for-referring-to-an-item-in-the-module-tree.html#paths-for-referring-to-an-item-in-the-module-tree)：了解模块的引用。
- [Bringing Paths into Scope with the `use` Keyword](https://doc.rust-lang.org/book/ch07-04-bringing-paths-into-scope-with-the-use-keyword.html#bringing-paths-into-scope-with-the-use-keyword)：通过 use 关键字简化模块引用。
- [Separating Modules into Different Files](https://doc.rust-lang.org/book/ch07-05-separating-modules-into-different-files.html#separating-modules-into-different-files)：了解如何将不同的模块拆分到不同的文件中。

