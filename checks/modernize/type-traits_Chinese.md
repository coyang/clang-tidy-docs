以下是翻译后的 markdown 文件内容，保留了中英文双语，中文在英文下方，并以一个空行分隔，同时保留了原始代码块格式：

# modernize-type-traits

Converts standard library type traits of the form traits<...>::type and traits<...>::value into traits_t<...> and traits_v<...> respectively.

将标准库类型萃取（type traits）形式 traits<...>::type 和 traits<...>::value 转换为 traits_t<...> 和 traits_v<...>。

For example:

例如：

```c++
std::is_integral<T>::value
std::is_same<int, float>::value
typename std::add_const<T>::type
std::make_signed<unsigned>::type
```

Would be converted into:

将被转换为：

```c++
std::is_integral_v<T>
std::is_same_v<int, float>
std::add_const_t<T>
std::make_signed_t<unsigned>
```

## Options

## 选项

::: option
IgnoreMacros

If true don't diagnose traits defined in macros.

如果为 true，则不会诊断宏中定义的类型萃取。

Note: Fixes will never be emitted for code inside of macros.

注意：不会对宏内部的代码进行修复。

```c++
#define IS_SIGNED(T) std::is_signed<T>::value
```

Defaults to false.

默认值为 false。
:::
