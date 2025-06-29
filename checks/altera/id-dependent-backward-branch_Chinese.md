# altera-id-dependent-backward-branch

Finds ID-dependent variables and fields that are used within loops. This causes branches to occur inside the loops, and thus leads to performance degradation.

查找在循环中使用的依赖于线程 ID 的变量和字段。这会导致在循环内部出现分支，从而导致性能下降。

```c++
// The following code will produce a warning because this ID-dependent
// variable is used in a loop condition statement.
int ThreadID = get_local_id(0);

// 以下代码会产生警告，因为这个依赖于线程 ID 的变量被用于循环条件语句中。

// The following loop will produce a warning because the loop condition
// statement depends on an ID-dependent variable.
for (int i = 0; i < ThreadID; ++i) {
  std::cout << i << std::endl;
}

// 以下循环会产生警告，因为循环条件语句依赖于一个依赖线程 ID 的变量。

// The following loop will not produce a warning, because the ID-dependent
// variable is not used in the loop condition statement.
for (int i = 0; i < 100; ++i) {
  std::cout << ThreadID << std::endl;
}

// 以下循环不会产生警告，因为依赖线程 ID 的变量没有被用于循环条件语句中。
```

Based on the [Altera SDK for OpenCL: Best Practices Guide](https://www.altera.com/en_US/pdfs/literature/hb/opencl-sdk/aocl_optimization_guide.pdf).

基于 [Altera SDK for OpenCL：最佳实践指南](https://www.altera.com/en_US/pdfs/literature/hb/opencl-sdk/aocl_optimization_guide.pdf)。
