---
title: Rust Note
---
- 基本概念
	- [[rustc与Cargo]]
	- [[类型系统]]
	- [[变量与可变性]]
	- [[常量]]
	- [[基础数据类型]]
	- [[表达式]]
	- [[枚举与模式]]

## 所有权

[What Is Ownership?](https://doc.rust-lang.org/book/ch04-01-what-is-ownership.html#what-is-ownership)：了解所有权。所有权是 Rust 独特的内存管理机制，能保证资源的及时释放，避免内存泄漏的问题。

所有权的规则：
- Each value in Rust has an _owner_.
- There can only be one owner at a time.
- When the owner goes out of scope, the value will be dropped.

[References and Borrowing](https://doc.rust-lang.org/book/ch04-02-references-and-borrowing.html#references-and-borrowing)：通过引用可以解决函数传参的时候所有权转移的问题。

[The Slice Type](https://doc.rust-lang.org/book/ch04-03-slices.html#the-slice-type)：以引用的方式访问一个集合中连续的一部分。通过 slice 可以安全地访问数据的一部分，避免越界访问。

## 结构体


## 模块系统

- [Packages and Crates](https://doc.rust-lang.org/book/ch07-01-packages-and-crates.html#packages-and-crates)：了解 package 跟 crate 的关系。
- [Defining Modules to Control Scope and Privacy](https://doc.rust-lang.org/book/ch07-02-defining-modules-to-control-scope-and-privacy.html#defining-modules-to-control-scope-and-privacy)：了解模块的定义。
- [Paths for Referring to an Item in the Module Tree](https://doc.rust-lang.org/book/ch07-03-paths-for-referring-to-an-item-in-the-module-tree.html#paths-for-referring-to-an-item-in-the-module-tree)：了解模块的引用。
- [Bringing Paths into Scope with the `use` Keyword](https://doc.rust-lang.org/book/ch07-04-bringing-paths-into-scope-with-the-use-keyword.html#bringing-paths-into-scope-with-the-use-keyword)：通过 use 关键字简化模块引用。
- [Separating Modules into Different Files](https://doc.rust-lang.org/book/ch07-05-separating-modules-into-different-files.html#separating-modules-into-different-files)：了解如何将不同的模块拆分到不同的文件中。

## 错误处理

- [[错误处理]]
