# modernize-use-bool-literals

Finds integer literals which are cast to bool.

查找被转换为 bool 类型的整数字面量。

```c++
bool p = 1;
bool f = static_cast<bool>(1);
std::ios_base::sync_with_stdio(0);
bool x = p ? 1 : 0;

// transforms to

bool p = true;
bool f = true;
std::ios_base::sync_with_stdio(false);
bool x = p ? true : false;
```

## Options

选项

::: option
IgnoreMacros

If set to true, the check will not give warnings inside macros. Default is true.

如果设置为 true，则该检查不会在宏内部发出警告。默认值为 true。
:::
