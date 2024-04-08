`match` 表达式类似于其他语言的 `switch` 语句。其形式为：

```rust
match value {
	pattern => expr,
}
```

`match` 表达式所有分支都必须具有相同的类型。即所有的 `pattern` 需要是同一种类型，同样的所有 `expr` 也需要是同一种类型。

`match`的多功能性源于每个分支 `=>` 左侧支持的多个模式。模式可以解构元组、匹配结构体的各个字段、可以匹配结构体的各个字段等等。

```rust
fn main() {
    foo(1);
    foo(2);
    foo(3);
    foo(4);
}

fn foo(x: i32) {
    match x {
        1 => println!("one"),
        2 => println!("two"),
        3 => println!("three"),
        _ => println!("something else"),
    }
}
```

通配符模式 `_` 会匹配所有内容，类似于 `switch` 语句中的 `default`。`_` 模式必须放到最后，如果放到最前，则意味着它会优先于其他模式，其他模式将永远没有机会匹配。
