# altera-struct-pack-align

Finds structs that are inefficiently packed or aligned, and recommends
packing and/or aligning of said structs as needed.

查找打包或对齐效率低下的结构体，并根据需要建议对这些结构体进行打包和/或对齐。

Structs that are not packed take up more space than they should, and
accessing structs that are not well aligned is inefficient.

未打包的结构体会占用比实际需要更多的内存空间，而未正确对齐的结构体在访问时效率较低。

Fix-its are provided to fix both of these issues by inserting and/or
amending relevant struct attributes.

通过插入和/或修改相关的结构体属性，提供修复建议以解决这两个问题。

Based on the Altera SDK for OpenCL: Best Practices Guide.

基于《Altera OpenCL SDK 最佳实践指南》。

```c++
// The following struct is originally aligned to 4 bytes, and thus takes up
// 12 bytes of memory instead of 10. Packing the struct will make it use
// only 10 bytes of memory, and aligning it to 16 bytes will make it
// efficient to access.

//
// 以下结构体原本按 4 字节对齐，因此占用了 12 字节内存，而不是 10 字节。
// 对该结构体进行打包后将只使用 10 字节内存，
// 并将其对齐到 16 字节可以提高访问效率。
struct example {
  char a;    // 1 byte
             // 1 字节
  double b;  // 8 bytes
             // 8 字节
  char c;    // 1 byte
             // 1 字节
};

// The following struct is arranged in such a way that packing is not needed.
// However, it is aligned to 4 bytes instead of 8, and thus needs to be
// explicitly aligned.

//
// 以下结构体的成员排列方式使其无需打包。
// 但它被对齐到 4 字节而不是 8 字节，因此需要显式对齐。
struct implicitly_packed_example {
  char a;  // 1 byte
           // 1 字节
  char b;  // 1 byte
           // 1 字节
  char c;  // 1 byte
           // 1 字节
  char d;  // 1 byte
           // 1 字节
  int e;   // 4 bytes
           // 4 字节
};

// The following struct is explicitly aligned and packed.

//
// 以下结构体已显式打包并对齐。
struct good_example {
  char a;    // 1 byte
             // 1 字节
  double b;  // 8 bytes
             // 8 字节
  char c;    // 1 byte
             // 1 字节
} __attribute__((packed)) __attribute__((aligned(16)));

// Explicitly aligning a struct to the wrong value will result in a warning.
// The following example should be aligned to 16 bytes, not 32.

//
// 将结构体显式对齐到错误的值会导致警告。
// 以下示例应对齐到 16 字节，而不是 32 字节。
struct badly_aligned_example {
  char a;    // 1 byte
             // 1 字节
  double b;  // 8 bytes
             // 8 字节
  char c;    // 1 byte
             // 1 字节
} __attribute__((packed)) __attribute__((aligned(32)));
```
