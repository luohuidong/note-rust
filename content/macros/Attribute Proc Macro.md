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

使用例子：

```rust
use tlborm_proc::tlborm_attribute;

#[tlborm_attribute]
fn foo() {}

#[tlborm_attribute(attributes are pretty handsome)]
fn bar() {}
```
