以下是翻译后的 markdown 文件内容，保留了中英文双语，中文在英文下方，并以一个空行分隔，同时保留了代码块格式：

# modernize-min-max-use-initializer-list

Replaces nested std::min and std::max calls with an initializer list where applicable.

替换嵌套的 std::min 和 std::max 调用，在适用的情况下使用初始化列表。

For instance, consider the following code:

例如，考虑以下代码：

```cpp
int a = std::max(std::max(i, j), k);
```

The check will transform the above code to:

该检查会将上述代码转换为：

```cpp
int a = std::max({i, j, k});
```

# Performance Considerations

# 性能考虑

While this check simplifies the code and makes it more readable, it may cause performance degradation for non-trivial types due to the need to copy objects into the initializer list.

虽然此检查简化了代码并提高了可读性，但对于非平凡类型（non-trivial types），由于需要将对象复制到初始化列表中，可能会导致性能下降。

To avoid this, it is recommended to use std::ref or std::cref for non-trivial types:

为避免这种情况，建议对非平凡类型使用 std::ref 或 std::cref：

```cpp
std::string b = std::max({std::ref(i), std::ref(j), std::ref(k)});
```

# Options

# 选项

::: option
IncludeStyle

A string specifying which include-style is used, llvm or google. Default is llvm.

一个字符串，指定使用哪种 include 风格，可选值为 llvm 或 google。默认值为 llvm。
:::

::: option
IgnoreNonTrivialTypes

A boolean specifying whether to ignore non-trivial types. Default is true.

一个布尔值，指定是否忽略非平凡类型。默认值为 true。
:::

::: option
IgnoreTrivialTypesOfSizeAbove

An integer specifying the size (in bytes) above which trivial types are ignored. Default is 32.

一个整数，指定在超过该大小（以字节为单位）时忽略平凡类型。默认值为 32。
:::
