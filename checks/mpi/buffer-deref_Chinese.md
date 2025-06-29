# mpi-buffer-deref

This check verifies if a buffer passed to an MPI (Message Passing Interface) function is sufficiently dereferenced. Buffers should be passed as a single pointer or array. As MPI function signatures specify void \* for their buffer types, insufficiently dereferenced buffers can be passed, like for example as double pointers or multidimensional arrays, without a compiler warning emitted.

该检查用于验证传递给 MPI（消息传递接口）函数的缓冲区是否被充分解引用。缓冲区应作为单个指针或数组传递。由于 MPI 函数的签名将缓冲区类型指定为 void \*，因此即使传递了未充分解引用的缓冲区（例如双重指针或多维数组），编译器也不会发出警告。

Examples:

示例：

```c++
// A double pointer is passed to the MPI function.
// 向 MPI 函数传递了一个双重指针。
char *buf;
MPI_Send(&buf, 1, MPI_CHAR, 0, 0, MPI_COMM_WORLD);

// A multidimensional array is passed to the MPI function.
// 向 MPI 函数传递了一个多维数组。
short buf[1][1];
MPI_Send(buf, 1, MPI_SHORT, 0, 0, MPI_COMM_WORLD);

// A pointer to an array is passed to the MPI function.
// 向 MPI 函数传递了一个指向数组的指针。
short *buf[1];
MPI_Send(buf, 1, MPI_SHORT, 0, 0, MPI_COMM_WORLD);
```
