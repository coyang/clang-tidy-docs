# bugprone-not-null-terminated-result

Finds function calls where it is possible to cause a not null-terminated result. Usually the proper length of a string is strlen(src) + 1 or equal length of this expression, because the null terminator needs an extra space. Without the null terminator it can result in undefined behavior when the string is read.

查找可能导致结果字符串未以空字符结尾的函数调用。通常字符串的正确长度应为 strlen(src) + 1 或等价的表达式长度，因为空字符终止符需要额外的空间。如果没有空字符终止符，在读取字符串时可能会导致未定义行为。

The following and their respective wchar_t based functions are checked:

以下函数及其对应的 wchar_t 版本会被检查：

memcpy, memcpy_s, memchr, memmove, memmove_s, strerror_s, strncmp, strxfrm

The following is a real-world example where the programmer forgot to increase the passed third argument, which is size_t length. That is why the length of the allocated memory is not enough to hold the null terminator.

下面是一个真实的示例，程序员忘记增加传入的第三个参数（即 size_t 类型的长度），因此分配的内存长度不足以容纳空字符终止符。

```c
static char *stringCpy(const std::string &str) {
  char *result = reinterpret_cast<char *>(malloc(str.size()));
  memcpy(result, str.data(), str.size());
  return result;
}
```

In addition to issuing warnings, fix-it rewrites all the necessary code. It also tries to adjust the capacity of the destination array:

除了发出警告之外，修复工具还会重写所有必要的代码，并尝试调整目标数组的容量：

```c
static char *stringCpy(const std::string &str) {
  char *result = reinterpret_cast<char *>(malloc(str.size() + 1));
  strcpy(result, str.data());
  return result;
}
```

Note: It cannot guarantee to rewrite every of the path-sensitive memory allocations.

注意：它无法保证重写所有路径敏感的内存分配。

## Transformation rules of 'memcpy()' {#MemcpyTransformation}

It is possible to rewrite the memcpy() and memcpy_s() calls as the following four functions: strcpy(), strncpy(), strcpy_s(), strncpy_s(), where the latter two are the safer versions of the former two. It rewrites the wchar_t based memory handler functions respectively.

可以将 memcpy() 和 memcpy_s() 调用重写为以下四个函数：strcpy()、strncpy()、strcpy_s()、strncpy_s()，其中后两个是前两个的更安全版本。对于基于 wchar_t 的内存处理函数也会进行相应的重写。

### Rewrite based on the destination array

基于目标数组的重写规则：

- If copy to the destination array cannot overflow [1] the new function should be the older copy function (ending with cpy), because it is more efficient than the safe version.

  如果复制到目标数组不会溢出 [1]，则应使用旧的复制函数（以 cpy 结尾），因为它比安全版本更高效。

- If copy to the destination array can overflow [1] and WantToUseSafeFunctions is set to true and it is possible to obtain the capacity of the destination array then the new function could be the safe version (ending with cpy_s).

  如果复制到目标数组可能溢出 [1]，并且 WantToUseSafeFunctions 设置为 true，且可以获取目标数组的容量，则新函数可以使用安全版本（以 cpy_s 结尾）。

- If the new function is could be safe version and C++ files are analyzed and the destination array is plain char/wchar_t without un/signed then the length of the destination array can be omitted.

  如果新函数可以是安全版本，且分析的是 C++ 文件，并且目标数组是未加 un/signed 修饰的普通 char/wchar_t 类型，则可以省略目标数组的长度。

- If the new function is could be safe version and the destination array is un/signed it needs to be casted to plain char _/wchar_t _.

  如果新函数可以是安全版本，且目标数组是 un/signed 类型，则需要将其强制转换为普通的 char _/wchar_t _ 类型。

[1] It is possible to overflow:

[1] 存在溢出的可能性：

: - If the capacity of the destination array is unknown.

- If the given length is equal to the destination array's capacity.

: - 如果目标数组的容量未知。

- 如果给定的长度等于目标数组的容量。

### Rewrite based on the length of the source string

基于源字符串长度的重写规则：

- If the given length is strlen(source) or equal length of this expression then the new function should be the older copy function (ending with cpy), as it is more efficient than the safe version (ending with cpy_s).

  如果给定的长度是 strlen(source) 或等价的表达式长度，则应使用旧的复制函数（以 cpy 结尾），因为它比安全版本（以 cpy_s 结尾）更高效。

- Otherwise we assume that the programmer wanted to copy 'N' characters, so the new function is ncpy-like which copies 'N' characters.

  否则我们假设程序员想要复制 'N' 个字符，因此新函数应为类似 ncpy 的函数，它会复制 'N' 个字符。

## Transformations with 'strlen()' or equal length of this expression

It transforms the wchar_t based memory and string handler functions respectively (where only strerror_s does not have wchar_t based alias).

对于使用 strlen() 或等价表达式长度的情况，会分别转换基于 wchar_t 的内存和字符串处理函数（其中只有 strerror_s 没有基于 wchar_t 的别名）。

### Memory handler functions

内存处理函数：

memcpy Please visit the Transformation rules of 'memcpy()' section.

memcpy 请参阅“Transformation rules of 'memcpy()'”部分。

memchr Usually there is a C-style cast and it is needed to be removed, because the new function strchr's return type is correct. The given length is going to be removed.

memchr 通常存在 C 风格的类型转换，需要将其移除，因为新函数 strchr 的返回类型是正确的。给定的长度也将被移除。

memmove If safe functions are available the new function is memmove_s, which has a new second argument which is the length of the destination array, it is adjusted, and the length of the source string is incremented by one. If safe functions are not available the given length is incremented by one.

memmove 如果可用安全函数，则新函数为 memmove_s，它有一个新的第二个参数，即目标数组的长度，该长度会被调整，并且源字符串的长度会加一。如果不可用安全函数，则仅将给定长度加一。

memmove_s The given length is incremented by one.

memmove_s 给定的长度加一。

### String handler functions

字符串处理函数：

strerror_s The given length is incremented by one.

strerror_s 给定的长度加一。

strncmp If the third argument is the first or the second argument's length + 1 it has to be truncated without the + 1 operation.

strncmp 如果第三个参数是第一个或第二个参数的长度 + 1，则必须去掉 + 1 操作。

strxfrm The given length is incremented by one.

strxfrm 给定的长度加一。

## Options

## 选项

::: option
WantToUseSafeFunctions

The value true specifies that the target environment is considered to implement '\_s' suffixed memory and string handler functions which are safer than older versions (e.g. 'memcpy_s()'). The default value is true.

值为 true 表示目标环境被认为实现了以 '\_s' 结尾的内存和字符串处理函数，这些函数比旧版本（例如 memcpy_s()）更安全。默认值为 true。
:::
