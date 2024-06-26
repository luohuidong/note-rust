有两种方式可以造成 panic。一种是代码造成的 bug，如越界访问 vector：

```rust
fn main() {
    let v = vec![1, 2, 3];
    let _result = v[99];
}
```

另外一种是通过调用 `panic!`：

```rust
fn main() {
    panic!("crash and burn");
}
```

## panic 回溯

运行下面的例子：

```rust
fn main() {
    panic!("crash and burn");
}
```

输出的结果为：

```
   Compiling error_example v0.1.0 (/home/luo/study/rust/error_example)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.14s
     Running `target/debug/error_example`
thread 'main' panicked at src/main.rs:2:5:
crash and burn
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

结果的最后一行提示可以通过添加 `RUST_BACKTRACE=1` 通过回溯获取确切的报错原因。要获取 backtrace 的信息，需要开启 debug symbol，这个在运行 `cargo build` 或者 `cargo run` 时不添加 `--release` 就会自动开启。

运行 `RUST_BACKTRACE=1 cargo run` 得到下面的结果：

```
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.00s
     Running `target/debug/error_example`
thread 'main' panicked at src/main.rs:2:5:
crash and burn
stack backtrace:
   0: rust_begin_unwind
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:645:5
   1: core::panicking::panic_fmt
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/core/src/panicking.rs:72:14
   2: error_example::main
             at ./src/main.rs:2:5
   3: core::ops::function::FnOnce::call_once
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/core/src/ops/function.rs:250:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
```

得到的结果跟其他语言报错所获得的堆栈信息类似，这些信息包含核心 Rust 代码、标准库代码或者是我们正在用的 crate、我们写的代码信息。定位 Bug，其实都可以从第一行出现我们写的代码的信息开始定位。

