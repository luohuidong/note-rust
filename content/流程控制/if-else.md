在 Rust 中，`if-else` 是表达式。

If 后的布尔条件不需要用括号括起来。

```rust
fn main() {
    let n = 5;

    if n < 0 {
        println!("{} is negative", n);
    } else if n > 0 {
        println!("{} is positive", n);
    } else {
        println!("{} is zero", n);
    }
}
```

如果 if-else 返回一个值，则所有分支必须返回相同的类型：

```rust
fn main() {
    let n = 5;

    let m = if n < 0 {
        println!("{} is negative", n);
        0
    } else {
        println!("{} is positive", n);
        1
    };

    println!("m is {}", m);
}
```

