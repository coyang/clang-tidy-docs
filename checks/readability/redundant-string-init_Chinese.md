# readability-redundant-string-init

Finds unnecessary string initializations.

查找不必要的字符串初始化。

## Examples

## 示例

```c++
// Initializing string with empty string literal is unnecessary.
std::string a = "";
std::string b("");

// becomes

std::string a;
std::string b;

// Initializing a string_view with an empty string literal produces an
// instance that compares equal to string_view().
std::string_view a = "";
std::string_view b("");

// becomes
std::string_view a;
std::string_view b;
```

```c++
// 使用空字符串字面量初始化字符串是不必要的。
std::string a = "";
std::string b("");

// 可简化为

std::string a;
std::string b;

// 使用空字符串字面量初始化 string_view 会生成一个
// 与默认构造的 string_view() 相等的实例。
std::string_view a = "";
std::string_view b("");

// 可简化为
std::string_view a;
std::string_view b;
```

## Options

## 选项

::: option  
StringNames

Default is [::std::basic_string;::std::basic_string_view]{.title-ref}.

默认值为 [::std::basic_string;::std::basic_string_view]{.title-ref}。

Semicolon-delimited list of class names to apply this check to. By  
default [::std::basic_string]{.title-ref} applies to std::string and  
std::wstring. Set to e.g.  
[::std::basic_string;llvm::StringRef;QString]{.title-ref} to perform  
this check on custom classes.

用分号分隔的类名列表，用于指定要应用此检查的类。默认情况下，[::std::basic_string]{.title-ref} 应用于 std::string 和 std::wstring。  
例如设置为 [::std::basic_string;llvm::StringRef;QString]{.title-ref}，以对自定义类执行此检查。
:::
