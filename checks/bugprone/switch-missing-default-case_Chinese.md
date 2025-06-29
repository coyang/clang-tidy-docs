# bugprone-switch-missing-default-case

Ensures that switch statements without default cases are flagged, focuses only on covering cases with non-enums where the compiler may not issue warnings.

确保没有 default 分支的 switch 语句会被标记，仅关注编译器可能不会发出警告的非枚举类型情况。

Switch statements without a default case can lead to unexpected behavior and incomplete handling of all possible cases. When a switch statement lacks a default case, if a value is encountered that does not match any of the specified cases, the program will continue execution without any defined behavior or handling.

没有 default 分支的 switch 语句可能会导致意外行为，并且无法完整处理所有可能的情况。当 switch 语句缺少 default 分支时，如果遇到一个不匹配任何已指定 case 的值，程序将继续执行，但不会有明确的行为或处理逻辑。

This check helps identify switch statements that are missing a default case, allowing developers to ensure that all possible cases are handled properly. Adding a default case allows for graceful handling of unexpected or unmatched values, reducing the risk of program errors and unexpected behavior.

此检查有助于识别缺少 default 分支的 switch 语句，使开发人员能够确保妥善处理所有可能的情况。添加 default 分支可以优雅地处理意外或未匹配的值，从而降低程序错误和意外行为的风险。

Example:

示例：

```c++
// Example 1:
// warning: switching on non-enum value without default case may not cover all cases
switch (i) {
case 0:
  break;
}

// 示例 1：
// 警告：在非枚举值上使用 switch 且没有 default 分支，可能无法覆盖所有情况
switch (i) {
case 0:
  break;
}

// Example 2:
enum E { eE1 };
E e = eE1;
switch (e) { // no-warning
case eE1:
  break;
}

// 示例 2：
enum E { eE1 };
E e = eE1;
switch (e) { // 无警告
case eE1:
  break;
}

// Example 3:
int i = 0;
switch (i) { // no-warning
case 0:
  break;
default:
  break;
}

// 示例 3：
int i = 0;
switch (i) { // 无警告
case 0:
  break;
default:
  break;
}
```

::: note

Enum types are already covered by compiler warnings (comes under -Wswitch) when a switch statement does not handle all enum values. This check focuses on non-enum types where the compiler warnings may not be present.

当 switch 语句未处理所有枚举值时，枚举类型已经由编译器警告（属于 -Wswitch）覆盖。此检查专注于编译器可能不会发出警告的非枚举类型。

:::

::: seealso
The CppCoreGuideline ES.79 provide guidelines on switch statements, including the recommendation to always provide a default case.

CppCoreGuideline ES.79 提供了关于 switch 语句的指导方针，其中包括始终提供 default 分支的建议。
:::
