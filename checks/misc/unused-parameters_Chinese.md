# misc-unused-parameters

Finds unused function parameters. Unused parameters may signify a bug in the code (e.g. when a different parameter is used instead). The suggested fixes either comment parameter name out or remove the parameter completely, if all callers of the function are in the same translation unit and can be updated.

查找未使用的函数参数。未使用的参数可能表示代码中存在错误（例如，使用了另一个参数）。建议的修复方式是将参数名称注释掉，或者在所有调用该函数的代码都在同一个翻译单元并且可以更新的情况下，完全移除该参数。

The check is similar to the -Wunused-parameter compiler diagnostic and can be used to prepare a codebase to enabling of that diagnostic. By default the check is more permissive (see StrictMode).

该检查类似于 -Wunused-parameter 编译器诊断选项，可用于为启用该诊断做准备。默认情况下，该检查更为宽松（参见 StrictMode）。

```c++
void a(int i) { /*some code that doesn't use `i`*/ }

// becomes

void a(int  /*i*/) { /*some code that doesn't use `i`*/ }
```

```c++
static void staticFunctionA(int i);
static void staticFunctionA(int i) { /*some code that doesn't use `i`*/ }

// becomes

static void staticFunctionA()
static void staticFunctionA() { /*some code that doesn't use `i`*/ }
```

## Options

## 选项

::: option
StrictMode

When false (default value), the check will ignore trivially unused parameters, i.e. when the corresponding function has an empty body (and in case of constructors - no constructor initializers). When the function body is empty, an unused parameter is unlikely to be unnoticed by a human reader, and there's basically no place for a bug to hide.

当为 false（默认值）时，该检查将忽略明显未使用的参数，即对应函数体为空（对于构造函数而言，也没有构造函数初始化器）。当函数体为空时，未使用的参数很容易被人类读者发现，基本上没有隐藏错误的空间。
:::

::: option
IgnoreVirtual

Determines whether virtual method parameters should be inspected. Set to true to ignore them. Default is false.

决定是否检查虚函数的方法参数。设置为 true 可忽略它们。默认值为 false。
:::
