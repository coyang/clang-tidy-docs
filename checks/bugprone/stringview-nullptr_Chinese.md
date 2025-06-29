# bugprone-stringview-nullptr

Checks for various ways that the const CharT\* constructor of std::basic_string_view can be passed a null argument and replaces them with the default constructor in most cases. For the comparison operators, braced initializer list does not compile so instead a call to .empty() or the empty string literal are used, where appropriate.

检查将 null 参数传递给 std::basic_string_view 的 const CharT\* 构造函数的各种方式，并在大多数情况下将其替换为默认构造函数。对于比较运算符，由于使用大括号初始化列表无法编译，因此在适当的情况下使用 .empty() 调用或空字符串字面量来替代。

This prevents code from invoking behavior which is unconditionally undefined. The single-argument const CharT\* constructor does not check for the null case before dereferencing its input. The standard is slated to add an explicitly-deleted overload to catch some of these cases: wg21.link/p2166

这可以防止代码触发无条件未定义的行为。单参数 const CharT\* 构造函数在解引用其输入之前不会检查是否为 null。C++ 标准计划添加一个显式删除的重载来捕捉这些情况：wg21.link/p2166

To catch the additional cases of NULL (which expands to \_\_null) and 0, first run the modernize-use-nullptr check to convert the callers to nullptr.

为了捕捉使用 NULL（展开为 \_\_null）和 0 的额外情况，应首先运行 modernize-use-nullptr 检查，将调用者转换为 nullptr。

代码示例：

```c++
std::string_view sv = nullptr;

sv = nullptr;

bool is_empty = sv == nullptr;
bool isnt_empty = sv != nullptr;

accepts_sv(nullptr);

accepts_sv({{}});  // A

accepts_sv({nullptr, 0});  // B
```

is translated into...

被转换为……

```c++
std::string_view sv = {};

sv = {};

bool is_empty = sv.empty();
bool isnt_empty = !sv.empty();

accepts_sv("");

accepts_sv("");  // A

accepts_sv({nullptr, 0});  // B
```

::: note

The source pattern with trailing comment "A" selects the (const CharT\*) constructor overload and then value-initializes the pointer, causing a null dereference. It happens to not include the nullptr literal, but it is still within the scope of this ClangTidy check.

带有注释 "A" 的源代码模式选择了 (const CharT\*) 构造函数重载，并对指针进行了值初始化，导致了空指针解引用。尽管它没有直接包含 nullptr 字面量，但仍属于本 ClangTidy 检查的范围。

:::

::: note

The source pattern with trailing comment "B" selects the (const CharT\*, size_type) constructor which is perfectly valid, since the length argument is 0. It is not changed by this ClangTidy check.

带有注释 "B" 的源代码模式选择了 (const CharT\*, size_type) 构造函数，这是完全合法的，因为长度参数为 0。因此该模式不会被此 ClangTidy 检查修改。

:::
