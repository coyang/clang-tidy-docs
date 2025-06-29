# cppcoreguidelines-owning-memory

This check implements the type-based semantics of gsl::owner<T\*>, which allows static analysis on code, that uses raw pointers to handle resources like dynamic memory, but won't introduce RAII concepts.

此检查实现了 gsl::owner<T\*> 的基于类型的语义，允许对使用原始指针处理资源（如动态内存）的代码进行静态分析，但不会引入 RAII（资源获取即初始化）概念。

This check implements I.11, C.33, R.3 and GSL.Views from the C++ Core Guidelines. The definition of a gsl::owner<T\*> is straight forward

此检查实现了 C++ 核心指南中的 I.11、C.33、R.3 和 GSL.Views。gsl::owner<T\*> 的定义非常直接：

```c++
namespace gsl { template <typename T> owner = T; }
```

It is therefore simple to introduce the owner even without using an implementation of the Guideline Support Library.

因此，即使不使用指南支持库（Guideline Support Library）的实现，也可以轻松引入 owner。

All checks are purely type based and not (yet) flow sensitive.

所有检查都仅基于类型，目前尚不支持流程敏感分析。

The following examples will demonstrate the correct and incorrect initializations of owners, assignment is handled the same way. Note that both new and malloc()-like resource functions are considered to produce resources.

以下示例将演示 owner 的正确和错误初始化方式，赋值操作的处理方式相同。请注意，new 和类似 malloc() 的资源函数都被视为生成资源。

```c++
// Creating an owner with factory functions is checked.
gsl::owner<int*> function_that_returns_owner() { return gsl::owner<int*>(new int(42)); }

// Dynamic memory must be assigned to an owner
int* Something = new int(42); // BAD, will be caught
gsl::owner<int*> Owner = new int(42); // Good
gsl::owner<int*> Owner = new int[42]; // Good as well

// Returned owner must be assigned to an owner
int* Something = function_that_returns_owner(); // Bad, factory function
gsl::owner<int*> Owner = function_that_returns_owner(); // Good, result lands in owner

// Something not a resource or owner should not be assigned to owners
int Stack = 42;
gsl::owner<int*> Owned = &Stack; // Bad, not a resource assigned
```

```c++
// 使用工厂函数创建 owner 会被检查。
gsl::owner<int*> function_that_returns_owner() { return gsl::owner<int*>(new int(42)); }

// 动态内存必须赋值给 owner
int* Something = new int(42); // 错误，将被检测出
gsl::owner<int*> Owner = new int(42); // 正确
gsl::owner<int*> Owner = new int[42]; // 也正确

// 返回的 owner 必须赋值给 owner
int* Something = function_that_returns_owner(); // 错误，工厂函数返回值未赋给 owner
gsl::owner<int*> Owner = function_that_returns_owner(); // 正确，结果赋给了 owner

// 非资源或非 owner 的对象不应赋值给 owner
int Stack = 42;
gsl::owner<int*> Owned = &Stack; // 错误，非资源被赋值
```

In the case of dynamic memory as resource, only gsl::owner<T\*> variables are allowed to be deleted.

当动态内存作为资源时，仅允许删除 gsl::owner<T\*> 类型的变量。

```c++
// Example Bad, non-owner as resource handle, will be caught.
int* NonOwner = new int(42); // First warning here, since new must land in an owner
delete NonOwner; // Second warning here, since only owners are allowed to be deleted

// Example Good, Ownership correctly stated
gsl::owner<int*> Owner = new int(42); // Good
delete Owner; // Good as well, statically enforced, that only owners get deleted
```

```c++
// 错误示例，非 owner 作为资源句柄，将被检测出。
int* NonOwner = new int(42); // 第一个警告，因为 new 必须赋值给 owner
delete NonOwner; // 第二个警告，因为只有 owner 才能被 delete

// 正确示例，正确声明了所有权
gsl::owner<int*> Owner = new int(42); // 正确
delete Owner; // 也正确，静态检查确保只有 owner 被 delete
```

