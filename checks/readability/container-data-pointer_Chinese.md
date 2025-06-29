# readability-container-data-pointer

Finds cases where code could use data() rather than the address of the element at index 0 in a container. This pattern is commonly used to materialize a pointer to the backing data of a container. std::vector and std::string provide a data() accessor to retrieve the data pointer which should be preferred.

查找代码中可以使用 data() 而不是容器中索引为 0 的元素地址的情况。这种模式通常用于获取容器底层数据的指针。std::vector 和 std::string 提供了 data() 访问器来获取数据指针，应该优先使用。

This also ensures that in the case that the container is empty, the data pointer access does not perform an errant memory access.

这也可以确保在容器为空的情况下，访问数据指针不会导致错误的内存访问。

## Options

## 选项

::: option
IgnoredContainers

Semicolon-separated list of containers regexp for which this check won't be enforced. Default is an empty string.

以分号分隔的容器正则表达式列表，对于这些容器将不会强制执行此检查。默认值为空字符串。
:::
