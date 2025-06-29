# bugprone-fold-init-type

The check flags type mismatches in folds like std::accumulate that might result in loss of precision. std::accumulate folds an input range into an initial value using the type of the latter, with operator+ by default. This can cause loss of precision through:

该检查器会标记在折叠操作（如 std::accumulate）中可能导致精度丢失的类型不匹配问题。std::accumulate 会使用初始值的类型将输入范围折叠为一个值，默认使用 operator+。这可能会通过以下方式导致精度丢失：

- Truncation: The following code uses a floating point range and an int initial value, so truncation will happen at every application of operator+ and the result will be 0, which might not be what the user expected.

- 截断：以下代码使用了一个浮点数范围和一个 int 类型的初始值，因此在每次应用 operator+ 时都会发生截断，最终结果将是 0，这可能不是用户所期望的。

```c++
auto a = {0.5f, 0.5f, 0.5f, 0.5f};
return std::accumulate(std::begin(a), std::end(a), 0);
```

- Overflow: The following code also returns 0.

- 溢出：以下代码同样返回 0。

```c++
auto a = {65536LL * 65536 * 65536};
return std::accumulate(std::begin(a), std::end(a), 0);
```
