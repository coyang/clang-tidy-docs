# cppcoreguidelines-no-malloc

This check handles C-Style memory management using malloc(), realloc(), calloc() and free(). It warns about its use and tries to suggest the use of an appropriate RAII object. Furthermore, it can be configured to check against a user-specified list of functions that are used for memory management (e.g. posix_memalign()).

本检查项处理使用 malloc()、realloc()、calloc() 和 free() 的 C 风格内存管理。它会对这些用法发出警告，并尝试建议使用合适的 RAII（资源获取即初始化）对象。此外，它还可以配置为检查用户指定的用于内存管理的函数列表（例如 posix_memalign()）。

This check implements R.10 from the C++ Core Guidelines.

本检查项实现了 C++ 核心指南中的 R.10 条款。

There is no attempt made to provide fix-it hints, since manual resource management isn't easily transformed automatically into RAII.

由于手动资源管理无法轻易自动转换为 RAII，因此本检查项不会尝试提供自动修复建议。

```c++
// Warns each of the following lines.
// Containers like std::vector or std::string should be used.
char* some_string = (char*) malloc(sizeof(char) * 20);
char* some_string = (char*) realloc(sizeof(char) * 30);
free(some_string);

int* int_array = (int*) calloc(30, sizeof(int));

// Rather use a smartpointer or stack variable.
struct some_struct* s = (struct some_struct*) malloc(sizeof(struct some_struct));
```

```c++
// 以下每一行都会触发警告。
// 应该使用如 std::vector 或 std::string 等容器。
char* some_string = (char*) malloc(sizeof(char) * 20);
char* some_string = (char*) realloc(sizeof(char) * 30);
free(some_string);

int* int_array = (int*) calloc(30, sizeof(int));

// 应该使用智能指针或栈变量。
struct some_struct* s = (struct some_struct*) malloc(sizeof(struct some_struct));
```

## Options

## 选项

::: option
Allocations

Semicolon-separated list of fully qualified names of memory allocation functions. Defaults to [::malloc;::calloc]{.title-ref}.

内存分配函数的全限定名称列表，使用分号分隔。默认值为 [::malloc;::calloc]{.title-ref}。
:::

::: option
Deallocations

Semicolon-separated list of fully qualified names of memory allocation functions. Defaults to [::free]{.title-ref}.

内存释放函数的全限定名称列表，使用分号分隔。默认值为 [::free]{.title-ref}。
:::

::: option
Reallocations

Semicolon-separated list of fully qualified names of memory allocation functions. Defaults to [::realloc]{.title-ref}.

内存重新分配函数的全限定名称列表，使用分号分隔。默认值为 [::realloc]{.title-ref}。
:::
