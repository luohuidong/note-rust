
宏是一种简写形式。在编译期间，在检查类型并生成任何机器码之前，每个宏调用都会被展开。也就是说，每个宏调用都会被替换成一些 Rust 代码。declarative macro 在 rust 中广泛使用，是编写宏最简单的方式。

## 定义

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
    let result = macro_example::vec![1, 2, 3];
    println!("{:?}", result)
}
```

这两段代码展示了 declarative macro 的定义与使用。例子简单模拟了 `vec` 宏，有个大致印象即可，后续会对各个部分进行讲解。

## 形式

declarative macro 借助“模式匹配”的方式发挥作用，宏的主体只是一系列规则。其形式如下：

```rust
macro_rules! macro_name {
	( pattern1 ) => { template1 };
	( pattern2 ) => { template2 };
}
```

当 rust 代码匹配了某个 pattern，则内容会替换为对应的 template。

pattern 和 template 是使用 `()` 包裹，还是使用 `[]` 或者 `{}` 进行包裹都是可以的，这对 Rust 来说并没有任何影响，下面三种定义是等效的：

```rust
macro_rules! macro_name {
	{ pattern } => { template };
}

macro_rules! macro_name {
	( pattern ) => ( template );
}

macro_rules! macro_name {
	[ pattern ] => [ template ];
}
```

这个规则同样适用于宏调用的时候，下面三种宏的调用情况是等效的：

```rust
let result = macro_example::vec![1, 2, 3];
let result = macro_example::vec!(1, 2, 3);
let result = macro_example::vec!{1, 2, 3};
```

虽然说用什么进行包裹对 rust 没有影响，但是在使用 declarative macro 的时候建议还是按照惯例来使用。在调用 `assert_eq!` 时使用圆括号，在调用 `vec!` 时使用方括号，而在调用 `macro_rules!` 时使用花括号。

这里已经对 declarative macro 的形式有一个大致的了解，下一章节的内容将展开了解 declarative macro 的 pattern。

## Pattern

declarative macro 的 pattern 虽然形式上与 match 的 pattern 类似，但实际上是完全不一样的东西，因为 declarative macro 的 pattern 用来匹配 rust 代码，而 match 的 pattern 用来匹配值。如果要理解 pattern 的话，其实它更加类似于匹配字符串的正则表达式。

以开头的例子来展开讲解：

```rust
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

上面的例子中，宏 `vec` 定义的 pattern 为 `$( $x:expr ),*`。我们将这个 pattern 分开两部分来理解，一部分为 `$( $x:expr )`，另一部分为 `,*`。

pattern 更加深入的了解可以参考 [Macros By Example](https://doc.rust-lang.org/reference/macros-by-example.html)。

## 调试

[The Little Book of Rust Macros - Debugging](https://veykril.github.io/tlborm/syntax-extensions/debugging.html#debugging) 中提到几种调试 macro 的方式，但原理都是一致的，都是查看调用 macro 的地方代码是否展开正确。

### 借助 Rust Playground

[Rust Playground](https://play.rust-lang.org)页面中，可以选择 “TOOlS” 中的 “Expand macros” 来查看 macro 是否展开正确

![image](https://cdn.luohuidong.cn/clipboard_20240501_113402.png)

展开后的结果：

![Pasted image 20240502125010](http://cdn.luohuidong.cn/Pasted%20image%2020240502125010.png)

### 使用 cargo-expand

[cargo-expand](https://github.com/dtolnay/cargo-expand)用于展示展开 macro 的结果，使用也非常方便：

![Pasted image 20240502134357](http://cdn.luohuidong.cn/Pasted%20image%2020240502134357.png)

## 导出宏

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
