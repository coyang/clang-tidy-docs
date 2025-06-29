以下是翻译后的 markdown 文件内容，保留了中英文双语，中文在英文下方，并以一个空行分隔，同时保留了原始代码块格式：

# modernize-use-trailing-return-type

Rewrites function and lambda signatures to use a trailing return type (introduced in C++11). This transformation is purely stylistic. The return type before the function name is replaced by auto and inserted after the function parameter list (and qualifiers).

重写函数和 lambda 表达式的签名以使用尾置返回类型（在 C++11 中引入）。此转换纯属风格上的改变。函数名前的返回类型将被替换为 auto，并插入到函数参数列表（以及限定符）之后。

## Example

## 示例

```c++
int f1();
inline int f2(int arg) noexcept;
virtual float f3() const && = delete;
auto lambda = []() {};
```

transforms to:

转换为：

```c++
auto f1() -> int;
inline auto f2(int arg) -> int noexcept;
virtual auto f3() const && -> float = delete;
auto lambda = []() -> void {};
```

## Known Limitations

## 已知限制

The following categories of return types cannot be rewritten currently:

以下几类返回类型当前无法重写：

- function pointers  
  函数指针
- member function pointers  
  成员函数指针
- member pointers  
  成员指针

Unqualified names in the return type might erroneously refer to different entities after the rewrite. Preventing such errors requires a full lookup of all unqualified names present in the return type in the scope of the trailing return type location. This location includes e.g. function parameter names and members of the enclosing class (including all inherited classes). Such a lookup is currently not implemented.

返回类型中的非限定名称在重写后可能会错误地引用不同的实体。为了防止此类错误，需要在尾置返回类型所在的作用域中对返回类型中出现的所有非限定名称进行完整查找。该作用域包括例如函数参数名称以及封闭类的成员（包括所有继承的类）。目前尚未实现此类查找。

Given the following piece of code

给定以下代码片段：

```c++
struct S { long long value; };
S f(unsigned S) { return {S * 2}; }
class CC {
  int S;
  struct S m();
};
S CC::m() { return {0}; }
```

a careless rewrite would produce the following output:

不加小心地重写将产生如下输出：

```c++
struct S { long long value; };
auto f(unsigned S) -> S { return {S * 2}; } // error
class CC {
  int S;
  auto m() -> struct S;
};
auto CC::m() -> S { return {0}; } // error
```

This code fails to compile because the S in the context of f refers to the equally named function parameter. Similarly, the S in the context of m refers to the equally named class member. The check can currently only detect and avoid a clash with a function parameter name.

这段代码无法编译，因为在函数 f 的上下文中，S 指的是同名的函数参数。同样地，在函数 m 的上下文中，S 指的是同名的类成员。目前的检查只能检测并避免与函数参数名称的冲突。

## Options

## 选项

::: option  
TransformFunctions

When set to true, function declarations will be transformed to use trailing return. Default is true.

当设置为 true 时，函数声明将被转换为使用尾置返回类型。默认值为 true。

:::

::: option  
TransformLambdas

Controls how lambda expressions are transformed to use trailing return type. Possible values are:

控制如何将 lambda 表达式转换为使用尾置返回类型。可选值包括：

- all - Transform all lambda expressions without an explicit return type to use trailing return type. If type can not be deduced, auto will be used since C++14 and generic message will be emitted otherwise.  
  all - 转换所有没有显式返回类型的 lambda 表达式为使用尾置返回类型。如果无法推导出类型，在 C++14 中将使用 auto，否则将发出通用警告信息。

- all_except_auto - Transform all lambda expressions except those whose return type can not be deduced.  
  all_except_auto - 转换所有 lambda 表达式，除了那些无法推导出返回类型的。

- none - Do not transform any lambda expressions.  
  none - 不转换任何 lambda 表达式。

Default is all.  
默认值为 all。

:::
