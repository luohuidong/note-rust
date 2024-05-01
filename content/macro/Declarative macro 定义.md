declarative macro 通过 `macro_rules!` 定义，与 `match` 的使用类似。declarative macro 借助“模式匹配”的方式发挥作用，宏的主体只是一系列规则。其形式如下：

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

## 参考资料

- 《Rust 程序设计（第2版）》第21章。
- pattern 语法可以参考 [Macros By Example](https://doc.rust-lang.org/reference/macros-by-example.html#macros-by-example) 。