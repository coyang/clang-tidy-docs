# bugprone-easily-swappable-parameters

Finds function definitions where parameters of convertible types follow each other directly, making call sites prone to calling the function with swapped (or badly ordered) arguments.

查找函数定义中相邻的参数类型可互相转换的情况，这可能导致在调用函数时容易出现参数顺序颠倒（或顺序错误）的问题。

```c++
void drawPoint(int X, int Y) { /* ... */ }
FILE *open(const char *Dir, const char *Name, Flags Mode) { /* ... */ }
```

A potential call like drawPoint(-2, 5) or openPath("a.txt", "tmp", Read) is perfectly legal from the language's perspective, but might not be what the developer of the function intended.

像 drawPoint(-2, 5) 或 openPath("a.txt", "tmp", Read) 这样的调用在语言层面上是合法的，但可能不是函数开发者所期望的调用方式。

More elaborate and type-safe constructs, such as opaque typedefs or strong types should be used instead, to prevent a mistaken order of arguments.

应使用更复杂且类型安全的结构，例如不透明的 typedef 或强类型，以防止参数顺序错误。

```c++
struct Coord2D { int X; int Y; };
void drawPoint(const Coord2D Pos) { /* ... */ }

FILE *open(const Path &Dir, const Filename &Name, Flags Mode) { /* ... */ }
```

Due to the potentially elaborate refactoring and API-breaking that is necessary to strengthen the type safety of a project, no automatic fix-its are offered.

由于增强项目类型安全性可能需要复杂的重构并破坏现有 API，因此本检查不会提供自动修复建议。

## Options

## 选项

### Extension/relaxation options

### 扩展/放宽选项

Relaxation (or extension) options can be used to broaden the scope of the analysis and fine-tune the enabling of more mixes between types. Some mixes may depend on coding style or preference specific to a project, however, it should be noted that enabling all of these relaxations model the way of mixing at call sites the most. These options are expected to make the check report for more functions, and report longer mixable ranges.

放宽（或扩展）选项可用于扩大分析范围，并微调不同类型之间混合的启用条件。一些混合可能取决于项目特定的编码风格或偏好，但需要注意的是，启用所有这些放宽选项将最全面地模拟调用点的混合方式。这些选项预计会使检查器报告更多函数，并报告更长的可混合参数序列。

::: option
QualifiersMix

Whether to consider parameters of some cvr-qualified T and a differently cvr-qualified T (i.e. T and const T, const T and volatile T, etc.) mixable between one another. If false, the check will consider differently qualified types unmixable. True turns the warnings on. Defaults to false.

是否将某些具有不同 CVR 限定符（const、volatile、restrict）的 T 类型参数视为可混合（例如 T 与 const T，const T 与 volatile T 等）。如果为 false，则检查器认为具有不同限定符的类型不可混合。设置为 true 将启用此类警告。默认值为 false。

The following example produces a diagnostic only if QualifiersMix is enabled:

以下示例仅在启用 QualifiersMix 时会产生诊断：

```c++
void *memcpy(const void *Destination, void *Source, std::size_t N) { /* ... */ }
```

:::

::: option
ModelImplicitConversions

Whether to consider parameters of type T and U mixable if there exists an implicit conversion from T to U and U to T. If false, the check will not consider implicitly convertible types for mixability. True turns warnings for implicit conversions on. Defaults to true.

是否在 T 和 U 类型之间存在隐式转换时将其视为可混合类型。如果为 false，则检查器不会将隐式可转换类型视为可混合。设置为 true 将启用对隐式转换的警告。默认值为 true。

The following examples produce a diagnostic only if ModelImplicitConversions is enabled:

以下示例仅在启用 ModelImplicitConversions 时会产生诊断：

```c++
void fun(int Int, double Double) { /* ... */ }
void compare(const char *CharBuf, std::string String) { /* ... */ }
```

::: note

Changing the qualifiers of an expression's type (e.g. from int to const int) is defined as an implicit conversion in the C++ Standard. However, the check separates this decision-making on the mixability of differently qualified types based on whether QualifiersMix was enabled.

根据 C++ 标准，更改表达式类型的限定符（例如从 int 到 const int）被视为隐式转换。然而，此检查器会根据是否启用了 QualifiersMix 来单独判断不同限定符类型是否可混合。

For example, the following code snippet will only produce a diagnostic if both QualifiersMix and ModelImplicitConversions are enabled:

例如，以下代码片段仅在同时启用 QualifiersMix 和 ModelImplicitConversions 时才会产生诊断：

```c++
void fun2(int Int, const double Double) { /* ... */ }
```

:::
:::

### Filtering options

### 过滤选项

