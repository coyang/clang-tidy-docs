# readability-string-compare

Finds string comparisons using the compare method.

查找使用 compare 方法进行字符串比较的情况。

A common mistake is to use the string's compare method instead of using the equality or inequality operators. The compare method is intended for sorting functions and thus returns a negative number, a positive number or zero depending on the lexicographical relationship between the strings compared. If an equality or inequality check can suffice, that is recommended. This is recommended to avoid the risk of incorrect interpretation of the return value and to simplify the code. The string equality and inequality operators can also be faster than the compare method due to early termination.

一个常见的错误是使用字符串的 compare 方法来进行比较，而不是使用等于或不等于运算符。compare 方法主要用于排序函数，因此它会根据被比较字符串之间的字典序关系返回一个负数、正数或零。如果使用等于或不等于判断就足够了，建议使用它们。这样做可以避免对返回值的错误解释，并简化代码。由于可以提前终止，字符串的等于和不等于运算符在性能上也可能优于 compare 方法。

## Example

示例

```c++
// The same rules apply to std::string_view.
std::string str1{"a"};
std::string str2{"b"};

// use str1 != str2 instead.
// 应使用 str1 != str2 替代。
if (str1.compare(str2)) {
}

// use str1 == str2 instead.
// 应使用 str1 == str2 替代。
if (!str1.compare(str2)) {
}

// use str1 == str2 instead.
// 应使用 str1 == str2 替代。
if (str1.compare(str2) == 0) {
}

// use str1 != str2 instead.
// 应使用 str1 != str2 替代。
if (str1.compare(str2) != 0) {
}

// use str1 == str2 instead.
// 应使用 str1 == str2 替代。
if (0 == str1.compare(str2)) {
}

// use str1 != str2 instead.
// 应使用 str1 != str2 替代。
if (0 != str1.compare(str2)) {
}

// Use str1 == "foo" instead.
// 应使用 str1 == "foo" 替代。
if (str1.compare("foo") == 0) {
}
```

The above code examples show the list of if-statements that this check will give a warning for. All of them use compare to check equality or inequality of two strings instead of using the correct operators.

上述代码示例展示了该检查器会发出警告的 if 语句列表。所有这些语句都使用 compare 方法来判断两个字符串是否相等或不等，而不是使用正确的运算符。

## Options

选项

::: option
StringLikeClasses

A string containing semicolon-separated names of string-like classes. By default contains only ::std::basic_string and ::std::basic_string_view. If a class from this list has a compare method similar to that of std::string, it will be checked in the same way.

一个包含以分号分隔的类名的字符串，这些类名表示类似字符串的类。默认值仅包含 ::std::basic_string 和 ::std::basic_string_view。如果列表中的某个类具有类似于 std::string 的 compare 方法，则会以相同方式进行检查。
:::

### Example

示例

```c++
struct CustomString {
public:
  int compare (const CustomString& other) const;
}

CustomString str1;
CustomString str2;

// use str1 != str2 instead.
// 应使用 str1 != str2 替代。
if (str1.compare(str2)) {
}
```

If StringLikeClasses contains CustomString, the check will suggest replacing compare with equality operator.

如果 StringLikeClasses 包含 CustomString，该检查器将建议用等于运算符替换 compare 方法。
