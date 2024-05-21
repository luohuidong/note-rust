procedural macro 的形式为一个 rust 函数，输入 token stream 后再输出 token stream。简单的来说就是对 AST 进行修改。

一个 proc-macro 的核心是一个函数，这个函数被类型为 proc-macro 的 crate 导出。如果有多个 proc-macro，可以都放到同一个 crate 中。

当使用 Cargo 的时候，定义类型为 proc-macro 的 crate，需要在 `Cargo.toml` 将 `lib.proc-macro` 设置为 `true`：

```toml
// cargo.toml

[lib]
proc-macro = true
```

proc-macro 类型的 crate 会隐式链接到编译器提供的 proc_macro crate，它包含所有需要开发 procedural macros 的东西。`TokenStream` 和 `Span` 是 proc-macro crate 导出的最重要的类型。

procedural macro 有三种形式：

- [[Function-like Proc Macro]]，用于实现 `$name ! $arg` 可调用 macro
- [[Attribute Proc Macro]]，用于实现 `#[$arg]` 属性
- [[Derive Proc Macro]]derive macros，用于实现派生，作为 `#[derive(...)]` 属性的输入

这三种形式的 procedural macro 工作原理几乎相同，通过 attribute 来定义这个 proc-macro 的类型，在输入和输出有一些差异。
