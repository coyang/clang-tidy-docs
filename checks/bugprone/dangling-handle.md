# bugprone-dangling-handle

Detect dangling references in value handles like `std::string_view`.
These dangling references can be a result of constructing handles from
temporary values, where the temporary is destroyed soon after the handle
is created.

Examples:

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

::: option
HandleClasses

A semicolon-separated list of class names that should be treated as
handles. By default only `std::basic_string_view`,
`std::experimental::basic_string_view` and `std::span` are considered.
:::
