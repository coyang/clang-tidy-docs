# readability-enum-initial-value

Enforces consistent style for enumerators' initialization, covering three styles: none, first only, or all initialized explicitly.

强制枚举值初始化风格一致，涵盖三种风格：全部不初始化、仅初始化第一个、或全部显式初始化。

An inconsistent style and strictness to defining the initializing value of enumerators may cause issues if the enumeration is extended with new enumerators that obtain their integer representation implicitly.

如果枚举值初始化风格不一致，或者对初始化值的定义不够严格，当枚举类型扩展新成员并隐式获取整数值时，可能会导致问题。

The following three cases are accepted:

以下三种情况是被接受的：

1.  No enumerators are explicit initialized.  
    所有枚举值都未显式初始化。

2.  Exactly the first enumerator is explicit initialized.  
    仅第一个枚举值被显式初始化。

3.  All enumerators are explicit initialized.  
    所有枚举值都被显式初始化。

```c++
enum A {    // (1) Valid, none of enumerators are initialized.
            // （1）合法，所有枚举值都未初始化。
  a0,
  a1,
  a2,
};

enum B {    // (2) Valid, the first enumerator is initialized.
            // （2）合法，仅第一个枚举值被初始化。
  b0 = 0,
  b1,
  b2,
};

enum C {    // (3) Valid, all of enumerators are initialized.
            // （3）合法，所有枚举值都被初始化。
  c0 = 0,
  c1 = 1,
  c2 = 2,
};

enum D {    // Invalid, d1 is not explicitly initialized!
            // 非法，d1 未被显式初始化！
  d0 = 0,
  d1,
  d2 = 2,
};

enum E {    // Invalid, e1, e3, and e5 are not explicitly initialized.
            // 非法，e1、e3 和 e5 未被显式初始化。
  e0 = 0,
  e1,
  e2 = 2,
  e3,       // Dangerous, as the numeric values of e3 and e5 are both 3, and this is not explicitly visible in the code!
            // 危险，因为 e3 和 e5 的数值都是 3，但在代码中并未显式体现！
  e4 = 2,
  e5,
};
```

This check corresponds to the CERT C Coding Standard recommendation INT09-C. Ensure enumeration constants map to unique values.

此检查对应于 CERT C 编码标准的建议：INT09-C. 确保枚举常量映射到唯一值。

cert-int09-c 重定向到此检查，作为其别名。

cert-int09-c 将作为此检查的别名进行重定向。

## Options

## 选项

::: option
AllowExplicitZeroFirstInitialValue

If set to false, the first enumerator must not be explicitly initialized to a literal 0. Default is true.

如果设置为 false，则第一个枚举值不能被显式初始化为字面值 0。默认值为 true。

```c++
enum F {
  f0 = 0, // Not allowed if AllowExplicitZeroFirstInitialValue is false.
          // 如果 AllowExplicitZeroFirstInitialValue 为 false，则此写法不被允许。
  f1,
  f2,
};
```

:::

::: option
AllowExplicitSequentialInitialValues

If set to false, explicit initialization to sequential values are not allowed. Default is true.

如果设置为 false，则不允许显式初始化为连续的数值。默认值为 true。

```c++
enum G {
  g0 = 1, // Not allowed if AllowExplicitSequentialInitialValues is false.
          // 如果 AllowExplicitSequentialInitialValues 为 false，则此写法不被允许。
  g1 = 2,
  g2 = 3,
```

:::
