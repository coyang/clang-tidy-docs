# bugprone-dangling-handle

Detect dangling references in value handles like std::string_view.  
检测像 std::string_view 这样的值句柄中的悬空引用。

These dangling references can be a result of constructing handles from temporary values, where the temporary is destroyed soon after the handle is created.  
这些悬空引用可能是由于从临时值构造句柄而导致的，在句柄创建后临时值很快被销毁。

Examples:  
示例：

```c++
string_view View = string();  // View will dangle.
string A;
View = A + "A";  // still dangle.

vector<string_view> V;
V.push_back(string());  // V[0] is dangling.
V.resize(3, string());  // V[1] and V[2] will also dangle.

string_view f() {
  // All these return values will dangle.
  return string();
  string S;
  return S;
  char Array[10]{};
  return Array;
}

span<int> g() {
  array<int, 1> V;
  return {V};
  int Array[10]{};
  return {Array};
}
```

## Options

## 选项

::: option  
HandleClasses

A semicolon-separated list of class names that should be treated as handles. By default only std::basic_string_view, std::experimental::basic_string_view and std::span are considered.  
一个以分号分隔的类名列表，这些类将被视为句柄。默认情况下，仅考虑 std::basic_string_view、std::experimental::basic_string_view 和 std::span。  
:::
