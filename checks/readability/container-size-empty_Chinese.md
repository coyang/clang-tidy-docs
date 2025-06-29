以下是翻译后的 markdown 文件内容，保留了中英文双语，中文在英文下方，并以一个空行分隔，同时保留了代码块格式：

# readability-container-size-empty

Checks whether a call to the size()/length() method can be replaced with a call to empty().

检查是否可以将对 size()/length() 方法的调用替换为对 empty() 方法的调用。

The emptiness of a container should be checked using the empty() method instead of the size()/length() method. It shows clearer intent to use empty(). Furthermore some containers (for example, a std::forward_list) may implement the empty() method but not implement the size() or length() method. Using empty() whenever possible makes it easier to switch to another container in the future.

应使用 empty() 方法来检查容器是否为空，而不是使用 size()/length() 方法。使用 empty() 可以更清楚地表达意图。此外，一些容器（例如 std::forward_list）可能实现了 empty() 方法，但没有实现 size() 或 length() 方法。在可能的情况下使用 empty() 可以使将来更容易切换到其他容器。

The check issues warning if a container has empty() and size() or length() methods matching following signatures:

如果一个容器同时具有符合以下签名的 empty() 和 size() 或 length() 方法，则此检查会发出警告：

```c++
size_type size() const;
size_type length() const;
bool empty() const;
```

size_type 可以是任何类型的整数。

## Options

## 选项

::: option
ExcludedComparisonTypes

A semicolon-separated list of class names for which the check will ignore comparisons of objects with default-constructed objects of the same type. If a class is listed here, the check will not suggest using empty() instead of such comparisons for objects of that class. Default value is: ::std::array.

一个以分号分隔的类名列表，对于这些类，该检查将忽略与同类型默认构造对象的比较。如果某个类包含在此列表中，则该检查不会建议将此类对象的比较替换为使用 empty()。默认值为：::std::array。
:::
