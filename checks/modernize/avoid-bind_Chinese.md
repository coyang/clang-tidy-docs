# modernize-avoid-bind

The check finds uses of std::bind and boost::bind and replaces them with lambdas. Lambdas will use value-capture unless reference capture is explicitly requested with std::ref or boost::ref.

该检查会查找 std::bind 和 boost::bind 的使用，并将其替换为 lambda 表达式。除非使用 std::ref 或 boost::ref 明确请求引用捕获，否则 lambda 表达式将使用值捕获。

It supports arbitrary callables including member functions, function objects, and free functions, and all variations thereof. Anything that you can pass to the first argument of bind should be diagnosable. Currently, the only known case where a fix-it is unsupported is when the same placeholder is specified multiple times in the parameter list.

该检查支持任意可调用对象，包括成员函数、函数对象和自由函数，以及它们的各种变体。任何可以作为 bind 第一个参数传入的对象都应当可以被诊断。目前，唯一已知无法自动修复的情况是参数列表中多次使用相同的占位符。

Given:

给定：

```c++
int add(int x, int y) { return x + y; }
```

Then:

那么：

```c++
void f() {
  int x = 2;
  auto clj = std::bind(add, x, _1);
}
```

is replaced by:

将被替换为：

```c++
void f() {
  int x = 2;
  auto clj = [=](auto && arg1) { return add(x, arg1); };
}
```

std::bind can be hard to read and can result in larger object files and binaries due to type information that will not be produced by equivalent lambdas.

std::bind 可读性较差，并且由于包含类型信息，可能导致生成的目标文件和可执行文件体积更大，而等效的 lambda 表达式不会产生这些额外信息。

## Options

## 选项

::: option
PermissiveParameterList

If the option is set to true, the check will append auto&&... to the end of every placeholder parameter list. Without this, it is possible for a fix-it to perform an incorrect transformation in the case where the result of the bind is used in the context of a type erased functor such as std::function which allows mismatched arguments. Default is false.

如果该选项设置为 true，检查器将在每个占位符参数列表的末尾追加 auto&&...。如果不启用此选项，在某些情况下（例如 bind 的结果用于类型擦除的函数对象如 std::function，且允许参数不匹配时），自动修复可能会进行错误的转换。默认值为 false。

:::

For example:

例如：

```c++
int add(int x, int y) { return x + y; }
int foo() {
  std::function<int(int,int)> ignore_args = std::bind(add, 2, 2);
  return ignore_args(3, 3);
}
```

is valid code, and returns 4. The actual values passed to ignore_args are simply ignored. Without PermissiveParameterList, this would be transformed into

这是合法的代码，并返回 4。传递给 ignore_args 的实际参数会被忽略。如果未启用 PermissiveParameterList，该代码将被转换为：

```c++
int add(int x, int y) { return x + y; }
int foo() {
  std::function<int(int,int)> ignore_args = [] { return add(2, 2); }
  return ignore_args(3, 3);
}
```

which will not compile, since the lambda does not contain an operator() that accepts 2 arguments. With permissive parameter list, it instead generates

这段代码将无法编译，因为该 lambda 表达式没有包含接受两个参数的 operator()。启用 permissive 参数列表后，将生成如下代码：

```c++
int add(int x, int y) { return x + y; }
int foo() {
  std::function<int(int,int)> ignore_args = [](auto&&...) { return add(2, 2); }
  return ignore_args(3, 3);
}
```

which is correct.

这是正确的写法。

This check requires using C++14 or higher to run.

此检查器要求使用 C++14 或更高版本。
