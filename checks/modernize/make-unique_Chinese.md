# modernize-make-unique

This check finds the creation of std::unique_ptr objects by explicitly calling the constructor and a new expression, and replaces it with a call to std::make_unique, introduced in C++14.

此检查会查找通过显式调用构造函数和 new 表达式创建 std::unique_ptr 对象的代码，并将其替换为 C++14 引入的 std::make_unique 调用。

```c++
auto my_ptr = std::unique_ptr<MyPair>(new MyPair(1, 2));

// becomes

auto my_ptr = std::make_unique<MyPair>(1, 2);
```

This check also finds calls to std::unique_ptr::reset() with a new expression, and replaces it with a call to std::make_unique.

此检查还会查找使用 new 表达式调用 std::unique_ptr::reset() 的代码，并将其替换为 std::make_unique 调用。

```c++
my_ptr.reset(new MyPair(1, 2));

// becomes

my_ptr = std::make_unique<MyPair>(1, 2);
```

## Options

## 选项

::: option
MakeSmartPtrFunction

A string specifying the name of make-unique-ptr function. Default is std::make_unique.

指定 make-unique-ptr 函数名称的字符串。默认值为 std::make_unique。
:::

::: option
MakeSmartPtrFunctionHeader

A string specifying the corresponding header of make-unique-ptr function. Default is <memory>.

指定 make-unique-ptr 函数对应头文件的字符串。默认值为 <memory>。
:::

::: option
IncludeStyle

A string specifying which include-style is used, llvm or google. Default is llvm.

指定使用哪种 include 风格的字符串，可选值为 llvm 或 google。默认值为 llvm。
:::

::: option
IgnoreMacros

If set to true, the check will not give warnings inside macros. Default is true.

如果设置为 true，此检查将在宏内部不发出警告。默认值为 true。
:::

::: option
IgnoreDefaultInitialization

If set to false, the check does not suggest edits that will transform default initialization into value initialization, as this can cause performance regressions. Default is true.

如果设置为 false，此检查不会建议将默认初始化转换为值初始化的修改，因为这可能会导致性能下降。默认值为 true。
:::
