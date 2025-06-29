# modernize-make-shared

This check finds the creation of std::shared_ptr objects by explicitly calling the constructor and a new expression, and replaces it with a call to std::make_shared.

此检查会查找通过显式调用构造函数和 new 表达式创建 std::shared_ptr 对象的情况，并将其替换为调用 std::make_shared。

```c++
auto my_ptr = std::shared_ptr<MyPair>(new MyPair(1, 2));

// becomes

auto my_ptr = std::make_shared<MyPair>(1, 2);
```

This check also finds calls to std::shared_ptr::reset() with a new expression, and replaces it with a call to std::make_shared.

此检查还会查找使用 new 表达式调用 std::shared_ptr::reset() 的情况，并将其替换为调用 std::make_shared。

```c++
my_ptr.reset(new MyPair(1, 2));

// becomes

my_ptr = std::make_shared<MyPair>(1, 2);
```

## Options

## 选项

::: option
MakeSmartPtrFunction

A string specifying the name of make-shared-ptr function. Default is std::make_shared.

一个字符串，用于指定 make-shared-ptr 函数的名称。默认值为 std::make_shared。
:::

::: option
MakeSmartPtrFunctionHeader

A string specifying the corresponding header of make-shared-ptr function. Default is memory.

一个字符串，用于指定 make-shared-ptr 函数所对应的头文件。默认值为 memory。
:::

::: option
IncludeStyle

A string specifying which include-style is used, llvm or google. Default is llvm.

一个字符串，用于指定使用哪种 include 风格，llvm 或 google。默认值为 llvm。
:::

::: option
IgnoreMacros

If set to true, the check will not give warnings inside macros. Default is true.

如果设置为 true，该检查不会在宏内部发出警告。默认值为 true。
:::

::: option
IgnoreDefaultInitialization

If set to false, the check does not suggest edits that will transform default initialization into value initialization, as this can cause performance regressions. Default is true.

如果设置为 false，该检查不会建议将默认初始化转换为值初始化的修改，因为这可能会导致性能下降。默认值为 true。
:::
