# mpi-type-mismatch

This check verifies if buffer type and MPI (Message Passing Interface)  
datatype pairs match for used MPI functions. All MPI datatypes defined  
by the MPI standard (3.1) are verified by this check. User defined  
typedefs, custom MPI datatypes and null pointer constants are skipped,  
in the course of verification.

此检查用于验证在使用 MPI（消息传递接口）函数时，缓冲区类型与 MPI 数据类型是否匹配。该检查会验证 MPI 标准（3.1）中定义的所有 MPI 数据类型。用户自定义的 typedef、自定义 MPI 数据类型以及空指针常量在验证过程中会被跳过。

Example:  
示例：

```c++
// In this case, the buffer type matches MPI datatype.
char buf;
MPI_Send(&buf, 1, MPI_CHAR, 0, 0, MPI_COMM_WORLD);

// 在此示例中，缓冲区类型与 MPI 数据类型匹配。
char buf;
MPI_Send(&buf, 1, MPI_CHAR, 0, 0, MPI_COMM_WORLD);

// In the following case, the buffer type does not match MPI datatype.
int buf;
MPI_Send(&buf, 1, MPI_CHAR, 0, 0, MPI_COMM_WORLD);

// 在下一个示例中，缓冲区类型与 MPI 数据类型不匹配。
int buf;
MPI_Send(&buf, 1, MPI_CHAR, 0, 0, MPI_COMM_WORLD);
```
