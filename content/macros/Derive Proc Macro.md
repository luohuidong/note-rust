derive procedural macro 作用于 derive 属性。调用方式如 `#[derive(TlbormDerive)]`。

简单的结构如下：

```rust
#[proc_macro_derive(MyDerive)]
pub fn my_derive(annotated_item: TokenStream) -> TokenStream {
    TokenStream::new()
}
```

输入的 token stream 为 derive 属性所绑定的项，这些项一般为 `enum`、`struct` 或者 `union`。

使用例子：

```rust
use tlborm_proc::TlbormDerive;

#[derive(TlbormDerive)]
struct Foo;
```

### Helper Attributes

derive proc macro 的特殊之处在于额可以添加额外的属性，这个属性只有项定义的范围才可见。这些属性称之为 derive macro helper attributes。这些属性的目的是给 derive proc macro 提供针对每个 field 或者 variant 的可定制性。helper attributes 是 inert 的，所以在属性处理期间并不会被清除，因此对所有 macro 可见。

helper attribute 可以通过给 `proc_macro_derive` 属性添加 `attributes(helper0, helper1, ..)` 参数来定义，`helper0`，`helper1` 这些标识符为 helper attribute 的名字。

带有 helper attribute 的 derive proc macro 结构如下：

```rust
use proc_macro::TokenStream;

#[proc_macro_derive(TlbormDerive, attributes(tlborm_helper))]
pub fn tlborm_derive(item: TokenStream) -> TokenStream {
    TokenStream::new()
}
```

使用示例：

```rust
use tlborm_proc::TlbormDerive;

#[derive(TlbormDerive)]
struct Foo {
    #[tlborm_helper]
    field: u32
}

#[derive(TlbormDerive)]
enum Bar {
    #[tlborm_helper]
    Variant { #[tlborm_helper] field: u32 }
}
```