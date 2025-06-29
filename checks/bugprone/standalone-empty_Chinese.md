# bugprone-standalone-empty

Warns when empty() is used on a range and the result is ignored.  
当对一个范围调用 empty() 并忽略其返回结果时发出警告。

Suggests clear() if it is an existing member function.  
如果该范围有 clear() 成员函数，则建议使用 clear()。

The empty() method on several common ranges returns a Boolean indicating whether or not the range is empty, but is often mistakenly interpreted as a way to clear the contents of a range. Some ranges offer a clear() method for this purpose. This check warns when a call to empty returns a result that is ignored, and suggests replacing it with a call to clear() if it is available as a member function of the range.  
在多个常见的范围类型中，empty() 方法返回一个布尔值，用于指示该范围是否为空，但它经常被错误地理解为清空范围内容的方法。某些范围类型提供了 clear() 方法用于清空内容。此检查会在调用 empty() 并忽略其返回结果时发出警告，并在该范围类型提供 clear() 成员函数的情况下，建议使用 clear() 代替。

For example, the following code could be used to indicate whether a range is empty or not, but the result is ignored:  
例如，以下代码用于判断一个范围是否为空，但其返回结果被忽略了：

```c++
std::vector<int> v;
...
v.empty();
```

A call to clear() would appropriately clear the contents of the range:  
调用 clear() 可以正确地清空该范围的内容：

```c++
std::vector<int> v;
...
v.clear();
```

Limitations:  
限制：

- Doesn't warn if empty() is defined and used with the ignore result in the class template definition (for example in the library implementation). These error cases can be caught with [[nodiscard]] attribute.  
  如果 empty() 是在类模板定义中定义并使用的，并且其返回结果被忽略（例如在库的实现中），则不会发出警告。这类错误可以通过 [[nodiscard]] 属性来捕获。
