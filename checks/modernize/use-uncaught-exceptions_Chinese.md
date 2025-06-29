# modernize-use-uncaught-exceptions

This check will warn on calls to std::uncaught_exception and replace  
them with calls to std::uncaught_exceptions, since  
std::uncaught_exception was deprecated in C++17.

此检查会对调用 std::uncaught_exception 的代码发出警告，并将其替换为 std::uncaught_exceptions 的调用，因为 std::uncaught_exception 在 C++17 中已被弃用。

Below are a few examples of what kind of occurrences will be found and  
what they will be replaced with.

下面是一些示例，展示了会被检测到的代码形式以及它们将被替换成的内容。

```c++
#define MACRO1 std::uncaught_exception
#define MACRO2 std::uncaught_exception

int uncaught_exception() {
  return 0;
}

int main() {
  int res;

  res = uncaught_exception();
  // No warning, since it is not the deprecated function from namespace std

  res = MACRO2();
  // Warning, but will not be replaced

  res = std::uncaught_exception();
  // Warning and replaced

  using std::uncaught_exception;
  // Warning and replaced

  res = uncaught_exception();
  // Warning and replaced
}
```

```c++
#define MACRO1 std::uncaught_exception
#define MACRO2 std::uncaught_exception

int uncaught_exception() {
  return 0;
}

int main() {
  int res;

  res = uncaught_exception();

  res = MACRO2();

  res = std::uncaught_exceptions();

  using std::uncaught_exceptions;

  res = uncaught_exceptions();
}
```

应用修复后，代码将如下所示：
