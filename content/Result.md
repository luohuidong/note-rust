大多数遇到的错误的严重程度并没有到达需要终止整个程序的地步。例如在一个打开文件的场景，文件并不存的情况下尝试打开文件会发生错误，但是在这种情况下可能更希望是创建文件而不是终止整个进程。

类似打开文件的过程一般都会封装到函数中，而为了更好地处理打开文件的错误，函数一般会返回 `Result` 类型的数据

`Result` 是一个 enum，包含两个变体：

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

`Ok(T)` 为成功时返回的值，而 `Err(E)` 为失败时返回的值。`Ok` 和 `Err` 可以直接使用：

```rust
fn result_example(num: i32) -> Result<i32, String> {
    if num == 42 {
        Ok(num)
    } else {
        Err("Not 42".to_string())
    }
}
```

## Result 基本使用场景

打开文件的操作，在标准库中是有现成的函数的，即 `std::fs:File:open` ，该函数返回值为 `Result` 类型。由于 `Result` 为 enum，它可以与 `match` 表达式搭配使用：

```rust
use std::fs::File;

fn main() {
    let greeting_file_result = File::open("hello.txt");

    let greeting_file = match greeting_file_result {
        Ok(file) => file,
        Err(error) => panic!("Problem opening the file: {error:?}"),
    };
}
```

使用 `match` 表达式未免有些繁琐，`Result` 类型其实提供了不少 helper methods 来进行不同的操作，例如 `unwrap` 方法则与上面的 `match` 表达式实现了类似的逻辑。当 `Result` 的值为 `Ok` 变体的时候，`unwrap` 会返回 `Ok` 内部的值。如果 `Result` 为 `Err` 变体的时候，`unwrap` 会调用 `panic!` ：

```rust
use std::fs::File;

fn main() {
    let greeting_file = File::open("hello.txt").unwrap();
}
```

`unwrap` 的 `panic!` 错误信息是没法自定义的，而使用 `expect` 则可以设置 `panic!` 报错信息：

```rust
use std::fs::File;

fn main() {
    let greeting_file = File::open("hello.txt")
        .expect("hello.txt should be included in this project");
}
```

## Propagating Errors

当函数内部发生错误的时候，我们可能会选择将错误传给调用者，由调用者来决定如何处理。还是以打开文件这个场景来举例：

```rust
use std::fs::File;
use std::io::{self, Read};

fn read_username_from_file() -> Result<String, io::Error> {
    let username_file_result = File::open("hello.txt");

    let mut username_file = match username_file_result {
        Ok(file) => file,
        Err(e) => return Err(e),
    };

    let mut username = String::new();

    match username_file.read_to_string(&mut username) {
        Ok(_) => Ok(username),
        Err(e) => Err(e),
    }
}
```

上面的例子中，`File::open` 将文件发生错误的处理移交给 `read_username_from_file` 函数，而 `read_username_from_file` 又将错误移交给它的调用者。