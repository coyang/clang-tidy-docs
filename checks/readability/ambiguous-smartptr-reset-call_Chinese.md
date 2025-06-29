# readability-ambiguous-smartptr-reset-call

Finds potentially erroneous calls to reset method on smart pointers when the pointee type also has a reset method. Having a reset method in both classes makes it easy to accidentally make the pointer null when intending to reset the underlying object.

查找在智能指针上调用 reset 方法时可能出现的错误调用，当被指向的类型也有一个 reset 方法时尤为如此。在两个类中都存在 reset 方法时，开发者很容易在本意是重置底层对象时，不小心将指针置空。

```c++
struct Resettable {
  void reset() { /* Own reset logic */ }
};

auto ptr = std::make_unique<Resettable>();

ptr->reset();  // Calls underlying reset method
ptr.reset();   // Makes the pointer null
```

Both calls are valid C++ code, but the second one might not be what the developer intended, as it destroys the pointed-to object rather than resetting its state. It's easy to make such a typo because the difference between . and -> is really small.

这两种调用在 C++ 中都是合法的，但第二种可能并不是开发者的本意，因为它销毁了被指向的对象，而不是重置其状态。由于 . 和 -> 之间的差异非常小，因此很容易出现这种笔误。

The recommended approach is to make the intent explicit by using either member access or direct assignment:

推荐的做法是通过成员访问或直接赋值来明确表达意图：

```c++
std::unique_ptr<Resettable> ptr = std::make_unique<Resettable>();

(*ptr).reset();  // Clearly calls underlying reset method
ptr = nullptr;   // Clearly makes the pointer null
```

The default smart pointers and classes that are considered are std::unique_ptr, std::shared_ptr, boost::shared_ptr. To specify other smart pointers or other classes use the SmartPointers option.

默认考虑的智能指针和类包括 std::unique_ptr、std::shared_ptr 和 boost::shared_ptr。要指定其他智能指针或类，请使用 SmartPointers 选项。

::: note

The check may emit invalid fix-its and misleading warning messages when specifying custom smart pointers or other classes in the SmartPointers 选项. For example, boost::scoped_ptr does not have an operator= which makes fix-its invalid.

当在 SmartPointers 选项中指定自定义智能指针或其他类时，该检查可能会生成无效的修复建议和误导性的警告信息。例如，boost::scoped_ptr 没有 operator=，这会导致修复建议无效。

:::

::: note

Automatic fix-its are enabled only if clang-tidy is invoked with the --fix-notes option.

仅当使用 --fix-notes 选项调用 clang-tidy 时，才会启用自动修复建议。

:::

## Options

## 选项

::: option
SmartPointers

Semicolon-separated list of fully qualified class names of custom smart pointers. Default value is ::std::unique_ptr;::std::shared_ptr;::boost::shared_ptr.

以分号分隔的自定义智能指针的完全限定类名列表。默认值为 ::std::unique_ptr;::std::shared_ptr;::boost::shared_ptr。

:::
