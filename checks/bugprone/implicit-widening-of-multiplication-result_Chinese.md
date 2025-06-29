# bugprone-implicit-widening-of-multiplication-result

该检查诊断乘法结果被隐式扩展（widening）的情况，并建议（带有修复建议）通过显式扩展来消除警告，或者在更宽的类型中执行乘法，以避免后续的隐式扩展。

该检查主要在处理非常大的缓冲区时非常有用。例如，考虑以下代码：

This check diagnoses instances where a result of a multiplication is
implicitly widened, and suggests (with fix-it) to either silence the
code by making widening explicit, or to perform the multiplication in a
wider type, to avoid the widening afterwards.

该检查诊断乘法结果被隐式扩展的情况，并建议（带有修复建议）通过显式扩展来消除警告，或者在更宽的类型中执行乘法，以避免后续的扩展。

This is mainly useful when operating on very large buffers. For example,
consider:

该检查主要在处理非常大的缓冲区时非常有用。例如，考虑以下代码：

```c++
void zeroinit(char* base, unsigned width, unsigned height) {
  for(unsigned row = 0; row != height; ++row) {
    for(unsigned col = 0; col != width; ++col) {
      char* ptr = base + row * width + col;
      *ptr = 0;
    }
  }
}
```

This is fine in general, but if width \* height overflows, you end up
wrapping back to the beginning of base instead of processing the
entire requested buffer.

这段代码通常没有问题，但如果 width \* height 发生溢出，你最终会回绕到 base 的起始位置，而不是处理整个请求的缓冲区。

Indeed, this only matters for pretty large buffers (4GB+), but that can
happen very easily for example in image processing, where for that to
happen you "only" need a ~269MPix image.

确实，这种情况只会在非常大的缓冲区（4GB 以上）中出现，但在图像处理等场景中，这种情况很容易发生，例如只需要一张约 2.69 亿像素的图像就可能触发。

## Options

## 选项

::: option
UseCXXStaticCastsInCppSources

When suggesting fix-its for C++ code, should C++-style
static_cast<>()'s be suggested, or C-style casts. Defaults to
true.

在为 C++ 代码建议修复时，是否建议使用 C++ 风格的 static_cast<>()，而不是 C 风格的强制类型转换。默认值为 true。

:::

::: option
UseCXXHeadersInCppSources

When suggesting to include the appropriate header in C++ code, should
<cstddef> header be suggested, or <stddef.h>. Defaults to
true.

在建议 C++ 代码包含适当的头文件时，是否建议使用 <cstddef> 而不是 <stddef.h>。默认值为 true。

:::

::: option
IgnoreConstantIntExpr

If the multiplication operands are compile-time constants (like literals
or are constexpr) and fit within the source expression type, do not
emit a diagnostic or suggested fix. Only considers expressions where the
source expression is a signed integer type. Defaults to
false.

如果乘法操作数是编译时常量（如字面量或 constexpr），并且在源表达式类型范围内，则不发出诊断或修复建议。仅考虑源表达式为有符号整数类型的情况。默认值为 false。

:::

Examples:

示例：

```c++
long mul(int a, int b) {
  return a * b; // warning: performing an implicit widening conversion to type 'long' of a multiplication performed in type 'int'
                // 警告：在 int 类型中执行的乘法结果被隐式转换为 long 类型
}

char* ptr_add(char *base, int a, int b) {
  return base + a * b; // warning: result of multiplication in type 'int' is used as a pointer offset after an implicit widening conversion to type 'ssize_t'
                       // 警告：int 类型的乘法结果在被隐式转换为 ssize_t 类型后用作指针偏移
}

char ptr_subscript(char *base, int a, int b) {
  return base[a * b]; // warning: result of multiplication in type 'int' is used as a pointer offset after an implicit widening conversion to type 'ssize_t'
                      // 警告：int 类型的乘法结果在被隐式转换为 ssize_t 类型后用作指针偏移
}
```
