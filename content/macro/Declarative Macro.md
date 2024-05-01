宏是一种简写形式。在编译期间，在检查类型并生成任何机器码之前，每个宏调用都会被展开。也就是说，每个宏调用都会被替换成一些 Rust 代码。

declarative macro 在 rust 中广泛使用，是编写宏最简单的方式，它的定义类似 `match` 的语法。下面是一个简单模拟 `vec` 宏的例子：

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

上面这个例子包含了 declarative macro 的定义跟使用，有个大致印象即可。后续会借助这个简单的例子来介绍 declarative macro。

- [[Declarative macro 定义]]
- [[Declarative macro 调试]]

## 参考资料

- 《Rust 程序设计（第2版）》第21章。
- pattern 语法可以参考 [Macros By Example](https://doc.rust-lang.org/reference/macros-by-example.html#macros-by-example) 。