# readability-implicit-bool-conversion

This check can be used to find implicit conversions between built-in types and booleans. Depending on use case, it may simply help with readability of the code, or in some cases, point to potential bugs which remain unnoticed due to implicit conversions.

该检查可用于查找内建类型与布尔类型之间的隐式转换。根据使用场景，它可能仅有助于提高代码的可读性，或者在某些情况下指出由于隐式转换而未被注意到的潜在错误。

The following is a real-world example of bug which was hiding behind implicit bool conversion:

以下是一个隐藏在隐式 bool 转换背后的真实错误示例：

```c++
class Foo {
  int m_foo;

public:
  void setFoo(bool foo) { m_foo = foo; } // warning: implicit conversion bool -> int
  int getFoo() { return m_foo; }
};

void use(Foo& foo) {
  bool value = foo.getFoo(); // warning: implicit conversion int -> bool
}
```

This code is the result of unsuccessful refactoring, where type of m_foo changed from bool to int. The programmer forgot to change all occurrences of bool, and the remaining code is no longer correct, yet it still compiles without any visible warnings.

这段代码是一次不成功的重构的结果，其中 m_foo 的类型从 bool 更改为 int。程序员忘记更改所有 bool 的使用位置，导致剩余代码不再正确，但仍然可以编译且没有任何明显的警告。

In addition to issuing warnings, fix-it hints are provided to help solve the reported issues. This can be used for improving readability of code, for example:

除了发出警告外，还提供了修复建议，以帮助解决报告的问题。这也可用于提高代码的可读性，例如：

```c++
void conversionsToBool() {
  float floating;
  bool boolean = floating;
  // ^ propose replacement: bool boolean = floating != 0.0f;

  int integer;
  if (integer) {}
  // ^ propose replacement: if (integer != 0) {}

  int* pointer;
  if (!pointer) {}
  // ^ propose replacement: if (pointer == nullptr) {}

  while (1) {}
  // ^ propose replacement: while (true) {}
}

void functionTakingInt(int param);

void conversionsFromBool() {
  bool boolean;
  functionTakingInt(boolean);
  // ^ propose replacement: functionTakingInt(static_cast<int>(boolean));

  functionTakingInt(true);
  // ^ propose replacement: functionTakingInt(1);
}
```

In general, the following conversion types are checked:

通常会检查以下类型的转换：

- integer expression/literal to boolean (conversion from a single bit bitfield to boolean is explicitly allowed, since there's no ambiguity / information loss in this case),
- 整型表达式/字面值转换为布尔类型（从单个位字段转换为布尔类型是明确允许的，因为在这种情况下没有歧义或信息丢失），

- floating expression/literal to boolean,
- 浮点表达式/字面值转换为布尔类型，

- pointer/pointer to member/nullptr/NULL to boolean,
- 指针/成员指针/nullptr/NULL 转换为布尔类型，

- boolean expression/literal to integer (conversion from boolean to a single bit bitfield is explicitly allowed),
- 布尔表达式/字面值转换为整型（从布尔类型转换为单个位字段是明确允许的），

- boolean expression/literal to floating.
- 布尔表达式/字面值转换为浮点类型。

The rules for generating fix-it hints are:

生成修复建议的规则如下：

- in case of conversions from other built-in type to bool, an explicit comparison is proposed to make it clear what exactly is being compared:
  - bool boolean = floating; is changed to bool boolean = floating == 0.0f;
  - for other types, appropriate literals are used (0, 0u, 0.0f, 0.0, nullptr),
- 当从其他内建类型转换为 bool 时，建议使用显式比较，以明确比较的内容：
  - bool boolean = floating; 被替换为 bool boolean = floating == 0.0f;
  - 对于其他类型，使用适当的字面值（0、0u、0.0f、0.0、nullptr），

- in case of negated expressions conversion to bool, the proposed replacement with comparison is simplified:
  - if (!pointer) is changed to if (pointer == nullptr),
- 对于取反表达式转换为 bool 的情况，建议的替换使用简化的比较方式：
  - if (!pointer) 被替换为 if (pointer == nullptr)，

- in case of conversions from bool to other built-in types, an explicit static_cast (or a C-style cast since C23) is proposed to make it clear that a conversion is taking place:
  - int integer = boolean; is changed to int integer = static_cast<int>(boolean);
- 当从 bool 转换为其他内建类型时，建议使用显式的 static_cast（或自 C23 起使用 C 风格转换），以明确正在进行类型转换：
  - int integer = boolean; 被替换为 int integer = static_cast<int>(boolean);

- if the conversion is performed on type literals, an equivalent literal is proposed, according to what type is actually expected, for example:
  - functionTakingBool(0); is changed to functionTakingBool(false);
  - functionTakingInt(true); is changed to functionTakingInt(1);
  - for other types, appropriate literals are used (false, true, 0, 1, 0u, 1u, 0.0f, 1.0f, 0.0, 1.0f).
- 如果转换发生在类型字面值上，则根据实际期望的类型，建议使用等效的字面值，例如：
  - functionTakingBool(0); 被替换为 functionTakingBool(false);
  - functionTakingInt(true); 被替换为 functionTakingInt(1);
  - 对于其他类型，使用适当的字面值（false、true、0、1、0u、1u、0.0f、1.0f、0.0、1.0f）。

Some additional accommodations are made for pre-C++11 dialects:

对于 C++11 之前的方言，还做了一些额外的适配：

- false literal conversion to pointer is detected,
- 检测 false 字面值转换为指针的情况，

- instead of nullptr literal, 0 is proposed as replacement.
- 建议使用 0 替代 nullptr 字面值。

Occurrences of implicit conversions inside macros and template instantiations are deliberately ignored, as it is not clear how to deal with such cases.

宏和模板实例化中的隐式转换会被有意忽略，因为目前尚不清楚如何处理这类情况。

## Options

选项

::: option
AllowIntegerConditions

When true, the check will allow conditional integer conversions. Default is false.

当为 true 时，该检查将允许条件中的整型转换。默认值为 false。
:::

::: option
AllowPointerConditions

When true, the check will allow conditional pointer conversions. Default is false.

当为 true 时，该检查将允许条件中的指针转换。默认值为 false。
:::

::: option
UseUpperCaseLiteralSuffix

When true, the replacements will use an uppercase literal suffix in the provided fixes. Default is false.

当为 true 时，修复建议中将使用大写字面值后缀。默认值为 false。

> Example
>
> 示例
>
> ```c++
> uint32_t foo;
> if (foo) {}
> // ^ propose replacement default: if (foo != 0u) {}
> // ^ propose replacement with option UseUpperCaseLiteralSuffix: if (foo != 0U) {}
> ```
