# bugprone-unused-local-non-trivial-variable

Warns when a local non trivial variable is unused within a function. The following types of variables are excluded from this check:

- trivial and trivially copyable
- references and pointers
- exception variables in catch clauses
- static or thread local
- structured bindings
- variables with [[maybe_unused]] attribute
- name-independent variables

此检查会在函数中存在未使用的非平凡局部变量时发出警告。以下类型的变量将被排除在检查之外：

- 平凡类型和可平凡复制类型
- 引用和指针
- catch 子句中的异常变量
- static 或 thread_local 变量
- 结构化绑定变量
- 带有 [[maybe_unused]] 属性的变量
- 与名称无关的变量

This check can be configured to warn on all non-trivial variables by setting IncludeTypes to .\*, and excluding specific types using ExcludeTypes.

通过将 IncludeTypes 设置为 .\*，并使用 ExcludeTypes 排除特定类型，可以配置此检查对所有非平凡变量发出警告。

In the this example, my_lock would generate a warning that it is unused.

在以下示例中，my_lock 会因为未被使用而触发警告。

```c++
std::mutex my_lock;
// my_lock local variable is never used
```

```c++
std::mutex my_lock;
// my_lock 局部变量从未被使用
```

In the next example, future2 would generate a warning that it is unused.

在下一个示例中，future2 会因为未被使用而触发警告。

```c++
std::future<MyObject> future1;
std::future<MyObject> future2;
// ...
MyObject foo = future1.get();
// future2 is not used.
```

```c++
std::future<MyObject> future1;
std::future<MyObject> future2;
// ...
MyObject foo = future1.get();
// future2 未被使用。
```

## Options

## 选项

::: option
IncludeTypes

Semicolon-separated list of regular expressions matching types of variables to check. By default the following types are checked:

- ::std::.\*mutex
- ::std::future
- ::std::basic_string
- ::std::basic_regex
- ::std::basic_istringstream
- ::std::basic_stringstream
- ::std::bitset
- ::std::filesystem::path

::: option
IncludeTypes

用分号分隔的正则表达式列表，用于匹配需要检查的变量类型。默认情况下，会检查以下类型：

- ::std::.\*mutex
- ::std::future
- ::std::basic_string
- ::std::basic_regex
- ::std::basic_istringstream
- ::std::basic_stringstream
- ::std::bitset
- ::std::filesystem::path
  :::

::: option
ExcludeTypes

A semicolon-separated list of regular expressions matching types that are excluded from the IncludeTypes matches. By default it is an empty list.

::: option
ExcludeTypes

用分号分隔的正则表达式列表，用于匹配需要从 IncludeTypes 匹配中排除的类型。默认情况下该列表为空。
:::
