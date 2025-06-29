# modernize-pass-by-value

With move semantics added to the language and the standard library updated with move constructors added for many types it is now interesting to take an argument directly by value, instead of by const-reference, and then copy. This check allows the compiler to take care of choosing the best way to construct the copy.

随着语言中引入了移动语义，并且标准库中为许多类型添加了移动构造函数，现在直接按值传递参数（而不是按 const 引用传递然后再复制）变得更有意义。此检查允许编译器选择构造副本的最佳方式。

The transformation is usually beneficial when the calling code passes an rvalue and assumes the move construction is a cheap operation. This short example illustrates how the construction of the value happens:

当调用代码传递一个右值并假设移动构造是廉价操作时，此转换通常是有益的。以下简短示例说明了值的构造方式：

```c++
void foo(std::string s);
std::string get_str();

void f(const std::string &str) {
  foo(str);       // lvalue  -> copy construction
  foo(get_str()); // prvalue -> move construction
}
```

::: note

Currently, only constructors are transformed to make use of pass-by-value. Contributions that handle other situations are welcome!

目前，仅构造函数会被转换为使用按值传递。欢迎贡献处理其他情况的实现！

:::

## Pass-by-value in constructors

Replaces the uses of const-references constructor parameters that are copied into class fields. The parameter is then moved with std::move().

将构造函数中被复制到类字段中的 const 引用参数替换为按值传递。然后使用 std::move() 移动该参数。

Since std::move() is a library function declared in <utility> it may be necessary to add this include. The check will add the include directive when necessary.

由于 std::move() 是在 <utility> 中声明的库函数，因此可能需要添加该头文件。此检查将在必要时添加该 include 指令。

```c++
#include <string>

class Foo {
public:
-  Foo(const std::string &Copied, const std::string &ReadOnly)
-    : Copied(Copied), ReadOnly(ReadOnly)
+  Foo(std::string Copied, const std::string &ReadOnly)
+    : Copied(std::move(Copied)), ReadOnly(ReadOnly)
  {}

private:
  std::string Copied;
  const std::string &ReadOnly;
};

std::string get_cwd();

void f(const std::string &Path) {
  // The parameter corresponding to 'get_cwd()' is move-constructed. By
  // using pass-by-value in the Foo constructor we managed to avoid a
  // copy-construction.
  Foo foo(get_cwd(), Path);
}
```

// get_cwd() 对应的参数是通过移动构造的。通过在 Foo 构造函数中使用按值传递，我们成功避免了一次复制构造。

If the parameter is used more than once no transformation is performed since moved objects have an undefined state. It means the following code will be left untouched:

如果参数被使用多次，则不会进行转换，因为被移动的对象处于未定义状态。这意味着以下代码将保持不变：

```c++
#include <string>

void pass(const std::string &S);

struct Foo {
  Foo(const std::string &S) : Str(S) {
    pass(S);
  }

  std::string Str;
};
```

### Known limitations

A situation where the generated code can be wrong is when the object referenced is modified before the assignment in the init-list through a "hidden" reference.

生成的代码可能出错的一种情况是：在初始化列表赋值之前，通过“隐藏”的引用修改了被引用的对象。

Example:

示例：

```c++
std::string s("foo");

struct Base {
  Base() {
    s = "bar";
  }
};

struct Derived : Base {
-  Derived(const std::string &S) : Field(S)
+  Derived(std::string S) : Field(std::move(S))
  { }

  std::string Field;
};

void f() {
-  Derived d(s); // d.Field holds "bar"
+  Derived d(s); // d.Field holds "foo"
}
```

### Note about delayed template parsing

When delayed template parsing is enabled, constructors part of templated contexts; templated constructors, constructors in class templates, constructors of inner classes of template classes, etc., are not transformed. Delayed template parsing is enabled by default on Windows as a Microsoft extension: Clang Compiler User's Manual - Microsoft extensions.

启用延迟模板解析时，模板上下文中的构造函数（如模板构造函数、类模板中的构造函数、模板类的内部类构造函数等）不会被转换。延迟模板解析在 Windows 上默认启用，作为 Microsoft 扩展的一部分：Clang 编译器用户手册 - Microsoft 扩展。

Delayed template parsing can be enabled using the -fdelayed-template-parsing flag and disabled using -fno-delayed-template-parsing.

可以使用 -fdelayed-template-parsing 标志启用延迟模板解析，使用 -fno-delayed-template-parsing 禁用。

Example:

示例：

```c++
template <typename T> class C {
  std::string S;

public:
=  // using -fdelayed-template-parsing (default on Windows)
=  C(const std::string &S) : S(S) {}

+  // using -fno-delayed-template-parsing (default on non-Windows systems)
+  C(std::string S) : S(std::move(S)) {}
};
```

::: seealso

For more information about the pass-by-value idiom, read: Want Speed? Pass by Value

有关按值传递惯用法的更多信息，请阅读：Want Speed? Pass by Value

:::

## Options

选项

::: option

IncludeStyle

A string specifying which include-style is used, llvm or google. Default is llvm.

一个字符串，用于指定使用哪种 include 风格，llvm 或 google。默认值为 llvm。

:::

::: option

ValuesOnly

When true, the check only warns about copied parameters that are already passed by value. Default is false.

当为 true 时，此检查仅对已经按值传递但仍被复制的参数发出警告。默认值为 false。

:::
