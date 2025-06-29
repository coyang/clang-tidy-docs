# readability-simplify-boolean-expr

Looks for boolean expressions involving boolean constants and simplifies them to use the appropriate boolean expression directly. Simplifies boolean expressions by application of DeMorgan's Theorem.

查找涉及布尔常量的布尔表达式，并将其简化为直接使用适当的布尔表达式。通过应用德摩根定律（DeMorgan's Theorem）来简化布尔表达式。

Examples:

示例：

+------------------------------------------+----------------+
| Initial expression | Result |
| 初始表达式 | 结果 |
+==========================================+================+
| if (b == true) | > if (b) |
| if (b == true) | > if (b) |
+------------------------------------------+----------------+
| if (b == false) | > if (!b) |
| if (b == false) | > if (!b) |
+------------------------------------------+----------------+
| if (b && true) | > if (b) |
| if (b && true) | > if (b) |
+------------------------------------------+----------------+
| if (b && false) | > if (false) |
| if (b && false) | > if (false) |
+------------------------------------------+----------------+
| if (b || true) | > if (true) |
| if (b || true) | > if (true) |
+------------------------------------------+----------------+
| if (b || false) | > if (b) |
| if (b || false) | > if (b) |
+------------------------------------------+----------------+
| e ? true : false | > e |
| e ? true : false | > e |
+------------------------------------------+----------------+
| e ? false : true | > !e |
| e ? false : true | > !e |
+------------------------------------------+----------------+
| if (true) t(); else f(); | > t(); |
| if (true) t(); else f(); | > t(); |
+------------------------------------------+----------------+
| if (false) t(); else f(); | > f(); |
| if (false) t(); else f(); | > f(); |
+------------------------------------------+----------------+
| if (e) return true; else return false; | > return e; |
| if (e) return true; else return false; | > return e; |
+------------------------------------------+----------------+
| if (e) return false; else return true; | > return !e; |
| if (e) return false; else return true; | > return !e; |
+------------------------------------------+----------------+
| if (e) b = true; else b = false; | > b = e; |
| if (e) b = true; else b = false; | > b = e; |
+------------------------------------------+----------------+
| if (e) b = false; else b = true; | > b = !e; |
| if (e) b = false; else b = true; | > b = !e; |
+------------------------------------------+----------------+
| if (e) return true; return false; | > return e; |
| if (e) return true; return false; | > return e; |
+------------------------------------------+----------------+
| if (e) return false; return true; | > return !e; |
| if (e) return false; return true; | > return !e; |
+------------------------------------------+----------------+
| !(!a || b) | > a && !b |
| !(!a || b) | > a && !b |
+------------------------------------------+----------------+
| !(a || !b) | > !a && b |
| !(a || !b) | > !a && b |
+------------------------------------------+----------------+
| !(!a || !b) | > a && b |
| !(!a || !b) | > a && b |
+------------------------------------------+----------------+
| !(!a && b) | > a || !b |
| !(!a && b) | > a || !b |
+------------------------------------------+----------------+
| !(a && !b) | > !a || b |
| !(a && !b) | > !a || b |
+------------------------------------------+----------------+
| !(!a && !b) | > a || b |
| !(!a && !b) | > a || b |
+------------------------------------------+----------------+

The resulting expression e is modified as follows:

生成的表达式 e 会按如下方式修改：

1. Unnecessary parentheses around the expression are removed.  
   删除表达式周围不必要的括号。

2. Negated applications of ! are eliminated.  
   消除多余的否定（!）操作。

3. Negated applications of comparison operators are changed to use the opposite condition.  
   否定的比较操作符将被替换为相反的条件。

4. Implicit conversions of pointers, including pointers to members, to bool are replaced with explicit comparisons to nullptr in C++11 or NULL in C++98/03.  
   指针（包括成员指针）到 bool 的隐式转换将被替换为与 nullptr（C++11）或 NULL（C++98/03）的显式比较。

