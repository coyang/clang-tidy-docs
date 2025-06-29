# hicpp-ignored-remove-result

Ensure that the result of std::remove, std::remove_if and std::unique are not ignored according to rule 17.5.1.

确保不要忽略 std::remove、std::remove_if 和 std::unique 的返回结果，符合规则 17.5.1 的要求。

The mutating algorithms std::remove, std::remove_if and both overloads of std::unique operate by swapping or moving elements of the range they are operating over. On completion, they return an iterator to the last valid element. In the majority of cases the correct behavior is to use this result as the first operand in a call to std::erase.

可变算法 std::remove、std::remove_if 以及两个重载版本的 std::unique 通过交换或移动操作范围内的元素来工作。完成后，它们会返回指向最后一个有效元素的迭代器。在大多数情况下，正确的做法是将该返回值作为 std::erase 调用的第一个参数使用。

This check is a subset of bugprone-unused-return-value and depending on used options it can be superfluous to enable both checks.

此检查是 bugprone-unused-return-value 的一个子集，根据所使用的选项，启用这两个检查可能是多余的。

## Options

## 选项

::: option
AllowCastToVoid

Controls whether casting return values to void is permitted. Default: true.

控制是否允许将返回值强制转换为 void。默认值：true。
:::
