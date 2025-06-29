# abseil-cleanup-ctad

Suggests switching the initialization pattern of absl::Cleanup instances from the factory function to class template argument deduction (CTAD), in C++17 and higher.

建议在 C++17 及更高版本中，将 absl::Cleanup 实例的初始化方式从工厂函数切换为类模板参数推导（CTAD）。

```c++
auto c1 = absl::MakeCleanup([] {});
```

```c++
auto c1 = absl::MakeCleanup([] {});
```

变为

Becomes

```c++
absl::Cleanup c1 = [] {};
```

```c++
absl::Cleanup c1 = [] {};
```

```c++
const auto c2 = absl::MakeCleanup(std::function<void()>([] {}));
```

```c++
const auto c2 = absl::MakeCleanup(std::function<void()>([] {}));
```

变为

Becomes

```c++
const absl::Cleanup c2 = std::function<void()>([] {});
```

```c++
const absl::Cleanup c2 = std::function<void()>([] {});
```
