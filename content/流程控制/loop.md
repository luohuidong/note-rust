Rust 提供 `loop` 关键字来表示无限循环。跟其他编程语言一样，可以使用 `break` 退出循环，使用 `continue` 用于跳过其余的迭代并开始新的循环。

```rust
fn main() {
   let mut sum = 0;
   let mut n = 0; 

   loop {
        sum += n;
        n += 1;
        if n > 100 {
            break;
        }
   }

   println!("Sum of 1 to 100 is: {}", sum)
}
```

`break` 后可带上一个值：

```rust
fn main() {
    let mut sum = 0;
    let mut n = 0;

    let result = loop {
        sum += n;
        n += 1;
        if n > 100 {
            break sum;
        }
    };

    println!("Result of loop is: {}", result);
}
```