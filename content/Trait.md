[Traits: Defining Shared Behavior](https://doc.rust-lang.org/book/ch10-02-traits.html#traits-defining-shared-behavior)，Rust 中的 Traits 类似于其他语言的 interface。

当类型实现了某个 trait 后，如果在其他 crate 中实例化这个类型并且想调用 trait 方法，则需要将该 trait 也引入到作用域中。

```rust
use aggregator::{Summary, Tweet};

fn main() {
    let tweet = Tweet {
        username: String::from("horse_ebooks"),
        content: String::from(
            "of course, as you probably already know, people",
        ),
        reply: false,
        retweet: false,
    };

    println!("1 new tweet: {}", tweet.summarize());
}
```

上面的例子中，`Tweet` 这个类型实现了 `Summary` 这个 trait，`summarize` 为 `Summary` 这个 trait 的方法。当实例 `tweet` 要使用 `summarize` 方法，则需要将 `Summary` 这个 trait 也引入到作用域中。

需要注意的是，我们无法对外部的类型实现外部的 trait。两者中必须满足最少其中一个是在我们本地的 crate 中。

在定义 trait 的时候，方法可以有默认的实现，这个有点类似于抽象类。在默认实现中还可以 trait 中的其他方法：

```rust
pub trait Summary {
    fn summarize_author(&self) -> String;

    fn summarize(&self) -> String {
        format!("(Read more from {}...)", self.summarize_author())
    }
}
```
