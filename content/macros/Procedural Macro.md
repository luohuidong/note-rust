procedural macro 的形式为一个 rust 函数，输入 token stream 后再输出 token stream。简单的来说就是对 AST 进行修改。

一个 proc-macro 的核心是一个函数，这个函数被类型为 proc-macro 的 crate 导出。如果有多个 proc-macro，可以都放到同一个 crate 中。

当使用 Cargo 的时候，定义类型为 proc-macro 的 crate，需要在 `Cargo.toml` 将 `lib.proc-macro` 设置为 `true`：

```toml
// cargo.toml

[lib]
proc-macro = true
```

procedural macro 有三种形式：

- function-like macros
- derive macros
- attribute macros

这三种形式的 procedural macro 工作方式类似。

procedural macros 允许在编译期间通过代码对 rust 语法进行操作，即对 AST 进行修改。它接受某些代码作用输入，并生成一些代码作为输出。这一点与 declarative macros 对代码进行模式匹配并且替换代码的行为并不一样。

```rust
use proc_macro::TokenStream;
use quote::quote;
use syn;

#[proc_macro_derive(HelloMacro)]
pub fn hello_macro_derive(input: TokenStream) -> TokenStream {
    // Construct a representation of Rust code as a syntax tree
    // that we can manipulate
    let ast = syn::parse(input).unwrap();

    // Build the trait implementation
    impl_hello_macro(&ast)
}

fn impl_hello_macro(ast: &syn::DeriveInput) -> TokenStream {
    let name = &ast.ident;
    let gen = quote! {
        impl HelloMacro for #name {
            fn hello_macro() {
                println!("Hello, Macro! My name is {}!", stringify!(#name));
            }
        }
    };
    gen.into()
}
```
