# bugprone-suspicious-enum-usage

The checker detects various cases when an enum is probably misused (as a bitmask).

该检查器会检测枚举类型被误用（作为位掩码）的各种情况。

1. When "ADD" or "bitwise OR" is used between two enum which come from different types and these types value ranges are not disjoint.

1. 当两个来自不同类型的枚举之间使用“加法”或“按位或”操作，并且这些类型的取值范围不互斥时。

The following cases will be investigated only using StrictMode. We regard the enum as a (suspicious) bitmask if the three conditions below are true at the same time:

以下情况仅在启用 StrictMode 时进行检查。如果同时满足以下三个条件，我们将认为该枚举是一个（可疑的）位掩码：

- at most half of the elements of the enum are non pow-of-2 numbers (because of short enumerations)
  - 枚举中最多只有一半的元素不是 2 的幂（由于枚举较短）

- there is another non pow-of-2 number than the enum constant representing all choices (the result "bitwise OR" operation of all enum elements)
  - 除了表示所有选项的枚举常量（即所有枚举元素按位或的结果）外，还有其他不是 2 的幂的数字

- enum type variable/enumconstant is used as an argument of a + or "bitwise OR" operator
  - 枚举类型变量或枚举常量被用作 + 或 “按位或” 操作符的参数

So whenever the non pow-of-2 element is used as a bitmask element we diagnose a misuse and give a warning.

因此，每当一个非 2 的幂的元素被用作位掩码元素时，我们会诊断为误用并发出警告。

2. Investigating the right hand side of += and |= operator.
3. 检查 += 和 |= 操作符右侧的表达式。

4. Check only the enum value side of a | and + operator if one of them is not enum val.
5. 如果 | 或 + 操作符的一侧不是枚举值，则仅检查枚举值一侧。

6. Check both side of | or + operator where the enum values are from the same enum type.
7. 如果 | 或 + 操作符两侧的枚举值来自相同的枚举类型，则检查两侧。

Examples:

示例：

```c++
enum { A, B, C };
enum { D, E, F = 5 };
enum { G = 10, H = 11, I = 12 };

unsigned flag;
flag =
    A |
    H; // OK, disjoint value intervals in the enum types ->probably good use.
        // 正确，不同枚举类型的值区间不重叠 -> 可能是合理用法。
flag = B | F; // Warning, have common values so they are probably misused.
              // 警告，存在相同的值，因此可能被误用。

// Case 2:
enum Bitmask {
  A = 0,
  B = 1,
  C = 2,
  D = 4,
  E = 8,
  F = 16,
  G = 31 // OK, real bitmask.
         // 正确，是真正的位掩码。
};

enum Almostbitmask {
  AA = 0,
  BB = 1,
  CC = 2,
  DD = 4,
  EE = 8,
  FF = 16,
  GG // Problem, forgot to initialize.
     // 问题，忘记初始化。
};

unsigned flag = 0;
flag |= E; // OK.
           // 正确。
flag |=
    EE; // Warning at the decl, and note that it was used here as a bitmask.
        // 声明时警告，并注意此处将其作为位掩码使用。
```

## Options

## 选项

::: option
StrictMode

Default value: 0. When non-null the suspicious bitmask usage will be investigated additionally to the different enum usage check.

默认值：0。当该选项非空时，除了检查不同枚举类型的使用外，还会额外检查可疑的位掩码用法。
:::
