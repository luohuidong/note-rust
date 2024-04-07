`while` 是带循环条件的 `loop`。

```rust
fn main() {
    let mut n = 0;
    while n < 5 {
        println!("n is {}", n);
        n += 1;
    }
}
```