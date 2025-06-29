# bugprone-multi-level-implicit-pointer-conversion

Detects implicit conversions between pointers of different levels of indirection.

检测不同间接级别的指针之间的隐式转换。

Conversions between pointer types of different levels of indirection can be dangerous and may lead to undefined behavior, particularly if the converted pointer is later cast to a type with a different level of indirection. For example, converting a pointer to a pointer to an int (int\*_) to a void_ can result in the loss of information about the original level of indirection, which can cause problems when attempting to use the converted pointer. If the converted pointer is later cast to a type with a different level of indirection and dereferenced, it may lead to access violations, memory corruption, or other undefined behavior.

不同间接级别的指针类型之间的转换可能是危险的，并可能导致未定义行为，特别是在转换后的指针随后被强制转换为具有不同间接级别的类型时。例如，将一个指向 int 的指针的指针（int\*_）转换为 void_，可能会导致原始间接级别的信息丢失，从而在使用该转换后的指针时引发问题。如果该指针随后被强制转换为具有不同间接级别的类型并被解引用，可能会导致访问违规、内存损坏或其他未定义行为。

Consider the following example:

请参考以下示例：

```c++
void foo(void* ptr);

int main() {
  int x = 42;
  int* ptr = &x;
  int** ptr_ptr = &ptr;
  foo(ptr_ptr); // warning will trigger here
  return 0;
}
```

In this example, foo() is called with ptr_ptr as its argument. However, ptr_ptr is a int\*_ pointer, while foo() expects a void_ pointer. This results in an implicit pointer level conversion, which could cause issues if foo() dereferences the pointer assuming it's a int\* pointer.

在此示例中，foo() 被调用时传入了 ptr_ptr 作为参数。然而，ptr_ptr 是一个 int\*_ 类型的指针，而 foo() 期望的是一个 void_ 类型的指针。这导致了一个隐式的指针级别转换，如果 foo() 假设该指针是 int\* 类型并对其进行解引用，可能会引发问题。

Using an explicit cast is a recommended solution to prevent issues caused by implicit pointer level conversion, as it allows the developer to explicitly state their intention and show their reasoning for the type conversion. Additionally, it is recommended that developers thoroughly check and verify the safety of the conversion before using an explicit cast. This extra level of caution can help catch potential issues early on in the development process, improving the overall reliability and maintainability of the code.

使用显式类型转换是防止隐式指针级别转换引发问题的推荐解决方案，因为它允许开发者明确表达其意图，并展示其进行类型转换的理由。此外，建议开发者在使用显式类型转换之前，彻底检查并验证该转换的安全性。这种额外的谨慎可以帮助在开发早期发现潜在问题，从而提高代码的整体可靠性和可维护性。

## Options

## 选项

::: option
EnableInC

If true, enables the check in C code (it is always enabled in C++ code). Default is true.

如果为 true，则在 C 代码中启用此检查（在 C++ 代码中始终启用）。默认值为 true。
:::
