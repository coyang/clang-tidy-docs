# altera-single-work-item-barrier

Finds OpenCL kernel functions that call a barrier function but do not
call an ID function (get_local_id, get_local_id, get_group_id, or
get_local_linear_id).

查找调用 barrier 函数但未调用 ID 函数（如 get_local_id、get_group_id 或 get_local_linear_id）的 OpenCL 内核函数。

These kernels may be viable single work-item kernels, but will be forced
to execute as NDRange kernels if using a newer version of the Altera
Offline Compiler (>= v17.01).

这些内核可能是可行的单工作项内核，但如果使用较新版本的 Altera 离线编译器（>= v17.01），将被强制作为 NDRange 内核执行。

If using an older version of the Altera Offline Compiler, these kernel
functions will be treated as single work-item kernels, which could be
inefficient or lead to errors if NDRange semantics were intended.

如果使用较旧版本的 Altera 离线编译器，这些内核函数将被视为单工作项内核，如果原本意图使用 NDRange 语义，可能会导致效率低下或错误。

Based on the Altera SDK for OpenCL: Best Practices
Guide.

基于《Altera OpenCL SDK 最佳实践指南》。

Examples:

示例：

```c++
// error: function calls barrier but does not call an ID function.
// 错误：函数调用了 barrier，但未调用任何 ID 函数。
void __kernel barrier_no_id(__global int * foo, int size) {
  for (int i = 0; i < 100; i++) {
    foo[i] += 5;
  }
  barrier(CLK_GLOBAL_MEM_FENCE);
}

// ok: function calls barrier and an ID function.
// 正确：函数调用了 barrier 和一个 ID 函数。
void __kernel barrier_with_id(__global int * foo, int size) {
  for (int i = 0; i < 100; i++) {
    int tid = get_global_id(0);
    foo[tid] += 5;
  }
  barrier(CLK_GLOBAL_MEM_FENCE);
}

// ok with AOC Version 17.01: the reqd_work_group_size turns this into
// an NDRange.
// 在 AOC 版本 17.01 中是正确的：reqd_work_group_size 将其转换为 NDRange。
__attribute__((reqd_work_group_size(2,2,2)))
void __kernel barrier_with_id(__global int * foo, int size) {
  for (int i = 0; i < 100; i++) {
    foo[tid] += 5;
  }
  barrier(CLK_GLOBAL_MEM_FENCE);
}
```

## Options

## 选项

::: option
AOCVersion

Defines the version of the Altera Offline Compiler. Defaults to 1600
(corresponding to version 16.00).

定义 Altera 离线编译器的版本。默认值为 1600（对应版本 16.00）。
:::
