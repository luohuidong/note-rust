
- [Macros](https://doc.rust-lang.org/book/ch19-06-macros.html#macros)：了解宏，宏的功能类似于其他语言的装饰器。
- [The Little Book of Rust Macros](https://veykril.github.io/tlborm/introduction.html#the-little-book-of-rust-macros)：宏的编写可以看这本书。

在 Rust 中，宏（Macro）是一种特殊的代码生成器，用于在编译时执行代码转换和代码生成。它们允许开发者编写通用代码模板，以及在编译时根据这些模板生成特定代码的能力。

macro 相较于函数来说，更加难以阅读与理解，因此比函数的维护难度要高。

Rust macro 的分类：
- declarative macros
- procedural macros
	- custom drive
	- attribute-like
	- function-like

declarative macros 在 rust 中广泛使用，它的定义类似 `match` 的语法。pattern 语法可以参考 [Macros By Example](https://doc.rust-lang.org/reference/macros-by-example.html#macros-by-example) ：

```rust
// lib.rs

#[macro_export]
macro_rules! vec {
    ( $( $x:expr ),* ) => {
        {
            let mut temp_vec = Vec::new();
            $(
                temp_vec.push($x);
            )*
            temp_vec
        }
    };
}
```

```rust
// main.rs

use declarative_macro_example::vec;

fn main() {
    let result = vec![1, 2, 3];
    println!("{:?}", result)
}
```

procedural macros 有三类，分别为 custom derive、attribute-like 和 function-like，这三类 macros 的工作方式什么类似。procedural macros 的表现比较像函数。procedural macros 接受某些代码作用输入，并生成一些代码作为输出。这一点与 declarative macros 对代码进行模式匹配并且替换代码的行为并不一样。