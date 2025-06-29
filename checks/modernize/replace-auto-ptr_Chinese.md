# modernize-replace-auto-ptr

# modernize-replace-auto-ptr

This check replaces the uses of the deprecated class std::auto_ptr by std::unique_ptr (introduced in C++11). The transfer of ownership, done by the copy-constructor and the assignment operator, is changed to match std::unique_ptr usage by using explicit calls to std::move().

此检查会将已弃用的类 std::auto_ptr 替换为 std::unique_ptr（在 C++11 中引入）。通过拷贝构造函数和赋值操作符进行的所有权转移将被修改为使用 std::move() 显式调用，以符合 std::unique_ptr 的用法。

Migration example:

迁移示例：

```c++
-void take_ownership_fn(std::auto_ptr<int> int_ptr);
+void take_ownership_fn(std::unique_ptr<int> int_ptr);

 void f(int x) {
-  std::auto_ptr<int> a(new int(x));
-  std::auto_ptr<int> b;
+  std::unique_ptr<int> a(new int(x));
+  std::unique_ptr<int> b;

-  b = a;
-  take_ownership_fn(b);
+  b = std::move(a);
+  take_ownership_fn(std::move(b));
 }
```

Since std::move() is a library function declared in <utility> it may be necessary to add this include. The check will add the include directive when necessary.

由于 std::move() 是在 <utility> 中声明的库函数，因此可能需要添加该头文件。此检查将在必要时自动添加该 include 指令。

## Known Limitations

## 已知限制

- If headers modification is not activated or if a header is not allowed to be changed this check will produce broken code (compilation error), where the headers' code will stay unchanged while the code using them will be changed.

  如果未启用头文件修改，或某个头文件不允许被修改，则此检查可能会生成错误代码（编译错误），即头文件中的代码保持不变，而使用它们的代码已被更改。

- Client code that declares a reference to an std::auto_ptr coming from code that can't be migrated (such as a header coming from a 3rd party library) will produce a compilation error after migration. This is because the type of the reference will be changed to std::unique_ptr but the type returned by the library won't change, binding a reference to std::unique_ptr from an std::auto_ptr. This pattern doesn't make much sense and usually std::auto_ptr are stored by value (otherwise what is the point in using them instead of a reference or a pointer?).

  如果客户端代码声明了一个引用，引用的是来自无法迁移的代码（例如来自第三方库的头文件）的 std::auto_ptr，那么在迁移后将会产生编译错误。这是因为引用的类型会被更改为 std::unique_ptr，而库返回的类型仍然是 std::auto_ptr，导致试图将 std::auto_ptr 绑定到 std::unique_ptr 的引用上。这种用法本身并不合理，通常 std::auto_ptr 是以值的方式存储的（否则使用它而不是引用或指针就没有意义了）。

```c++
// <3rd-party header...>
std::auto_ptr<int> get_value();
const std::auto_ptr<int> & get_ref();

// <calling code (with migration)...>
-std::auto_ptr<int> a(get_value());
+std::unique_ptr<int> a(get_value()); // ok, unique_ptr constructed from auto_ptr

-const std::auto_ptr<int> & p = get_ptr();
+const std::unique_ptr<int> & p = get_ptr(); // won't compile
```

- Non-instantiated templates aren't modified.

  未实例化的模板不会被修改。

```c++
template <typename X>
void f() {
    std::auto_ptr<X> p;
}

// only 'f<int>()' (or similar) will trigger the replacement.
// 只有 'f<int>()'（或类似实例化）才会触发替换。
```

## Options

## 选项

::: option
IncludeStyle

A string specifying which include-style is used, llvm or google. Default is llvm.

一个字符串，用于指定使用哪种 include 风格，可选项为 llvm 或 google。默认值为 llvm。
:::
