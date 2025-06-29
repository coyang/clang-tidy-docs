# performance-inefficient-string-concatenation

# 性能低效的字符串连接

This check warns about the performance overhead arising from concatenating strings using the operator+, for instance:

此检查会警告使用 operator+ 连接字符串所带来的性能开销，例如：

```c++
std::string a("Foo"), b("Bar");
a = a + b;
```

Instead of this structure you should use operator+= or std::string's (std::basic_string) class member function append(). For instance:

你应该使用 operator+= 或 std::string（std::basic_string）的成员函数 append() 来代替这种结构。例如：

```c++
std::string a("Foo"), b("Baz");
for (int i = 0; i < 20000; ++i) {
    a = a + "Bar" + b;
}
```

Could be rewritten in a greatly more efficient way like:

可以重写为更高效的方式，如下所示：

```c++
std::string a("Foo"), b("Baz");
for (int i = 0; i < 20000; ++i) {
    a.append("Bar").append(b);
}
```

And this can be rewritten too:

这段代码也可以重写为：

```c++
void f(const std::string&) {}
std::string a("Foo"), b("Baz");
void g() {
    f(a + "Bar" + b);
}
```

In a slightly more efficient way like:

可以稍微更高效地重写为：

```c++
void f(const std::string&) {}
std::string a("Foo"), b("Baz");
void g() {
    f(std::string(a).append("Bar").append(b));
}
```

## Options

## 选项

::: option
StrictMode

When false, the check will only check the string usage in while, for and for-range statements. Default is false.

当为 false 时，此检查仅会检查 while、for 和 for-range 语句中的字符串使用情况。默认值为 false。
:::
