# boost-use-ranges

Detects calls to standard library iterator algorithms that could be replaced with a Boost ranges version instead.

检测可以替换为 Boost ranges 版本的标准库迭代器算法调用。

## Example

示例

```c++
auto Iter1 = std::find(Items.begin(), Items.end(), 0);
auto AreSame = std::equal(Items1.cbegin(), Items1.cend(), std::begin(Items2),
                          std::end(Items2));
```

Transforms to:

转换为：

```c++
auto Iter1 = boost::range::find(Items, 0);
auto AreSame = boost::range::equal(Items1, Items2);
```

## Supported algorithms

支持的算法

Calls to the following std library algorithms are checked:

以下标准库算法的调用将被检查：

std::accumulate, std::adjacent_difference, std::adjacent_find,  
std::all_of, std::any_of, std::binary_search,  
std::copy_backward, std::copy_if, std::copy, std::count_if,  
std::count, std::equal_range, std::equal, std::fill,  
std::find_end, std::find_first_of, std::find_if_not,  
std::find_if, std::find, std::for_each, std::generate,  
std::includes, std::iota, std::is_partitioned,  
std::is_permutation, std::is_sorted_until, std::is_sorted,  
std::lexicographical_compare, std::lower_bound, std::make_heap,  
std::max_element, std::merge, std::min_element, std::mismatch,  
std::next_permutation, std::none_of, std::partial_sum,  
std::partial_sort_copy, std::partition_copy, std::partition_point,  
std::partition, std::pop_heap, std::prev_permutation,  
std::push_heap, std::random_shuffle, std::reduce,  
std::remove_copy_if, std::remove_copy, std::remove_if,  
std::remove, std::replace_copy_if, std::replace_copy,  
std::replace_if, std::replace, std::reverse_copy, std::reverse,  
std::search, std::set_difference, std::set_intersection,  
std::set_symmetric_difference, std::set_union, std::sort_heap,  
std::sort, std::stable_partition, std::stable_sort,  
std::transform, std::unique_copy, std::unique, std::upper_bound.

The check will also look for the following functions from the boost::algorithm namespace:

检查还会查找 boost::algorithm 命名空间中的以下函数：

all_of_equal, any_of_equal, any_of, apply_permutation,  
apply_reverse_permutation, clamp_range, copy_if_until,  
copy_if_while, copy_if, copy_until, copy_while, find_backward,  
find_if_backward, find_if_not_backward, find_if_not,  
find_not_backward, hex_lower, hex, iota, all_of,  
is_decreasing, is_increasing, is_palindrome,  
is_partitioned_until, is_partitioned, is_permutation,  
is_sorted_until, is_sorted, is_strictly_decreasing,  
is_strictly_increasing, none_of_equal, none_of, one_of_equal,  
one_of, partition_copy, partition_point, reduce, unhex.

## Reverse Iteration

反向迭代

If calls are made using reverse iterators on containers, The code will be fixed using the boost::adaptors::reverse adaptor.

如果在容器上使用反向迭代器进行调用，代码将使用 boost::adaptors::reverse 适配器进行修复。

```c++
auto AreSame = std::equal(Items1.rbegin(), Items1.rend(),
                          std::crbegin(Items2), std::crend(Items2));
```

Transforms to:

转换为：

```c++
auto AreSame = boost::range::equal(boost::adaptors::reverse(Items1),
                                   boost::adaptors::reverse(Items2));
```

## Options

选项

::: option
IncludeStyle

A string specifying which include-style is used, llvm or google. Default is llvm.

一个字符串，用于指定使用哪种 include 风格，llvm 或 google。默认值为 llvm。
:::

::: option
IncludeBoostSystem

If true (default value) the boost headers are included as system headers with angle brackets (#include <boost.hpp>), otherwise quotes are used (#include "boost.hpp").

如果为 true（默认值），则 boost 头文件将作为系统头文件使用尖括号包含（#include <boost.hpp>），否则使用引号包含（#include "boost.hpp"）。
:::

::: option
UseReversePipe

When true (default false), fixes which involve reverse ranges will use the pipe adaptor syntax instead of the function syntax.

当为 true（默认值为 false）时，涉及反向范围的修复将使用管道适配器语法，而不是函数语法。

```c++
std::find(Items.rbegin(), Items.rend(), 0);
```

Transforms to:

转换为：

```c++
boost::range::find(Items | boost::adaptors::reversed, 0);
```

:::
