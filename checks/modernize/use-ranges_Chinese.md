# modernize-use-ranges

Detects calls to standard library iterator algorithms that could be replaced with a ranges version instead.

检测可以替换为 ranges 版本的标准库迭代器算法调用。

## Example

示例

```c++
auto Iter1 = std::find(Items.begin(), Items.end(), 0);
auto AreSame = std::equal(Items1.cbegin(), Items1.cend(),
                          std::begin(Items2), std::end(Items2));
```

Transforms to:  
转换为：

```c++
auto Iter1 = std::ranges::find(Items, 0);
auto AreSame = std::ranges::equal(Items1, Items2);
```

## Supported algorithms

支持的算法

Calls to the following std library algorithms are checked:  
以下标准库算法的调用将被检查：

std::adjacent_find、std::all_of、std::any_of、  
std::binary_search、std::copy_backward、std::copy_if、std::copy、  
std::destroy、std::equal_range、std::equal、std::fill、  
std::find_end、std::find_if_not、std::find_if、std::find、  
std::for_each、std::generate、std::includes、std::inplace_merge、  
std::iota、std::is_heap_until、std::is_heap、  
std::is_partitioned、std::is_permutation、std::is_sorted_until、  
std::is_sorted、std::lexicographical_compare、std::lower_bound、  
std::make_heap、std::max_element、std::merge、std::min_element、  
std::minmax_element、std::mismatch、std::move_backward、  
std::move、std::next_permutation、std::none_of、  
std::partial_sort_copy、std::partition_copy、std::partition_point、  
std::partition、std::pop_heap、std::prev_permutation、  
std::push_heap、std::remove_copy_if、std::remove_copy、  
std::remove、std::remove_if、std::replace_if、std::replace、  
std::reverse_copy、std::reverse、std::rotate、std::rotate_copy、  
std::sample、std::search、std::set_difference、  
std::set_intersection、std::set_symmetric_difference、  
std::set_union、std::shift_left、std::shift_right、  
std::sort_heap、std::sort、std::stable_partition、  
std::stable_sort、std::transform、std::uninitialized_copy、  
std::uninitialized_default_construct、std::uninitialized_fill、  
std::uninitialized_move、std::uninitialized_value_construct、  
std::unique_copy、std::unique、std::upper_bound。

Note: some range algorithms for vector<bool> require C++23 because it uses proxy iterators.  
注意：某些针对 vector<bool> 的 ranges 算法需要 C++23，因为它使用了代理迭代器。

## Reverse Iteration

反向迭代

If calls are made using reverse iterators on containers, The code will be fixed using the std::views::reverse adaptor.  
如果使用容器的反向迭代器进行调用，代码将使用 std::views::reverse 适配器进行修复。

```c++
auto AreSame = std::equal(Items1.rbegin(), Items1.rend(),
                          std::crbegin(Items2), std::crend(Items2));
```

Transforms to:  
转换为：

```c++
auto AreSame = std::ranges::equal(std::ranges::reverse_view(Items1),
                                  std::ranges::reverse_view(Items2));
```

## Options

选项

::: option  
IncludeStyle

A string specifying which include-style is used, llvm 或 google。默认值为 llvm。  
指定使用哪种 include 风格的字符串，可选值为 [llvm]{.title-ref} 或 [google]{.title-ref}。默认值为 [llvm]{.title-ref}。
:::

::: option  
UseReversePipe

When true（默认值为 false），涉及反向范围的修复将使用管道适配器语法而非函数语法。  
当设置为 [true]{.title-ref}（默认值为 [false]{.title-ref}）时，涉及反向范围的修复将使用管道适配器语法而不是函数语法。

```c++
std::find(Items.rbegin(), Items.rend(), 0);
```

Transforms to:  
转换为：

```c++
std::ranges::find(Items | std::views::reverse, 0);
```

:::
