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

- function-like macros，用于实现 `$name ! $arg` 可调用 macro
- attribute macros，用于实现 `#[$arg]` 属性
- derive macros，用于实现派生，作为 `#[derive(...)]` 属性的输入

这三种形式的 procedural macro 工作原理几乎相同，通过 attribute 来定义这个 proc-macro 的类型，在输入和输出有一些差异。

## Function-like

function-like procedural macro 是三种 proccedural macros 中最简单的。function-like procedural macro 的简单结构如下所示：

```rust
use proc_macro::TokenStream;

#[proc_macro]
pub fn tlborm_fn_macro(input: TokenStream) -> TokenStream {
    input
}
```

function-like procedural macro 的调用类似与 declarative macro：`makro!(...)`。但从调用来看并不能区分 function-like procedural macro 和 declarative macro。function-like procedural macro 与 declarative macros 有相同的放置和展开规则，因此 macro 必须在调用位置输出正确的 token stream。与 declarative macro 不同的是，function-like procedural macro 对输入并没有做严格限制，并且它能直接对 token 进行修改。

使用例子：

```rust
use tlborm_proc::tlborm_attribute;

fn foo() {
    tlborm_attribute!(be quick; time is mana);
}
```

## Attribute

attribute procedural macro 定义新的外部属性，可以绑定到其他项上面。它的调用方式为 `#[attr]` 或者 `#[attr(...)]`，`...` 为任意 token tree。

一个简单的 attribute procedural macro 结构如下：

```rust
use proc_macro::TokenStream;

#[proc_macro_attribute]
pub fn tlborm_attribute(input: TokenStream, annotated_item: TokenStream) -> TokenStream {
    annotated_item
}
```

第一个参数为 `#[attr(...)]` 中的 `...` 内容，而第二个参数则是 macro 所绑定的项。

```rust
use tlborm_proc::tlborm_attribute;

#[tlborm_attribute]
fn foo() {}

#[tlborm_attribute(attributes are pretty handsome)]
fn bar() {}
```

## Derive

派生不是类似其他语言中的继承概念，在 Rust 中，派生是一种为结构体或枚举自动实现某些 trait 的机制。

```rust
#[proc_macro_derive(MyDerive)]
pub fn my_derive(annotated_item: TokenStream) -> TokenStream {
    TokenStream::new()
}
```














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
