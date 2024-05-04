
- [Macros](https://doc.rust-lang.org/book/ch19-06-macros.html#macros)：了解宏，宏的功能类似于其他语言的装饰器。
- [The Little Book of Rust Macros](https://veykril.github.io/tlborm/introduction.html#the-little-book-of-rust-macros)：宏的编写可以看这本书。

在 Rust 中，宏（Macro）是一种特殊的代码生成器，用于在编译时执行代码转换和代码生成。它们允许开发者编写通用代码模板，以及在编译时根据这些模板生成特定代码的能力。简单的来说就是每个宏调用都会被替换成一些 Rust 代码。

Rust macro 分为 Declarative Macro 和 Procedural Macros 两类。

## Declarative Macro

宏是一种简写形式。在编译期间，在检查类型并生成任何机器码之前，每个宏调用都会被展开。也就是说，每个宏调用都会被替换成一些 Rust 代码。declarative macro 在 rust 中广泛使用，是编写宏最简单的方式。

### 定义

declarative macro 通过 `macro_rules!` 定义，与 `match` 的使用类似。下面是一个简单模拟 `vec` 宏的例子：

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

use macro_example;

fn main() {
    let result = macro_example::vec!{1, 2, 3};
    println!("{:?}", result)
}
```

declarative macro 借助“模式匹配”的方式发挥作用，宏的主体只是一系列规则。其形式如下：

```rust
macro_rules! macro_name {
	( pattern1 ) => ( template1 );
	( pattern2 ) => ( template2 );
}
```

pattern 和 template 是使用 `()` 包裹，还是使用 `[]` 或者 `{}` 进行包裹都是可以的，这对 Rust 来说并没有任何影响，下面三种定义是
等效的：

```rust
macro_rules! vec {
    { $( $x:expr ),* } => {
        {
            let mut temp_vec = Vec::new();
            $(
                temp_vec.push($x);
            )*
            temp_vec
        }
    };
}

macro_rules! vec {
    ( $( $x:expr ),* ) => (
        {
            let mut temp_vec = Vec::new();
            $(
                temp_vec.push($x);
            )*
            temp_vec
        }
    );
}

macro_rules! vec {
    [ $( $x:expr ),* ] => [
        {
            let mut temp_vec = Vec::new();
            $(
                temp_vec.push($x);
            )*
            temp_vec
        }
    ];
}
```

这个规则同样适用于宏调用的时候，下面三种宏的调用情况是等效的：

```rust
let result = macro_example::vec![1, 2, 3];
let result = macro_example::vec!(1, 2, 3);
let result = macro_example::vec!{1, 2, 3};
```

虽然说用什么进行包裹对 rust 没有影响，但是在使用 declarative macro 的时候建议还是按照惯例来使用。在调用 `assert_eq!` 时使用圆括号，在调用 `vec!` 时使用方括号，而在调用 `macro_rules!` 时使用花括号。

### 调试

[The Little Book of Rust Macros - Debugging](https://veykril.github.io/tlborm/syntax-extensions/debugging.html#debugging) 中提到几种调试 macro 的方式，但原理都是一致的，都是查看调用 macro 的地方代码是否展开正确。

#### 借助 Rust Playground

[Rust Playground](https://play.rust-lang.org)页面中，可以选择 “TOOlS” 中的 “Expand macros” 来查看 macro 是否展开正确

![image](https://cdn.luohuidong.cn/clipboard_20240501_113402.png)

展开后的结果：

![Pasted image 20240502125010](http://cdn.luohuidong.cn/Pasted%20image%2020240502125010.png)

#### 使用 cargo-expand

[cargo-expand](https://github.com/dtolnay/cargo-expand)用于展示展开 macro 的结果，使用也非常方便：

![Pasted image 20240502134357](http://cdn.luohuidong.cn/Pasted%20image%2020240502134357.png)

### 导出宏

标有 `#[macro_export]` 的宏会自动变为 public，可以像其他语法项一样通过路径引用。例子：

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

use macro_example;

fn main() {
    let result = macro_example::vec!{1, 2, 3};
    println!("{:?}", result)
}
```

## Procedural Macros

procedural macros 有三类，分别为 custom derive、attribute-like 和 function-like，这三种 macros 的工作方式都是类似。procedural macros 的表现比较像函数。procedural macros 接受某些代码作用输入，并生成一些代码作为输出。这一点与 declarative macros 对代码进行模式匹配并且替换代码的行为并不一样。

### procedural macros 需要在独立的 crate 中

截止2024年05月04日，编写 procedural macro 需要在独立的 crate 中。如果需要编写一个 custom derive procedural，根据约定，它的命名应该以 derive 为后缀。例如 foo_derive。

另外需要在 Cargo.toml 中添加：

```toml
[lib]
proc-macro = true
```




## 参考资料

- [The Rust Programming Language - Macros](https://doc.rust-lang.org/book/ch19-06-macros.html#macros)
- [Macros By Example](https://doc.rust-lang.org/reference/macros-by-example.html#macros-by-example) 
- [The Rust Reference - Macros](https://doc.rust-lang.org/reference/macros.html#macros)
- [The Little Book of Rust Macros](https://veykril.github.io/tlborm/introduction.html#the-little-book-of-rust-macros)
- [《Rust 程序设计（第2版）》第 21 章 - 宏](https://time.geekbang.org/column/article/740828)
