# portability-simd-intrinsics

Finds SIMD intrinsics calls and suggests std::experimental::simd ([P0214](https://wg21.link/p0214)) alternatives.

查找 SIMD 内在函数调用，并建议使用 std::experimental::simd（[P0214](https://wg21.link/p0214)）作为替代方案。

If the option Suggest is set to true, for

如果选项 Suggest 设置为 true，对于如下代码：

```c++
_mm_add_epi32(a, b); // x86
vec_add(a, b);       // Power
```

the check suggests an alternative: operator+ on std::experimental::simd objects.

该检查器会建议使用 std::experimental::simd 对象上的 operator+ 作为替代方案。

Otherwise, it just complains the intrinsics are non-portable (and there are P0214 alternatives).

否则，它只会提示这些内在函数不可移植（并指出存在 P0214 的替代方案）。

Many architectures provide SIMD operations (e.g. x86 SSE/AVX, Power AltiVec/VSX, ARM NEON). It is common that SIMD code implementing the same algorithm, is written in multiple target-dispatching pieces to optimize for different architectures or micro-architectures.

许多体系结构提供 SIMD 操作（例如 x86 SSE/AVX、Power AltiVec/VSX、ARM NEON）。通常，为了优化不同的体系结构或微架构，实现相同算法的 SIMD 代码会被编写为多个针对目标平台分发的代码片段。

The C++ standard proposal P0214 and its extensions cover many common SIMD operations. By migrating from target-dependent intrinsics to P0214 operations, the SIMD code can be simplified and pieces for different targets can be unified.

C++ 标准提案 P0214 及其扩展涵盖了许多常见的 SIMD 操作。通过从依赖目标平台的内在函数迁移到 P0214 操作，可以简化 SIMD 代码，并统一面向不同目标平台的代码片段。

Refer to P0214 for introduction and motivation for the data-parallel standard library.

有关数据并行标准库的介绍和动机，请参阅 P0214。

## Options

## 选项

::: option
Suggest

If this option is set to true (default is false), the check will suggest P0214 alternatives, otherwise it only points out the intrinsic function is non-portable.

如果此选项设置为 true（默认值为 false），检查器将建议使用 P0214 的替代方案；否则，它只会指出该内在函数不可移植。

:::

::: option
Std

The namespace used to suggest P0214 alternatives. If not specified, std:: for -std=c++20 and std::experimental:: for -std=c++11.

用于建议 P0214 替代方案的命名空间。如果未指定，则在使用 -std=c++20 时为 std::，在使用 -std=c++11 时为 std::experimental::。

:::