The check will furthermore ensure, that functions, that expect a gsl::owner<T*> as argument get called with either a gsl::owner<T*> or a newly created resource.

该检查还将确保，期望 gsl::owner<T*> 参数的函数只能被 gsl::owner<T*> 或新创建的资源调用。

```c++
void expects_owner(gsl::owner<int*> o) { delete o; }

// Bad Code
int NonOwner = 42;
expects_owner(&NonOwner); // Bad, will get caught

// Good Code
gsl::owner<int*> Owner = new int(42);
expects_owner(Owner); // Good
expects_owner(new int(42)); // Good as well, recognized created resource

// Port legacy code for better resource-safety
gsl::owner<FILE*> File = fopen("my_file.txt", "rw+");
FILE* BadFile = fopen("another_file.txt", "w"); // Bad, warned

// ... use the file

fclose(File); // Ok, File is annotated as 'owner<>'
fclose(BadFile); // BadFile is not an 'owner<>', will be warned
```

```c++
void expects_owner(gsl::owner<int*> o) { delete o; }

// 错误代码
int NonOwner = 42;
expects_owner(&NonOwner); // 错误，将被检测出

// 正确代码
gsl::owner<int*> Owner = new int(42);
expects_owner(Owner); // 正确
expects_owner(new int(42)); // 也正确，识别为新创建的资源

// 移植旧代码以提高资源安全性
gsl::owner<FILE*> File = fopen("my_file.txt", "rw+");
FILE* BadFile = fopen("another_file.txt", "w"); // 错误，将被警告

// ... 使用文件

fclose(File); // 正确，File 被标注为 'owner<>'
fclose(BadFile); // 错误，BadFile 不是 'owner<>', 将被警告
```

## Options

## 选项

::: option
LegacyResourceProducers

Semicolon-separated list of fully qualified names of legacy functions that create resources but cannot introduce gsl::owner<>. Defaults to ::malloc;::aligned_alloc;::realloc;::calloc;::fopen;::freopen;::tmpfile.

用分号分隔的旧函数全限定名列表，这些函数创建资源但无法引入 gsl::owner<>。默认值为 ::malloc;::aligned_alloc;::realloc;::calloc;::fopen;::freopen;::tmpfile。

:::

::: option
LegacyResourceConsumers

Semicolon-separated list of fully qualified names of legacy functions expecting resource owners as pointer arguments but cannot introduce gsl::owner<>. Defaults to ::free;::realloc;::freopen;::fclose.

用分号分隔的旧函数全限定名列表，这些函数期望资源所有者作为指针参数，但无法引入 gsl::owner<>。默认值为 ::free;::realloc;::freopen;::fclose。

:::

## Limitations

## 限制

Using gsl::owner<T\*> in a typedef or alias is not handled correctly.

在 typedef 或别名中使用 gsl::owner<T\*> 无法被正确处理。

```c++
using heap_int = gsl::owner<int*>;
heap_int allocated = new int(42); // False positive!
```

```c++
using heap_int = gsl::owner<int*>;
heap_int allocated = new int(42); // 误报！
```

The gsl::owner<T*> is declared as a templated type alias. In template functions and classes, like in the example below, the information of the type aliases gets lost. Therefore using gsl::owner<T*> in a heavy templated code base might lead to false positives.

gsl::owner<T*> 被声明为模板类型别名。在模板函数和类中（如下例所示），类型别名信息会丢失。因此，在大量使用模板的代码中使用 gsl::owner<T*> 可能会导致误报。

Known code constructs that do not get diagnosed correctly are:

已知无法被正确诊断的代码结构包括：

- std::exchange
- std::vector<gsl::owner<T\*>>