Filtering options can be used to lessen the size of the diagnostics emitted by the checker, whether the aim is to ignore certain constructs or dampen the noisiness.

过滤选项可用于减少检查器发出的诊断数量，无论是为了忽略某些结构还是降低噪声。

::: option
MinimumLength

The minimum length required from an adjacent parameter sequence to be diagnosed. Defaults to 2. Might be any positive integer greater or equal to 2. If 0 or 1 is given, the default value 2 will be used instead.

被诊断的相邻参数序列所需的最小长度。默认值为 2。可以是大于或等于 2 的任意正整数。如果设置为 0 或 1，则将使用默认值 2。

For example, if 3 is specified, the examples above will not be matched.

例如，如果设置为 3，则上述示例将不会被匹配。

:::

::: option
IgnoredParameterNames

The list of parameter names that should never be considered part of a swappable adjacent parameter sequence. The value is a ;-separated list of names. To ignore unnamed parameters, add "" to the list verbatim (not the empty string, but the two quotes, potentially escaped!). This option is case-sensitive!

不应被视为可交换相邻参数序列一部分的参数名称列表。该值是以分号 ; 分隔的名称列表。要忽略未命名参数，请将 "" 原样添加到列表中（不是空字符串，而是两个引号，可能需要转义！）。此选项区分大小写！

By default, the following parameter names, and their Uppercase-initial variants are ignored: "", iterator, begin, end, first, last, lhs, rhs.

默认情况下，以下参数名称及其首字母大写的变体将被忽略：""（未命名参数）、iterator、begin、end、first、last、lhs、rhs。

:::

::: option
IgnoredParameterTypeSuffixes

The list of parameter type name suffixes that should never be considered part of a swappable adjacent parameter sequence. Parameters which type, as written in the source code, end with an element of this option will be ignored. The value is a ;-separated list of names. This option is case-sensitive!

不应被视为可交换相邻参数序列一部分的参数类型名称后缀列表。源代码中类型名称以该选项中的某个后缀结尾的参数将被忽略。该值是以分号 ; 分隔的名称列表。此选项区分大小写！

By default, the following, and their lowercase-initial variants are ignored: bool, It, Iterator, InputIt, ForwardIt, BidirIt, RandomIt, random_iterator, ReverseIt, reverse_iterator, reverse_const_iterator, Const_Iterator, ConstIterator, const_reverse_iterator, ConstReverseIterator. In addition, \_Bool (but not \_bool) is also part of the default value.

默认情况下，以下后缀及其首字母小写的变体将被忽略：bool、It、Iterator、InputIt、ForwardIt、BidirIt、RandomIt、random_iterator、ReverseIt、reverse_iterator、reverse_const_iterator、Const_Iterator、ConstIterator、const_reverse_iterator、ConstReverseIterator。此外，\_Bool（但不是 \_bool）也包含在默认值中。

:::

::: option
SuppressParametersUsedTogether

Suppresses diagnostics about parameters that are used together or in a similar fashion inside the function's body. Defaults to true. Specifying false will turn off the heuristics.

抑制对在函数体中一起使用或以相似方式使用的参数的诊断。默认值为 true。设置为 false 将关闭此启发式规则。

Currently, the following heuristics are implemented which will suppress the warning about the parameter pair involved:

当前实现了以下启发式规则，会抑制相关参数对的警告：

- The parameters are used in the same expression, e.g. f(a, b) or a < b.

  参数在同一表达式中使用，例如 f(a, b) 或 a < b。

- The parameters are further passed to the same function to the same parameter of that function, of the same overload. E.g. f(a, 1) and f(b, 2) to some f(T, int).

  参数被传递给同一个函数的相同参数位置，且为相同重载。例如 f(a, 1) 和 f(b, 2) 被传递给某个 f(T, int)。

  ::: note
  ::: title
  Note
  :::

  The check does not perform path-sensitive analysis, and as such, "same function" in this context means the same function declaration. If the same member function of a type on two distinct instances are called with the parameters, it will still be regarded as "same function".

  此检查不执行路径敏感分析，因此此处的“同一个函数”指的是相同的函数声明。如果两个不同实例调用了同一个成员函数，也会被视为“同一个函数”。

  :::

- The same member field is accessed, or member method is called of the two parameters, e.g. a.foo() and b.foo().

  两个参数访问相同的成员字段或调用相同的成员方法，例如 a.foo() 和 b.foo()。

- Separate return statements return either of the parameters on different code paths.

  在不同代码路径中分别通过 return 语句返回这两个参数之一。

:::

::: option
NamePrefixSuffixSilenceDissimilarityTreshold