5. Implicit casts to bool are replaced with explicit casts to bool.  
   隐式转换为 bool 的操作将被替换为显式转换。

6. Object expressions with explicit operator bool conversion operators are replaced with explicit casts to bool.  
   带有 explicit operator bool 转换函数的对象表达式将被替换为显式转换为 bool。

7. Implicit conversions of integral types to bool are replaced with explicit comparisons to 0.  
   整型到 bool 的隐式转换将被替换为与 0 的显式比较。

Examples:

示例：

1. The ternary assignment bool b = (i < 0) ? true : false; has redundant parentheses and becomes bool b = i < 0;  
   三元赋值 bool b = (i < 0) ? true : false; 括号冗余，简化为 bool b = i < 0;

2. The conditional return if (!b) return false; return true; has an implied double negation and becomes return b;  
   条件返回 if (!b) return false; return true; 存在隐式双重否定，简化为 return b;

3. The conditional return if (i < 0) return false; return true; becomes return i >= 0;  
   条件返回 if (i < 0) return false; return true; 简化为 return i >= 0;

   The conditional return if (i != 0) return false; return true; becomes return i == 0;  
   条件返回 if (i != 0) return false; return true; 简化为 return i == 0;

4. The conditional return if (p) return true; return false; has an implicit conversion of a pointer to bool and becomes return p != nullptr;  
   条件返回 if (p) return true; return false; 存在指针到 bool 的隐式转换，简化为 return p != nullptr;

   The ternary assignment bool b = (i & 1) ? true : false; has an implicit conversion of i & 1 to bool and becomes bool b = (i & 1) != 0;  
   三元赋值 bool b = (i & 1) ? true : false; 中 i & 1 隐式转换为 bool，简化为 bool b = (i & 1) != 0;

5. The conditional return if (i & 1) return true; else return false; has an implicit conversion of an integer quantity i & 1 to bool and becomes return (i & 1) != 0;  
   条件返回 if (i & 1) return true; else return false; 中整数 i & 1 被隐式转换为 bool，简化为 return (i & 1) != 0;

6. Given struct X { explicit operator bool(); };, and an instance x of struct X, the conditional return if (x) return true; return false; becomes return static_cast<bool>(x);  
   给定 struct X { explicit operator bool(); }; 和其实例 x，条件返回 if (x) return true; return false; 简化为 return static_cast<bool>(x);

## Options

## 选项

::: option
IgnoreMacros

If true, ignore boolean expressions originating from expanded macros. Default is false.

如果为 true，则忽略来自宏展开的布尔表达式。默认值为 false。
:::

::: option
ChainedConditionalReturn

If true, conditional boolean return statements at the end of an if/else if chain will be transformed. Default is false.

如果为 true，则转换 if/else if 链末尾的条件布尔返回语句。默认值为 false。
:::

::: option
ChainedConditionalAssignment

If true, conditional boolean assignments at the end of an if/else if chain will be transformed. Default is false.

如果为 true，则转换 if/else if 链末尾的条件布尔赋值语句。默认值为 false。
:::

::: option
SimplifyDeMorgan

If true, DeMorgan's Theorem will be applied to simplify negated conjunctions and disjunctions. Default is true.

如果为 true，将应用德摩根定律来简化否定的与/或表达式。默认值为 true。
:::

::: option
SimplifyDeMorganRelaxed

If true, SimplifyDeMorgan will also transform negated conjunctions and disjunctions where there is no negation on either operand. This option has no effect if SimplifyDeMorgan is false. Default is false.

如果为 true，SimplifyDeMorgan 还将转换操作数两侧都没有否定的与/或表达式。若 SimplifyDeMorgan 为 false，此选项无效。默认值为 false。

When Enabled:

启用时：

```
bool X = !(A && B)
bool Y = !(A || B)
```

Would be transformed to:

将被转换为：

```
bool X = !A || !B
bool Y = !A && !B
```

:::
