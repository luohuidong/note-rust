
- [Macros](https://doc.rust-lang.org/book/ch19-06-macros.html#macros)：了解宏，宏的功能类似于其他语言的装饰器。
- [The Little Book of Rust Macros](https://veykril.github.io/tlborm/introduction.html#the-little-book-of-rust-macros)：宏的编写可以看这本书。

在 Rust 中，宏（Macro）是一种特殊的代码生成器，用于在编译时执行代码转换和代码生成。它们允许开发者编写通用代码模板，以及在编译时根据这些模板生成特定代码的能力。简单的来说就是每个宏调用都会被替换成一些 Rust 代码。

Rust macro 分为 [[Declarative Macro]] 和 [[Procedural Macros]] 两类。