The number of characters two parameter names might be different on either the head or the tail end with the rest of the name the same so that the warning about the two parameters are silenced. Defaults to 1. Might be any positive integer. If 0, the filtering heuristic based on the parameters' names is turned off.

两个参数名称在前缀或后缀中最多可以有多少个字符不同（其余部分相同），以便抑制对这两个参数的警告。默认值为 1。可以是任意正整数。如果为 0，则关闭基于参数名称的过滤启发式规则。

This option can be used to silence warnings about parameters where the naming scheme indicates that the order of those parameters do not matter.

此选项可用于抑制那些通过命名方式表明参数顺序无关紧要的警告。

For example, the parameters LHS and RHS are 1-dissimilar suffixes of each other: L and R is the different character, while HS is the common suffix. Similarly, parameters text1, text2, text3 are 1-dissimilar prefixes of each other, with the numbers at the end being the dissimilar part. If the value is at least 1, such cases will not be reported.

例如，参数 LHS 和 RHS 是后缀仅相差 1 个字符的情况：L 和 R 是不同字符，而 HS 是共同后缀。类似地，参数 text1、text2、text3 是前缀仅相差 1 个字符的情况，末尾的数字是不同部分。如果该值至少为 1，则此类情况不会被报告。

:::

## Limitations

## 限制

This check is designed to check function signatures!

此检查器专为检查函数签名而设计！

The check does not investigate functions that are generated by the compiler in a context that is only determined from a call site. These cases include variadic functions, functions in C code that do not have an argument list, and C++ template instantiations. Most of these cases, which are otherwise swappable from a caller's standpoint, have no way of getting "fixed" at the definition point. In the case of C++ templates, only primary template definitions and explicit specializations are matched and analyzed.

该检查不会分析那些仅在调用点由编译器生成的函数。这些情况包括变参函数、C 语言中没有参数列表的函数，以及 C++ 模板实例化。尽管从调用者角度来看这些函数的参数可能是可交换的，但在定义点无法“修复”。对于 C++ 模板，仅分析主模板定义和显式特化。

None of the following cases produce a diagnostic:

以下情况不会产生诊断：

```c++
int printf(const char *Format, ...) { /* ... */ }
int someOldCFunction() { /* ... */ }

template <typename T, typename U>
int add(T X, U Y) { return X + Y };

void theseAreNotWarnedAbout() {
    printf("%d %d\n", 1, 2);   // Two ints passed, they could be swapped.
    someOldCFunction(1, 2, 3); // Similarly, multiple ints passed.

    add(1, 2); // Instantiates 'add<int, int>', but that's not a user-defined function.
}
```

Due to the limitation above, parameters which type are further dependent upon template instantiations to prove that they mix with another parameter's is not diagnosed.

由于上述限制，参数类型依赖于模板实例化以证明其与另一个参数类型可混合的情况将不会被诊断。

```c++
template <typename T>
struct Vector {
  typedef T element_type;
};

// Diagnosed: Explicit instantiation was done by the user, we can prove it
// is the same type.
void instantiated(int A, Vector<int>::element_type B) { /* ... */ }

// Diagnosed: The two parameter types are exactly the same.
template <typename T>
void exact(typename Vector<T>::element_type A,
           typename Vector<T>::element_type B) { /* ... */ }

// Skipped: The two parameters are both 'T' but we cannot prove this
// without actually instantiating.
template <typename T>
void falseNegative(T A, typename Vector<T>::element_type B) { /* ... */ }
```

In the context of implicit conversions (when ModelImplicitConversions is enabled), the modelling performed by the check warns if the parameters are swappable and the swapped order matches implicit conversions. It does not model whether there exists an unrelated third type from which both parameters can be given in a function call. This means that in the following example, even while strs() clearly carries the possibility to be called with swapped arguments (as long as the arguments are string literals), will not be warned about.

在启用 ModelImplicitConversions 时，检查器会在参数可交换且交换顺序符合隐式转换规则时发出警告。但它不会建模是否存在一个无关的第三种类型，可以用于提供两个参数。这意味着在以下示例中，即使 strs() 明显可能被交换参数调用（只要参数是字符串字面量），也不会发出警告。

```c++
struct String {
    String(const char *Buf);
};

struct StringView {
    StringView(const char *Buf);
    operator const char *() const;
};

// Skipped: Directly swapping expressions of the two type cannot mix.
// (Note: StringView -> const char * -> String would be two
// user-defined conversions, which is disallowed by the language.)
void strs(String Str, StringView SV) { /* ... */ }

// Diagnosed: StringView implicitly converts to and from a buffer.
void cStr(StringView SV, const char *Buf) { /* ... */ }
```
