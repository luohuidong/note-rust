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
