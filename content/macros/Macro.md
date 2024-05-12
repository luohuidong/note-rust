- [Macros](https://doc.rust-lang.org/book/ch19-06-macros.html#macros)：了解宏，宏的功能类似于其他语言的装饰器。
- [The Little Book of Rust Macros](https://veykril.github.io/tlborm/introduction.html#the-little-book-of-rust-macros)：宏进阶可以看这本书。

在 Rust 中，宏（Macro）是一种特殊的代码生成器，用于在编译时执行代码转换和代码生成。它们允许开发者编写通用代码模板，以及在编译时根据这些模板生成特定代码的能力。简单的来说就是每个宏调用都会被替换成一些 Rust 代码。

Rust macro 分为 [[Declarative Macro]] 和 Procedural Macros 两类。

## Procedural Macros

procedural macros 有三类，分别为 custom derive、attribute-like 和 function-like，这三种 macros 的工作方式都是类似。procedural macros 的表现比较像函数。procedural macros 接受某些代码作用输入，并生成一些代码作为输出。这一点与 declarative macros 对代码进行模式匹配并且替换代码的行为并不一样。

截止2024年05月04日，编写 procedural macro 需要在独立的 crate 中。如果需要编写一个 custom derive procedural，根据约定，它的命名应该以 derive 为后缀。例如 foo_derive。

另外需要在 Cargo.toml 中添加：

```toml
[lib]
proc-macro = true
```


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


## 参考资料

- [The Rust Programming Language - Macros](https://doc.rust-lang.org/book/ch19-06-macros.html#macros)
- [Macros By Example](https://doc.rust-lang.org/reference/macros-by-example.html#macros-by-example) 
- [The Rust Reference - Macros](https://doc.rust-lang.org/reference/macros.html#macros)
- [The Little Book of Rust Macros](https://veykril.github.io/tlborm/introduction.html#the-little-book-of-rust-macros)
- [《Rust 程序设计（第2版）》第 21 章 - 宏](https://time.geekbang.org/column/article/740828)
