假设有一个 main.rs 文件，内容如下：

```rust
fn main() {
    println!("Hello, world!");
}
```

main.rs 文件可通过 `rustc` 命令进行编译，编译完毕之后，会在同一目录下生成一个名为 main 的可执行文件，此时执行 `./main` 课件命令行中输出 `Hello, world!`。

对于简单的程序来说，使用 `rustc` 进行编译没啥问题，但随着项目规模越来越大，为了更容易地管理编译选项以及更便捷地分享代码，则需要使用 Cargo 这个工具。

Cargo 是 Rust 的构建系统以及 package 管理工具。Cargo 可用于创建代码、构建代码、下载依赖、管理 workspace 等。

前面使用 `rustc` 编译源码然后再执行二进制文件的过程可是使用 `Cargo run` 代替。

## 参考资料

- [The Rust Programming Language - Hello, World!](https://doc.rust-lang.org/book/ch01-02-hello-world.html#hello-world)
- [The Rust Programming Language - Hello, Cargo!](https://doc.rust-lang.org/book/ch01-03-hello-cargo.html#hello-cargo)