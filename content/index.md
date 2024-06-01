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

Rust 中没有其他语言中类的概念，但是 Rust 中的结构体与类类似。

[Defining and Instantiating Structs](https://doc.rust-lang.org/book/ch05-01-defining-structs.html#defining-and-instantiating-structs)，[An Example Program Using Structs](https://doc.rust-lang.org/book/ch05-02-example-structs.html#an-example-program-using-structs)，了解结构体的定义与使用。

[Method Syntax](https://doc.rust-lang.org/book/ch05-03-method-syntax.html#method-syntax)，了解如何定义 associated functions。

associated function 分为两类，一类是跟实例关联的，这类似于 JavaScript 类中的方法。而另一类则不跟实例关联，类似与 JavaScript 类中的实例方法，这类函数在 Rust 中常用作构造函数，用于生成新的实例。

## 枚举与模式匹配

- [Defining an Enum](https://doc.rust-lang.org/book/ch06-01-defining-an-enum.html#defining-an-enum)，了解 Enum 的使用。
- [The `match` Control Flow Construct](https://doc.rust-lang.org/book/ch06-02-match.html#the-match-control-flow-construct)，`match` 类似与其他语言的 `switch` 语句。
- [Concise Control Flow with `if let`](https://doc.rust-lang.org/book/ch06-03-if-let.html#concise-control-flow-with-if-let)：使用 `if let` 比 `match` 更简洁。

常用的内置 Enum 为 `Option`。 `match` 与 `if let` 可以与 `Option` 搭配使用。使用 `if let` 可以写出比 `match` 更简洁的代码，但是 `if let` 并不像 `match` 那样强制处理 enum 的每一种情况，这需要根据自己的实际情况来选择究竟是使用 `if let` 还是使用 `match` 。

## 模块系统

- [Packages and Crates](https://doc.rust-lang.org/book/ch07-01-packages-and-crates.html#packages-and-crates)：了解 package 跟 crate 的关系。
- [Defining Modules to Control Scope and Privacy](https://doc.rust-lang.org/book/ch07-02-defining-modules-to-control-scope-and-privacy.html#defining-modules-to-control-scope-and-privacy)：了解模块的定义。
- [Paths for Referring to an Item in the Module Tree](https://doc.rust-lang.org/book/ch07-03-paths-for-referring-to-an-item-in-the-module-tree.html#paths-for-referring-to-an-item-in-the-module-tree)：了解模块的引用。
- [Bringing Paths into Scope with the `use` Keyword](https://doc.rust-lang.org/book/ch07-04-bringing-paths-into-scope-with-the-use-keyword.html#bringing-paths-into-scope-with-the-use-keyword)：通过 use 关键字简化模块引用。
- [Separating Modules into Different Files](https://doc.rust-lang.org/book/ch07-05-separating-modules-into-different-files.html#separating-modules-into-different-files)：了解如何将不同的模块拆分到不同的文件中。

## 泛型

[Generic Data Types](https://doc.rust-lang.org/book/ch10-01-syntax.html#generic-data-types)，了解泛型的使用。

Rust 的泛型与 TypeScript 的泛型类似，但更加强大，例如在实现 struct 方法的时候，如果指定类型的话，则该方法仅在这种类型下才能使用，如：

```rust
struct Point<T> {
    x: T,
    y: T,
}

impl<T> Point<T> {
    fn x(&self) -> &T {
        &self.x
    }
}

impl Point<f32> {
    fn distance_from_origin(&self) -> f32 {
        let tmp = (self.x.powi(2) + self.y.powi(2)).sqrt();
        println!("distance_from_origin: {}", tmp);
        tmp
    }
}

fn main() {
    let a = Point { x: 1.0_f64, y: 2.0_f64 };
    a.x();
    a.distance_from_origin(); // 这里会报错，因为只有 Point<f32> 才有 distance_from_origin 方法

    let b = Point {
        x: 1.0_f32,
        y: 2.0_f32,
    };
    b.x();
    b.distance_from_origin(); // 这里正常
}
```

在使用泛型的时候，无需担心会影响运行时的性能，monomorphization（单态化）会在编译时根据具体的类型生成特定的代码实例。这种方式可以消除泛型引入的性能损失，并且在编译时进行类型检查。

## Trait

[Traits: Defining Shared Behavior](https://doc.rust-lang.org/book/ch10-02-traits.html#traits-defining-shared-behavior)，Rust 中的 Traits 类似于其他语言的 interface。

当类型实现了某个 trait 后，如果在其他 crate 中实例化这个类型并且想调用 trait 方法，则需要将该 trait 也引入到作用域中。

```rust
use aggregator::{Summary, Tweet};

fn main() {
    let tweet = Tweet {
        username: String::from("horse_ebooks"),
        content: String::from(
            "of course, as you probably already know, people",
        ),
        reply: false,
        retweet: false,
    };

    println!("1 new tweet: {}", tweet.summarize());
}
```

上面的例子中，`Tweet` 这个类型实现了 `Summary` 这个 trait，`summarize` 为 `Summary` 这个 trait 的方法。当实例 `tweet` 要使用 `summarize` 方法，则需要将 `Summary` 这个 trait 也引入到作用域中。

需要注意的是，我们无法对外部的类型实现外部的 trait。两者中必须满足最少其中一个是在我们本地的 crate 中。

在定义 trait 的时候，方法可以有默认的实现，这个有点类似于抽象类。在默认实现中还可以 trait 中的其他方法：

```rust
pub trait Summary {
    fn summarize_author(&self) -> String;

    fn summarize(&self) -> String {
        format!("(Read more from {}...)", self.summarize_author())
    }
}
```

## 生命周期

[Validating References with Lifetimes](https://doc.rust-lang.org/book/ch10-03-lifetime-syntax.html#validating-references-with-lifetimes)：了解引用的生命周期。

生命周期用于连接函数不同的参数与返回值，这样 rust 会有足够的信息来进行安全的内存操作，防止创建 dangling pointers 和其他破坏内存安全的操作。

## 错误处理

- [Error Handling](https://doc.rust-lang.org/book/ch09-00-error-handling.html#error-handling)
	- [Unrecoverable Errors with `panic!`](https://doc.rust-lang.org/book/ch09-01-unrecoverable-errors-with-panic.html#unrecoverable-errors-with-panic)
	- [Recoverable Errors with `Result`](https://doc.rust-lang.org/book/ch09-02-recoverable-errors-with-result.html#recoverable-errors-with-result)
	- [To `panic!` or Not to `panic!`](https://doc.rust-lang.org/book/ch09-03-to-panic-or-not-to-panic.html#to-panic-or-not-to-panic)

Rust 将错误分为可恢复错误和不可恢复的错误。可恢复错误，类似于文件不存在这种错误，我们通常是想将问题报告给用户，然后进行重试操作。不可恢复错误，通常是 bug，一般遇到的话需要停止程序。

大多数语言并不区分这两种错误，并且使用相同的方式来处理这两种错误。但是在 Rust 中则是分开处理的，可恢复场景使用 Result，不可恢复场景使用 [[panic]] 宏来终止程序执行。

Result 可配合 `match` 或者 `.expect()` 使用。

## Macro

- [The Rust Programming Language - Macros](https://doc.rust-lang.org/book/ch19-06-macros.html#macros)
- [The Rust Reference - Macros](https://doc.rust-lang.org/reference/macros.html#macros)
	- [The Rust Reference - Macros By Example](https://doc.rust-lang.org/reference/macros-by-example.html#macros-by-example) 
	- [The Rust Reference - Procedural Macros](https://doc.rust-lang.org/reference/procedural-macros.html#procedural-macros)
- [The Little Book of Rust Macros](https://veykril.github.io/tlborm/introduction.html#the-little-book-of-rust-macros)
- [《Rust 程序设计（第2版）》第 21 章 - 宏](https://time.geekbang.org/column/article/740828)

Rust 的功能和语法可以通过 macro 进行扩展。macro 是一种特殊的代码生成器，用于在编译时执行代码转换和代码生成。macro 允许开发者编写通用代码模板，以及在编译时根据这些模板生成特定代码的能力。简单的来说就是每个 macro 调用都会被替换成一些 Rust 代码。

Rust macro 分为 [[Declarative Macro]] 和 [[Procedural Macro]] 两类。

## CLI 开发

- [Command Line Book](https://rust-cli.github.io/book/index.html): Learn how to build effective command line applications in Rust.
- [clap](https://docs.rs/clap/latest/clap/index.html#): Command Line Argument Parser for Rust.
