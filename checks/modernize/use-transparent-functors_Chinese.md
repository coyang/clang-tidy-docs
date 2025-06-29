# modernize-use-transparent-functors

Prefer transparent functors to non-transparent ones. When using transparent functors, the type does not need to be repeated. The code is easier to read, maintain and less prone to errors. It is not possible to introduce unwanted conversions.

优先使用透明函子而非非透明函子。使用透明函子时，无需重复指定类型，代码更易于阅读和维护，且更不易出错。这样也避免了引入不必要的类型转换。

```c++
// Non-transparent functor
std::map<int, std::string, std::greater<int>> s;

// 非透明函子
std::map<int, std::string, std::greater<int>> s;

// Transparent functor.
std::map<int, std::string, std::greater<>> s;

// 透明函子
std::map<int, std::string, std::greater<>> s;

// Non-transparent functor
using MyFunctor = std::less<MyType>;

// 非透明函子
using MyFunctor = std::less<MyType>;
```

It is not always a safe transformation though. The following case will be untouched to preserve the semantics.

不过，这并不总是一个安全的转换。如下示例将保持不变，以保留原有语义。

```c++
// Non-transparent functor
std::map<const char *, std::string, std::greater<std::string>> s;

// 非透明函子
std::map<const char *, std::string, std::greater<std::string>> s;
```

## Options

## 选项

::: option
SafeMode

If the option is set to true, the check will not diagnose cases where using a transparent functor cannot be guaranteed to produce identical results as the original code. The default value for this option is false.

SafeMode

如果该选项设置为 true，则在无法保证使用透明函子与原始代码产生相同结果的情况下，检查器不会发出诊断。该选项的默认值为 false。
:::

This check requires using C++14 or higher to run.

此检查要求使用 C++14 或更高版本。
