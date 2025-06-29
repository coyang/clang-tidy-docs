# bugprone-undefined-memory-manipulation

Finds calls of memory manipulation functions memset(), memcpy() and memmove() on non-TriviallyCopyable objects resulting in undefined behavior.

查找对非 TriviallyCopyable（可平凡复制）对象调用内存操作函数 memset()、memcpy() 和 memmove() 的情况，这些调用会导致未定义行为。

Using memory manipulation functions on non-TriviallyCopyable objects can lead to a range of subtle and challenging issues in C++ code. The most immediate concern is the potential for undefined behavior, where the state of the object may become corrupted or invalid. This can manifest as crashes, data corruption, or unexpected behavior at runtime, making it challenging to identify and diagnose the root cause. Additionally, misuse of memory manipulation functions can bypass essential object-specific operations, such as constructors and destructors, leading to resource leaks or improper initialization.

在非 TriviallyCopyable 对象上使用内存操作函数可能会在 C++ 代码中引发一系列微妙且难以排查的问题。最直接的风险是可能导致未定义行为，使对象的状态被破坏或变得无效。这可能表现为程序崩溃、数据损坏或运行时出现意外行为，从而使问题的根本原因难以识别和诊断。此外，错误使用内存操作函数可能会绕过对象特有的重要操作，例如构造函数和析构函数，从而导致资源泄漏或初始化不当。

For example, when using memcpy to copy std::string, pointer data is being copied, and it can result in a double free issue.

例如，当使用 memcpy 复制 std::string 时，复制的是指针数据，这可能导致双重释放（double free）的问题。

```c++
#include <cstring>
#include <string>

int main() {
    std::string source = "Hello";
    std::string destination;

    std::memcpy(&destination, &source, sizeof(std::string));

    // Undefined behavior may occur here, during std::string destructor call.
    return 0;
}
```

```c++
#include <cstring>
#include <string>

int main() {
    std::string source = "Hello";
    std::string destination;

    std::memcpy(&destination, &source, sizeof(std::string));

    // 在调用 std::string 的析构函数时，可能会发生未定义行为。
    return 0;
}
```
