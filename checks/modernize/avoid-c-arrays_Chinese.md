# modernize-avoid-c-arrays

# modernize-avoid-c-arrays

[cppcoreguidelines-avoid-c-arrays]{.title-ref} redirects here as an alias for this check.

[cppcoreguidelines-avoid-c-arrays]{.title-ref} 是此检查项的别名，重定向至此。

[hicpp-avoid-c-arrays]{.title-ref} redirects here as an alias for this check.

[hicpp-avoid-c-arrays]{.title-ref} 是此检查项的别名，重定向至此。

Finds C-style array types and recommend to use std::array<> / std::vector<>. All types of C arrays are diagnosed.

检测 C 风格数组类型，并建议使用 std::array<> 或 std::vector<>。所有类型的 C 数组都会被诊断。

For parameters of incomplete C-style array type, it would be better to use std::span / gsl::span as replacement.

对于不完整的 C 风格数组类型的参数，建议使用 std::span 或 gsl::span 作为替代。

However, fix-it are potentially dangerous in header files and are therefore not emitted right now.

然而，在头文件中自动修复可能存在风险，因此当前不会提供自动修复建议。

```c++
int a[] = {1, 2}; // warning: do not declare C-style arrays, use 'std::array' instead

int a[] = {1, 2}; // 警告：不要声明 C 风格数组，请改用 'std::array'

int b[1]; // warning: do not declare C-style arrays, use 'std::array' instead

int b[1]; // 警告：不要声明 C 风格数组，请改用 'std::array'

void foo() {
  int c[b[0]]; // warning: do not declare C VLA arrays, use 'std::vector' instead
}

void foo() {
  int c[b[0]]; // 警告：不要声明 C 可变长度数组，请改用 'std::vector'
}

template <typename T, int Size>
class array {
  T d[Size]; // warning: do not declare C-style arrays, use 'std::array' instead

  T d[Size]; // 警告：不要声明 C 风格数组，请改用 'std::array'

  int e[1]; // warning: do not declare C-style arrays, use 'std::array' instead

  int e[1]; // 警告：不要声明 C 风格数组，请改用 'std::array'
};

array<int[4], 2> d; // warning: do not declare C-style arrays, use 'std::array' instead

array<int[4], 2> d; // 警告：不要声明 C 风格数组，请改用 'std::array'

using k = int[4]; // warning: do not declare C-style arrays, use 'std::array' instead

using k = int[4]; // 警告：不要声明 C 风格数组，请改用 'std::array'
```

However, the extern "C" code is ignored, since it is common to share such headers between C code, and C++ code.

然而，extern "C" 代码会被忽略，因为这种头文件通常在 C 和 C++ 代码之间共享。

```c++
// Some header
extern "C" {

int f[] = {1, 2}; // not diagnosed

int f[] = {1, 2}; // 不会被诊断

int j[1]; // not diagnosed

int j[1]; // 不会被诊断

inline void bar() {
  {
    int j[j[0]]; // not diagnosed
  }
}

inline void bar() {
  {
    int j[j[0]]; // 不会被诊断
  }
}

}
```

Similarly, the main() function is ignored. Its second and third parameters can be either char\* argv[] or char\*\* argv, but cannot be std::array<>.

同样，main() 函数也会被忽略。其第二和第三个参数可以是 char\* argv[] 或 char\*\* argv，但不能是 std::array<>。

## Options

## 选项

::: option
AllowStringArrays

AllowStringArrays

When set to true (default is false), variables of character array type with deduced length, initialized directly from string literals, will be ignored. This option doesn't affect cases where length can't be deduced, resembling pointers, as seen in class members and parameters. Example:

当设置为 true（默认值为 false）时，直接由字符串字面量初始化、长度可推导的字符数组变量将被忽略。此选项不会影响那些长度无法推导、类似指针的情况，例如类成员和函数参数。示例：

```c++
const char name[] = "Some name";
```

const char name[] = "Some name";

```

```
