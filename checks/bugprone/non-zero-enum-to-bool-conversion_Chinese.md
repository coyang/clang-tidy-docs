# bugprone-non-zero-enum-to-bool-conversion

Detect implicit and explicit casts of enum type into bool where enum type doesn't have a zero-value enumerator. If the enum is used only to hold values equal to its enumerators, then conversion to bool will always result in true value. This can lead to unnecessary code that reduces readability and maintainability and can result in bugs.

检测将 enum 类型隐式或显式转换为 bool 的情况，当该 enum 类型没有零值枚举项时。如果 enum 仅用于保存等于其枚举项的值，那么转换为 bool 将始终得到 true。这可能导致冗余代码，降低可读性和可维护性，并可能引发错误。

May produce false positives if the enum is used to store other values (used as a bit-mask or zero-initialized on purpose). To deal with them, // NOLINT or casting first to the underlying type before casting to bool can be used.

如果 enum 被用于存储其他值（例如作为位掩码使用或故意初始化为零），可能会产生误报。为了解决这些情况，可以使用 // NOLINT 注释，或先转换为底层类型再转换为 bool。

It is important to note that this check will not generate warnings if the definition of the enumeration type is not available. Additionally, C++11 enumeration classes are supported by this check.

需要注意的是，如果无法获取枚举类型的定义，此检查不会生成警告。此外，该检查支持 C++11 的枚举类。

Overall, this check serves to improve code quality and readability by identifying and flagging instances where implicit or explicit casts from enumeration types to boolean could cause potential issues.

总体而言，此检查旨在通过识别并标记将枚举类型隐式或显式转换为布尔值的潜在问题，来提升代码质量和可读性。

## Example

示例

```c++
enum EStatus {
  OK = 1,
  NOT_OK,
  UNKNOWN
};

void process(EStatus status) {
  if (!status) {
    // this true-branch won't be executed
    // 此 true 分支不会被执行
    return;
  }
  // proceed with "valid data"
  // 继续处理“有效数据”
}
```

## Options

选项

::: option
EnumIgnoreList

Option is used to ignore certain enum types when checking for implicit/explicit casts to bool. It accepts a semicolon-separated list of (fully qualified) enum type names or regular expressions that match the enum type names. The default value is an empty string, which means no enums will be ignored.

该选项用于在检查 enum 到 bool 的隐式/显式转换时忽略某些枚举类型。它接受一个由分号分隔的（完全限定的）枚举类型名称列表，或匹配枚举类型名称的正则表达式。默认值为空字符串，表示不忽略任何枚举类型。
:::