```c++
// This template function works as expected. Type information doesn't get lost.
template <typename T>
void delete_owner(gsl::owner<T*> owned_object) {
  delete owned_object; // Everything alright
}

gsl::owner<int*> function_that_returns_owner() { return gsl::owner<int*>(new int(42)); }

// Type deduction does not work for auto variables.
// This is caught by the check and will be noted accordingly.
auto OwnedObject = function_that_returns_owner(); // Type of OwnedObject will be int*

// Problematic function template that looses the typeinformation on owner
template <typename T>
void bad_template_function(T some_object) {
  // This line will trigger the warning, that a non-owner is assigned to an owner
  gsl::owner<T*> new_owner = some_object;
}

// Calling the function with an owner still yields a false positive.
bad_template_function(gsl::owner<int*>(new int(42)));


// The same issue occurs with templated classes like the following.
template <typename T>
class OwnedValue {
public:
  const T getValue() const { return _val; }
private:
  T _val;
};

// Code, that yields a false positive.
OwnedValue<gsl::owner<int*>> Owner(new int(42)); // Type deduction yield T -> int *
// False positive, getValue returns int* and not gsl::owner<int*>
gsl::owner<int*> OwnedInt = Owner.getValue();
```

```c++
// 此模板函数按预期工作，类型信息未丢失。
template <typename T>
void delete_owner(gsl::owner<T*> owned_object) {
  delete owned_object; // 一切正常
}

gsl::owner<int*> function_that_returns_owner() { return gsl::owner<int*>(new int(42)); }

// auto 变量无法进行类型推导。
// 此问题会被检查器捕获并记录。
auto OwnedObject = function_that_returns_owner(); // OwnedObject 的类型将是 int*

// 有问题的模板函数，会丢失 owner 的类型信息
template <typename T>
void bad_template_function(T some_object) {
  // 此行将触发警告：非 owner 被赋值给 owner
  gsl::owner<T*> new_owner = some_object;
}

// 即使传入的是 owner，调用该函数仍会产生误报。
bad_template_function(gsl::owner<int*>(new int(42)));


// 以下模板类也会出现相同问题。
template <typename T>
class OwnedValue {
public:
  const T getValue() const { return _val; }
private:
  T _val;
};

// 以下代码会产生误报。
OwnedValue<gsl::owner<int*>> Owner(new int(42)); // 类型推导为 T -> int *
// 误报，getValue 返回的是 int* 而不是 gsl::owner<int*>
gsl::owner<int*> OwnedInt = Owner.getValue();
```

Another limitation of the current implementation is only the type based checking. Suppose you have code like the following:

当前实现的另一个限制是仅基于类型的检查。假设你有如下代码：

```c++
// Two owners with assigned resources
gsl::owner<int*> Owner1 = new int(42);
gsl::owner<int*> Owner2 = new int(42);

Owner2 = Owner1; // Conceptual Leak of initial resource of Owner2!
Owner1 = nullptr;
```

```c++
// 两个 owner 拥有各自的资源
gsl::owner<int*> Owner1 = new int(42);
gsl::owner<int*> Owner2 = new int(42);

Owner2 = Owner1; // 概念上的资源泄漏：Owner2 原始资源未释放！
Owner1 = nullptr;
```

The semantic of a gsl::owner<T*> is mostly like a std::unique_ptr<T>, therefore assignment of two gsl::owner<T*> is considered a move, which requires that the resource Owner2 must have been released before the assignment. This kind of condition could be caught in later improvements of this check with flowsensitive analysis. Currently, the Clang Static Analyzer catches this bug for dynamic memory, but not for general types of resources.

gsl::owner<T*> 的语义大致类似于 std::unique_ptr<T>，因此两个 gsl::owner<T*> 之间的赋值被视为移动操作，这要求在赋值前必须释放 Owner2 的资源。这类问题可以通过未来引入流程敏感分析来捕获。目前，Clang 静态分析器可以检测到动态内存的此类错误，但无法检测一般类型的资源。
