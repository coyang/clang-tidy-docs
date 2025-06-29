# bugprone-sizeof-container

The check finds usages of sizeof on expressions of STL container types. Most likely the user wanted to use .size() instead.

该检查会发现对 STL 容器类型表达式使用 sizeof 的情况。用户很可能是想使用 .size() 方法。

All class/struct types declared in namespace std:: having a const size() method are considered containers, with the exception of std::bitset and std::array.

所有在命名空间 std:: 中声明且具有 const size() 方法的类/结构体类型都被视为容器，std::bitset 和 std::array 除外。

Examples:

示例：

```c++
std::string s;
int a = 47 + sizeof(s); // warning: sizeof() doesn't return the size of the container. Did you mean .size()?

std::string s;
int a = 47 + sizeof(s); // 警告：sizeof() 不会返回容器的大小。你是不是想用 .size()？

int b = sizeof(std::string); // no warning, probably intended.

int b = sizeof(std::string); // 没有警告，很可能是有意为之。

std::string array_of_strings[10];
int c = sizeof(array_of_strings) / sizeof(array_of_strings[0]); // no warning, definitely intended.

std::string array_of_strings[10];
int c = sizeof(array_of_strings) / sizeof(array_of_strings[0]); // 没有警告，肯定是有意为之。

std::array<int, 3> std_array;
int d = sizeof(std_array); // no warning, probably intended.

std::array<int, 3> std_array;
int d = sizeof(std_array); // 没有警告，很可能是有意为之。
```
