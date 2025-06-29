# performance-inefficient-vector-operation

Finds possible inefficient std::vector operations (e.g. push_back, emplace_back) that may cause unnecessary memory reallocations.

查找可能低效的 std::vector 操作（例如 push_back、emplace_back），这些操作可能导致不必要的内存重新分配。

It can also find calls that add element to protobuf repeated field in a loop without calling Reserve() before the loop. Calling Reserve() first can avoid unnecessary memory reallocations.

它还可以检测在循环中向 protobuf 的 repeated 字段添加元素时未在循环前调用 Reserve() 的情况。提前调用 Reserve() 可以避免不必要的内存重新分配。

Currently, the check only detects following kinds of loops with a single statement body:

目前，该检查仅检测以下类型的循环，且循环体仅包含单个语句：

- Counter-based for loops start with 0:

- 从 0 开始的基于计数器的 for 循环：

```c++
std::vector<int> v;
for (int i = 0; i < n; ++i) {
  v.push_back(n);
  // This will trigger the warning since the push_back may cause multiple
  // memory reallocations in v. This can be avoid by inserting a 'reserve(n)'
  // statement before the for statement.
  // 这将触发警告，因为 push_back 可能会导致 v 中多次内存重新分配。
  // 可以通过在 for 语句之前插入 'reserve(n)' 语句来避免这种情况。
}

SomeProto p;
for (int i = 0; i < n; ++i) {
  p.add_xxx(n);
  // This will trigger the warning since the add_xxx may cause multiple memory
  // reallocations. This can be avoid by inserting a
  // 'p.mutable_xxx().Reserve(n)' statement before the for statement.
  // 这将触发警告，因为 add_xxx 可能会导致多次内存重新分配。
  // 可以通过在 for 语句之前插入 'p.mutable_xxx().Reserve(n)' 语句来避免这种情况。
}
```

- For-range loops like for (range-declaration : range_expression), the type of range_expression can be std::vector, std::array, std::deque, std::set, std::unordered_set, std::map, std::unordered_set:

- 范围 for 循环，例如 for (range-declaration : range_expression)，其中 range_expression 的类型可以是 std::vector、std::array、std::deque、std::set、std::unordered_set、std::map、std::unordered_set：

```c++
std::vector<int> data;
std::vector<int> v;

for (auto element : data) {
  v.push_back(element);
  // This will trigger the warning since the 'push_back' may cause multiple
  // memory reallocations in v. This can be avoid by inserting a
  // 'reserve(data.size())' statement before the for statement.
  // 这将触发警告，因为 'push_back' 可能会导致 v 中多次内存重新分配。
  // 可以通过在 for 语句之前插入 'reserve(data.size())' 语句来避免这种情况。
}
```

## Options

## 选项

::: option
VectorLikeClasses

Semicolon-separated list of names of vector-like classes. By default only ::std::vector is considered.

以分号分隔的类似 vector 的类名列表。默认情况下，仅考虑 ::std::vector。

:::

::: option
EnableProto

When true, the check will also warn on inefficient operations for proto repeated fields. Otherwise, the check only warns on inefficient vector operations. Default is false.

当为 true 时，该检查还会对 protobuf repeated 字段的低效操作发出警告。否则，仅对 vector 的低效操作发出警告。默认值为 false。

:::
