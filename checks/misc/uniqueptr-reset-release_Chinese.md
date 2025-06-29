# misc-uniqueptr-reset-release

Find and replace unique_ptr::reset(release()) with std::move().

查找并将 unique_ptr::reset(release()) 替换为 std::move()。

Example:

示例：

```c++
std::unique_ptr<Foo> x, y;
x.reset(y.release()); -> x = std::move(y);
```

If y is already rvalue, std::move() is not added. x and y can also be std::unique_ptr<Foo>\*.

如果 y 已经是右值，则不会添加 std::move()。x 和 y 也可以是 std::unique_ptr<Foo>\* 类型。

## Options

## 选项

::: option
IncludeStyle

A string specifying which include-style is used, llvm 或 google。默认值为 llvm。

一个字符串，用于指定使用哪种 include 风格，[llvm]{.title-ref} 或 [google]{.title-ref}。默认是 [llvm]{.title-ref}。
:::
