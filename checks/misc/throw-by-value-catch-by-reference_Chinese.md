以下是完整的中英文双语翻译内容，保留了原始的 markdown 格式和代码块格式：

# misc-throw-by-value-catch-by-reference

# misc-throw-by-value-catch-by-reference

# 按值抛出，按引用捕获

[cert-err09-cpp]{.title-ref} and [cert-err61-cpp]{.title-ref} redirect here as aliases of this check.

[cert-err09-cpp]{.title-ref} 和 [cert-err61-cpp]{.title-ref} 被重定向为此检查的别名。

Finds violations of the rule "Throw by value, catch by reference" presented for example in "C++ Coding Standards" by H. Sutter and A. Alexandrescu, as well as the CERT C++ Coding Standard rule ERR61-CPP. Catch exceptions by lvalue reference.

查找违反“按值抛出，按引用捕获”规则的情况，该规则可在 H. Sutter 和 A. Alexandrescu 所著的《C++ 编码标准》中找到，同时也符合 CERT C++ 编码标准中的规则 ERR61-CPP：通过左值引用捕获异常。

Exceptions:

例外情况：

- Throwing string literals will not be flagged despite being a pointer. They are not susceptible to slicing and the usage of string literals is idiomatic.  
  抛出字符串字面量不会被标记为违规，尽管它们是指针类型。因为字符串字面量不容易被切片，并且其使用方式是惯用的。

- Catching character pointers (char, wchar_t, unicode character types) will not be flagged to allow catching string literals.  
  捕获字符指针（char、wchar_t、Unicode 字符类型）不会被标记为违规，以允许捕获字符串字面量。

- Moved named values will not be flagged as not throwing an anonymous temporary. In this case we can be sure that the user knows that the object can't be accessed outside catch blocks handling the error.  
  移动的具名值不会被标记为未抛出匿名临时对象。在这种情况下，可以确定用户知道该对象不能在处理错误的 catch 块之外访问。

- Throwing function parameters will not be flagged as not throwing an anonymous temporary. This allows helper functions for throwing.  
  抛出函数参数不会被标记为未抛出匿名临时对象。这允许使用辅助函数进行异常抛出。

- Re-throwing caught exception variables will not be flagged as not throwing an anonymous temporary. Although this can usually be done by just writing throw; it happens often enough in real code.  
  重新抛出已捕获的异常变量不会被标记为未抛出匿名临时对象。尽管通常可以通过直接写 throw; 来实现，但在实际代码中这种写法相当常见。

## Options

## 选项

::: option
CheckThrowTemporaries

Triggers detection of violations of the CERT recommendation ERR09-CPP. Throw anonymous temporaries. Default is true.
:::

::: option
CheckThrowTemporaries

启用对违反 CERT 建议 ERR09-CPP（抛出匿名临时对象）的检测。默认值为 true。
:::

::: option
WarnOnLargeObject

Also warns for any large, trivial object caught by value. Catching a large object by value is not dangerous but affects the performance negatively. The maximum size of an object allowed to be caught without warning can be set using the MaxSize option. Default is false.
:::

::: option
WarnOnLargeObject

对于按值捕获的大型平凡对象也会发出警告。按值捕获大型对象虽然不危险，但会对性能产生负面影响。可使用 MaxSize 选项设置允许无警告捕获的对象最大尺寸。默认值为 false。
:::

::: option
MaxSize

Determines the maximum size of an object allowed to be caught without warning. Only applicable if WarnOnLargeObject is set to true. If the option is set by the user to std::numeric_limits<uint64_t>::max() then it reverts to the default value. Default is the size of size_t.
:::

::: option
MaxSize

确定允许无警告捕获的对象的最大尺寸。仅当 WarnOnLargeObject 设置为 true 时适用。如果用户将该选项设置为 std::numeric_limits<uint64_t>::max()，则会恢复为默认值。默认值为 size_t 的大小。
:::
