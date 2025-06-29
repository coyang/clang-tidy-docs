# readability-simplify-subscript-expr

# readability-simplify-subscript-expr（可读性 - 简化下标表达式）

This check simplifies subscript expressions. Currently this covers calling .data() and immediately doing an array subscript operation to obtain a single element, in which case simply calling operator[] suffice.

此检查用于简化下标表达式。目前涵盖的情况是调用 .data() 后立即进行数组下标操作以获取单个元素，在这种情况下，直接使用 operator[] 即可。

Examples:

示例：

```c++
std::string s = ...;
char c = s.data()[i];  // char c = s[i];
```

## Options

## 选项

::: option
Types

The list of type(s) that triggers this check. Default is [::std::basic_string;::std::basic_string_view;::std::vector;::std::array;::std::span]{.title-ref}

类型

触发此检查的类型列表。默认值为 [::std::basic_string;::std::basic_string_view;::std::vector;::std::array;::std::span]{.title-ref}
:::
