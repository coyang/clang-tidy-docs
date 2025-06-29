# bugprone-assignment-in-if-condition

Finds assignments within conditions of if statements. Such assignments are bug-prone because they may have been intended as equality tests.

在 if 语句的条件中查找赋值操作。这类赋值容易引发错误，因为它们可能原本是想进行相等性测试。

This check finds all assignments within if conditions, including ones that are not flagged by -Wparentheses due to an extra set of parentheses, and including assignments that call an overloaded operator=(). The identified assignments violate BARR group "Rule 8.2.c".

此检查会查找所有出现在 if 条件中的赋值操作，包括那些由于额外的一对括号而未被 -Wparentheses 标记的情况，以及调用了重载 operator=() 的赋值操作。被识别出的赋值操作违反了 BARR 小组的“规则 8.2.c”。

```c++
int f = 3;
if(f = 4) { // This is identified by both `Wparentheses` and this check - should it have been: `if (f == 4)` ?
            // 此处的赋值操作会被 `Wparentheses` 和本检查同时识别 —— 原意是否应为：`if (f == 4)`？
  f = f + 1;
}

if((f == 5) || (f = 6)) { // the assignment here `(f = 6)` is identified by this check, but not by `-Wparentheses`. Should it have been `(f == 6)` ?
                         // 此处的赋值 `(f = 6)` 会被本检查识别，但不会被 `-Wparentheses` 标记。原意是否应为 `(f == 6)`？
  f = f + 2;
}
```
