# misc-const-correctness

# misc-const-correctness

This check implements detection of local variables which could be declared as const but are not. Declaring variables as const is required or recommended by many coding guidelines, such as: ES.25 from the C++ Core Guidelines.

此检查实现了对可以声明为 const 但未声明的局部变量的检测。根据许多编码规范（例如 C++ 核心指南中的 ES.25），建议或要求将变量声明为 const。

Please note that this check's analysis is type-based only. Variables that are not modified but used to create a non-const handle that might escape the scope are not diagnosed as potential const.

请注意，此检查仅基于类型分析。对于未被修改但用于创建可能逃逸作用域的非 const 句柄的变量，不会被诊断为潜在的 const。

```c++
// Declare a variable, which is not ``const`` ...
int i = 42;
// but use it as read-only. This means that `i` can be declared ``const``.
int result = i * i;       // Before transformation
int const result = i * i; // After transformation
```

```c++
// 声明一个变量，它不是 ``const`` ...
int i = 42;
// 但它被只读使用。这意味着 `i` 可以被声明为 ``const``。
int result = i * i;       // 转换前
int const result = i * i; // 转换后
```

The check can analyze values, pointers and references and pointees:

该检查可以分析值、指针、引用以及指针所指向的对象：

```c++
// Normal values like built-ins or objects.
int potential_const_int = 42;       // Before transformation
int const potential_const_int = 42; // After transformation
int copy_of_value = potential_const_int;

MyClass could_be_const;       // Before transformation
MyClass const could_be_const; // After transformation
could_be_const.const_qualified_method();

// References can be declared const as well.
int &reference_value = potential_const_int;       // Before transformation
int const& reference_value = potential_const_int; // After transformation
int another_copy = reference_value;

// The similar semantics of pointers are analyzed.
int *pointer_variable = &potential_const_int; // Before transformation
int const*const pointer_variable = &potential_const_int; // After transformation, both pointer itself and pointee are supported.
int last_copy = *pointer_variable;
```

```c++
// 普通值类型，如内建类型或对象。
int potential_const_int = 42;       // 转换前
int const potential_const_int = 42; // 转换后
int copy_of_value = potential_const_int;

MyClass could_be_const;       // 转换前
MyClass const could_be_const; // 转换后
could_be_const.const_qualified_method();

// 引用也可以声明为 const。
int &reference_value = potential_const_int;       // 转换前
int const& reference_value = potential_const_int; // 转换后
int another_copy = reference_value;

// 指针的类似语义也会被分析。
int *pointer_variable = &potential_const_int; // 转换前
int const*const pointer_variable = &potential_const_int; // 转换后，指针本身和指向对象都支持 const。
int last_copy = *pointer_variable;
```

The automatic code transformation is only applied to variables that are declared in single declarations. You may want to prepare your code base with readability-isolate-declaration first.

自动代码转换仅适用于单独声明的变量。您可能需要先使用 readability-isolate-declaration 准备您的代码库。

Note that there is the check cppcoreguidelines-avoid-non-const-global-variables to enforce const correctness on all globals.

请注意，还有一个检查项 cppcoreguidelines-avoid-non-const-global-variables 用于强制全局变量的 const 正确性。

## Known Limitations

## 已知限制

The check does not run on C code.

该检查不适用于 C 语言代码。

The check will not analyze templated variables or variables that are instantiation dependent. Different instantiations can result in different const correctness properties and in general it is not possible to find all instantiations of a template. The template might be used differently in an independent translation unit.

该检查不会分析模板变量或依赖实例化的变量。不同的实例化可能导致不同的 const 正确性属性，通常无法找到模板的所有实例化。模板可能在不同的翻译单元中以不同方式使用。

## Options

## 选项

::: option
AnalyzeValues

Enable or disable the analysis of ordinary value variables, like int i = 42;. Default is true.

启用或禁用对普通值变量（如 int i = 42;）的分析。默认值为 true。

```c++
// Warning
int i = 42;
// No warning
int const i = 42;

// Warning
int a[] = {42, 42, 42};
// No warning
int const a[] = {42, 42, 42};
```

```c++
// 警告
int i = 42;
// 无警告
int const i = 42;

// 警告
int a[] = {42, 42, 42};
// 无警告
int const a[] = {42, 42, 42};
```

:::

::: option
AnalyzeReferences

Enable or disable the analysis of reference variables, like int &ref = i;. Default is true.

启用或禁用对引用变量（如 int &ref = i;）的分析。默认值为 true。

```c++
int i = 42;
// Warning
int& ref = i;
// No warning
int const& ref = i;
```

```c++
int i = 42;
// 警告
int& ref = i;
// 无警告
int const& ref = i;
```

:::

::: option
AnalyzePointers

Enable or disable the analysis of pointers variables, like int \*ptr = &i;. For specific checks, see WarnPointersAsValues and WarnPointersAsPointers. Default is true.

启用或禁用对指针变量（如 int \*ptr = &i;）的分析。具体检查请参见 WarnPointersAsValues 和 WarnPointersAsPointers。默认值为 true。

:::

::: option
WarnPointersAsValues

This option enables the suggestion for const of the pointer itself. Pointer values have two possibilities to be const, the pointer and the value pointing to. Default is false.

此选项启用对指针本身添加 const 的建议。指针值有两个可以为 const 的部分：指针本身和其指向的值。默认值为 false。

```c++
int value = 42;

// Warning
const int * pointer_variable = &value;
// No warning
const int *const pointer_variable = &value;
```

```c++
int value = 42;

// 警告
const int * pointer_variable = &value;
// 无警告
const int *const pointer_variable = &value;
```

