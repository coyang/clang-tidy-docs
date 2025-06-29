# bugprone-multiple-statement-macro

Detect multiple statement macros that are used in unbraced conditionals.  
检测在未加大括号的条件语句中使用的多语句宏。

Only the first statement of the macro will be inside the conditional and the other ones will be executed unconditionally.  
宏中的第一条语句会被包含在条件语句中，而其他语句将无条件执行。

Example:  
示例：

```c++
#define INCREMENT_TWO(x, y) (x)++; (y)++
if (do_increment)
  INCREMENT_TWO(a, b);  // (b)++ will be executed unconditionally.
// (b)++ 将无条件执行。
```
