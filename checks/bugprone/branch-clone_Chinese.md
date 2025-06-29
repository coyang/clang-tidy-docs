# bugprone-branch-clone

Checks for repeated branches in if/else if/else chains, consecutive repeated branches in switch statements and identical true and false branches in conditional operators.

检查 if/else if/else 语句链中重复的分支、switch 语句中连续重复的分支，以及条件运算符中 true 和 false 分支相同的情况。

```c++
if (test_value(x)) {
  y++;
  do_something(x, y);
} else {
  y++;
  do_something(x, y);
}
```

In this simple example (which could arise e.g. as a copy-paste error) the then and else branches are identical and the code is equivalent the following shorter and cleaner code:

在这个简单的示例中（可能是由于复制粘贴错误导致），then 和 else 分支是相同的，代码等价于以下更简洁的写法：

```c++
test_value(x); // can be omitted unless it has side effects
y++;
do_something(x, y);
```

If this is the intended behavior, then there is no reason to use a conditional statement; otherwise the issue can be solved by fixing the branch that is handled incorrectly.

如果这是预期的行为，那么就没有必要使用条件语句；否则，可以通过修复处理错误的分支来解决问题。

The check detects repeated branches in longer if/else if/else chains where it would be even harder to notice the problem.

该检查还会检测较长的 if/else if/else 语句链中的重复分支，在这种情况下问题更难被发现。

The check also detects repeated inner and outer if statements that may be a result of a copy-paste error. This check cannot currently detect identical inner and outer if statements if code is between the if conditions. An example is as follows.

该检查还会检测可能由于复制粘贴错误导致的内层和外层 if 语句重复的情况。如果两个 if 条件之间有代码，目前该检查无法检测出内外层 if 语句是否相同。示例如下：

```c++
void test_warn_inner_if_1(int x) {
  if (x == 1) {    // warns, if with identical inner if
    if (x == 1)    // inner if is here
      ;
  if (x == 1) {    // does not warn, cannot detect
    int y = x;
    if (x == 1)
      ;
  }
}
```

In switch statements the check only reports repeated branches when they are consecutive, because it is relatively common that the case: labels have some natural ordering and rearranging them would decrease the readability of the code. For example:

在 switch 语句中，该检查仅在分支是连续时才报告重复分支，因为 case: 标签通常具有某种自然顺序，重新排列它们可能会降低代码的可读性。例如：

```c++
switch (ch) {
case 'a':
  return 10;
case 'A':
  return 10;
case 'b':
  return 11;
case 'B':
  return 11;
default:
  return 10;
}
```

Here the check reports that the 'a' and 'A' branches are identical (and that the 'b' and 'B' branches are also identical), but does not report that the default: branch is also identical to the first two branches. If this is indeed the correct behavior, then it could be implemented as:

在这里，该检查会报告 'a' 和 'A' 分支是相同的（'b' 和 'B' 分支也是相同的），但不会报告 default: 分支与前两个分支相同。如果这确实是正确的行为，那么可以改写为：

```c++
switch (ch) {
case 'a':
case 'A':
  return 10;
case 'b':
case 'B':
  return 11;
default:
  return 10;
}
```

Here the check does not warn for the repeated return 10;, which is good if we want to preserve that 'a' is before 'b' and default: is the last branch.

在这种写法中，检查不会对重复的 return 10; 发出警告，这很好，因为我们希望保留 'a' 在 'b' 之前，default: 是最后一个分支。

Switch cases marked with the [[fallthrough]] attribute are ignored.

带有 [[fallthrough]] 属性的 switch 分支将被忽略。

Finally, the check also examines conditional operators and reports code like:

最后，该检查还会分析条件运算符，并报告如下代码：

```c++
return test_value(x) ? x : x;
```

Unlike if statements, the check does not detect chains of conditional operators.

与 if 语句不同，该检查不会检测条件运算符的链式结构。

Note: This check also reports situations where branches become identical only after preprocessing.

注意：该检查还会报告那些仅在预处理后才变得相同的分支情况。
