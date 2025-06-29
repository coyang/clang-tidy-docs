# performance-avoid-endl

Checks for uses of std::endl on streams and suggests using the newline character '\n' instead.

检查在流中使用 std::endl 的情况，并建议改用换行符 '\n'。

Rationale: Using std::endl on streams can be less efficient than using the newline character '\n' because std::endl performs two operations: it writes a newline character to the output stream and then flushes the stream buffer. Writing a single newline character using '\n' does not trigger a flush, which can improve performance. In addition, flushing the stream buffer can cause additional overhead when working with streams that are buffered.

原理：在流中使用 std::endl 的效率可能低于使用换行符 '\n'，因为 std::endl 执行两个操作：它将换行符写入输出流，然后刷新流缓冲区。而使用 '\n' 写入单个换行符不会触发刷新操作，这可以提升性能。此外，在处理带缓冲的流时，刷新流缓冲区可能会带来额外的开销。

Example:

示例：

Consider the following code:

请考虑以下代码：

```c++
#include <iostream>

int main() {
  std::cout << "Hello" << std::endl;
}
```

Which gets transformed into:

它将被转换为：

```c++
#include <iostream>

int main() {
  std::cout << "Hello" << '\n';
}
```

This code writes a single newline character to the std::cout stream without flushing the stream buffer.

此代码将单个换行符写入 std::cout 流，而不会刷新流缓冲区。

Additionally, it is important to note that the standard C++ streams (like std::cerr, std::wcerr, std::clog and std::wclog) always flush after a write operation, unless std::ios_base::sync_with_stdio is set to false. regardless of whether std::endl or '\n' is used. Therefore, using '\n' with these streams will not result in any performance gain, but it is still recommended to use '\n' for consistency and readability.

此外，需要注意的是，标准 C++ 流（如 std::cerr、std::wcerr、std::clog 和 std::wclog）在写操作后总是会刷新，除非 std::ios_base::sync_with_stdio 被设置为 false。无论使用 std::endl 还是 '\n' 都会如此。因此，在这些流中使用 '\n' 不会带来性能提升，但仍建议为了一致性和可读性使用 '\n'。

If you do need to flush the stream buffer, you can use std::flush explicitly like this:

如果确实需要刷新流缓冲区，可以像下面这样显式使用 std::flush：

```c++
#include <iostream>

int main() {
  std::cout << "Hello\n" << std::flush;
}
```