:::

::: option
WarnPointersAsPointers

This option enables the suggestion for const of the value pointing to. Default is true.

Requires AnalyzePointers to be true.

此选项启用对指针所指向值添加 const 的建议。默认值为 true。

需要 AnalyzePointers 设置为 true。

```c++
int value = 42;

// No warning
const int *const pointer_variable = &value;
// Warning
int *const pointer_variable = &value;
```

```c++
int value = 42;

// 无警告
const int *const pointer_variable = &value;
// 警告
int *const pointer_variable = &value;
```

:::

::: option
TransformValues

Provides fixit-hints for value types that automatically add const if its a single declaration. Default is true.

为值类型提供修复建议，如果是单独声明的变量，则自动添加 const。默认值为 true。

```c++
// Before
int value = 42;
// After
int const value = 42;

// Before
int a[] = {42, 42, 42};
// After
int const a[] = {42, 42, 42};

// Result is modified later in its life-time. No diagnostic and fixit hint will be emitted.
int result = value * 3;
result -= 10;
```

```c++
// 转换前
int value = 42;
// 转换后
int const value = 42;

// 转换前
int a[] = {42, 42, 42};
// 转换后
int const a[] = {42, 42, 42};

// 结果在生命周期中被修改。不会发出诊断或修复建议。
int result = value * 3;
result -= 10;
```

:::

::: option
TransformReferences

Provides fixit-hints for reference types that automatically add const if its a single declaration. Default is true.

为引用类型提供修复建议，如果是单独声明的变量，则自动添加 const。默认值为 true。

```c++
// This variable could still be a constant. But because there is a non-const reference to
// it, it can not be transformed (yet).
int value = 42;
// The reference 'ref_value' is not modified and can be made 'const int &ref_value = value;'
// Before
int &ref_value = value;
// After
int const &ref_value = value;

// Result is modified later in its life-time. No diagnostic and fixit hint will be emitted.
int result = ref_value * 3;
result -= 10;
```

```c++
// 此变量仍然可以是常量。但由于存在一个非 const 引用，
// 它暂时无法被转换。
int value = 42;
// 引用 'ref_value' 未被修改，可以改为 'const int &ref_value = value;'
// 转换前
int &ref_value = value;
// 转换后
int const &ref_value = value;

// 结果在生命周期中被修改。不会发出诊断或修复建议。
int result = ref_value * 3;
result -= 10;
```

:::

::: option
TransformPointersAsValues

Provides fixit-hints for pointers if their pointee is not changed. This does not analyze if the value-pointed-to is unchanged! Default is false.

Requires 'WarnPointersAsValues' to be 'true'.

为指针提供修复建议，如果其指向对象未被修改。注意：不会分析指向值是否被修改！默认值为 false。

需要将 'WarnPointersAsValues' 设置为 'true'。

```c++
int value = 42;

// Before
const int * pointer_variable = &value;
// After
const int *const pointer_variable = &value;

// Before
const int * a[] = {&value, &value};
// After
const int *const a[] = {&value, &value};

// Before
int *ptr_value = &value;
// After
int *const ptr_value = &value;

int result = 100 * (*ptr_value); // Does not modify the pointer itself.
// This modification of the pointee is still allowed and not diagnosed.
*ptr_value = 0;

// The following pointer may not become a 'int *const'.
int *changing_pointee = &value;
changing_pointee = &result;
```

```c++
int value = 42;

// 转换前
const int * pointer_variable = &value;
// 转换后
const int *const pointer_variable = &value;

// 转换前
const int * a[] = {&value, &value};
// 转换后
const int *const a[] = {&value, &value};

// 转换前
int *ptr_value = &value;
// 转换后
int *const ptr_value = &value;

int result = 100 * (*ptr_value); // 未修改指针本身。
// 对指向对象的修改仍然允许，不会被诊断。
*ptr_value = 0;

// 以下指针不能转换为 'int *const'。
int *changing_pointee = &value;
changing_pointee = &result;
```

:::

::: option
TransformPointersAsPointers

Provides fix-it hints for pointers if the value it pointing to is not changed. Default is false.

Requires WarnPointersAsPointers to be true.

为指针提供修复建议，如果其指向的值未被修改。默认值为 false。

需要 WarnPointersAsPointers 设置为 true。

```c++
int value = 42;

// Before
int * pointer_variable = &value;
// After
const int * pointer_variable = &value;

// Before
int * a[] = {&value, &value};
// After
const int * a[] = {&value, &value};
```

```c++
int value = 42;

// 转换前
int * pointer_variable = &value;
// 转换后
const int * pointer_variable = &value;

// 转换前
int * a[] = {&value, &value};
// 转换后
const int * a[] = {&value, &value};
```

:::

::: option
AllowedTypes

A semicolon-separated list of names of types that will be excluded from const-correctness checking. Regular expressions are accepted, e.g. [Rr]ef(erence)?$ matches every type with suffix Ref, ref, Reference and reference. If a name in the list contains the sequence ::, it is matched against the qualified type name (i.e. namespace::Type), otherwise it is matched against only the type name (i.e. Type). Default is empty string.

一个以分号分隔的类型名称列表，这些类型将被排除在 const 正确性检查之外。支持正则表达式，例如 [Rr]ef(erence)?$ 匹配所有以 Ref、ref、Reference 和 reference 结尾的类型。如果列表中的名称包含 ::，则会与限定类型名（如 namespace::Type）匹配，否则只与类型名（如 Type）匹配。默认值为空字符串。

:::
