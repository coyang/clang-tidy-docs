# bugprone-exception-escape

Finds functions which may throw an exception directly or indirectly, but they should not. The functions which should not throw exceptions are the following:

查找可能直接或间接抛出异常的函数，但这些函数本不应抛出异常。以下是本不应抛出异常的函数：

- Destructors  
  析构函数

- Move constructors  
  移动构造函数

- Move assignment operators  
  移动赋值运算符

- The main() functions  
  main() 函数

- swap() functions  
  swap() 函数

- iter_swap() functions  
  iter_swap() 函数

- iter_move() functions  
  iter_move() 函数

- Functions marked with throw() or noexcept  
  使用 throw() 或 noexcept 标记的函数

- Other functions given as option  
  通过选项指定的其他函数

A destructor throwing an exception may result in undefined behavior, resource leaks or unexpected termination of the program. Throwing move constructor or move assignment also may result in undefined behavior or resource leak. The swap() operations expected to be non throwing most of the cases and they are always possible to implement in a non throwing way. Non throwing swap() operations are also used to create move operations. A throwing main() function also results in unexpected termination.

析构函数抛出异常可能导致未定义行为、资源泄漏或程序意外终止。移动构造函数或移动赋值运算符抛出异常也可能导致未定义行为或资源泄漏。swap() 操作在大多数情况下应为非抛出，并且总是可以以非抛出的方式实现。非抛出的 swap() 操作也常用于实现移动操作。main() 函数抛出异常也会导致程序意外终止。

Functions declared explicitly with noexcept(false) or throw(exception) will be excluded from the analysis, as even though it is not recommended for functions like swap(), main(), move constructors, move assignment operators and destructors, it is a clear indication of the developer's intention and should be respected.

显式声明为 noexcept(false) 或 throw(exception) 的函数将被排除在分析之外，尽管不推荐对 swap()、main()、移动构造函数、移动赋值运算符和析构函数使用这些声明，但它们明确表达了开发者的意图，应予以尊重。

WARNING! This check may be expensive on large source files.

警告！此检查在大型源文件上可能开销较大。

## Options

## 选项

::: option  
FunctionsThatShouldNotThrow

Comma separated list containing function names which should not throw. An example value for this parameter can be WinMain which adds function WinMain() in the Windows API to the list of the functions which should not throw. Default value is an empty string.

以逗号分隔的函数名列表，表示这些函数不应抛出异常。例如，该参数的一个值可以是 WinMain，它会将 Windows API 中的 WinMain() 函数加入不应抛出异常的函数列表。默认值为空字符串。

:::

::: option  
IgnoredExceptions

Comma separated list containing type names which are not counted as thrown exceptions in the check. Default value is an empty string.

以逗号分隔的类型名列表，这些类型在检查中不会被视为抛出的异常。默认值为空字符串。

:::
