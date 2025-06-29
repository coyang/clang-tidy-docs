# modernize-use-nullptr

The check converts the usage of null pointer constants (e.g. NULL, 0) to use the new C++11 and C23 nullptr keyword.

该检查会将空指针常量（例如 NULL、0）的用法转换为使用 C++11 和 C23 中引入的 nullptr 关键字。

## Example

示例

```c++
void assignment() {
  char *a = NULL;
  char *b = 0;
  char c = 0;
}

int *ret_ptr() {
  return 0;
}
```

transforms to:

转换为：

```c++
void assignment() {
  char *a = nullptr;
  char *b = nullptr;
  char c = 0;
}

int *ret_ptr() {
  return nullptr;
}
```

## Options

选项

::: option
IgnoredTypes

Semicolon-separated list of regular expressions to match pointer types for which implicit casts will be ignored. Default value: [std::_CmpUnspecifiedParam::;^std::__cmp_cat::__unspec]{.title-ref}.

以分号分隔的正则表达式列表，用于匹配在检查中将忽略隐式转换的指针类型。默认值为：[std::_CmpUnspecifiedParam::;^std::__cmp_cat::__unspec]{.title-ref}。

:::

::: option
NullMacros

Comma-separated list of macro names that will be transformed along with NULL. By default this check will only replace the NULL macro and will skip any similar user-defined macros.

以逗号分隔的宏名称列表，这些宏将在替换 NULL 的同时被转换。默认情况下，此检查仅替换 NULL 宏，并跳过任何类似的用户自定义宏。

:::

### Example

示例

```c++
#define MY_NULL (void*)0
void assignment() {
  void *p = MY_NULL;
}
```

transforms to:

转换为：

```c++
#define MY_NULL NULL
void assignment() {
  int *p = nullptr;
}
```

if the NullMacros option is set to MY_NULL.

如果将 NullMacros 选项设置为 MY_NULL。
