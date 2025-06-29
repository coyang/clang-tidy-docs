# misc-redundant-expression

Detect redundant expressions which are typically errors due to copy-paste.

检测冗余表达式，这些通常是由于复制粘贴造成的错误。

Depending on the operator expressions may be

- redundant,
- always true,
- always false,
- always a constant (zero or one).

根据操作符的不同，表达式可能是：

- 冗余的，
- 永远为 true，
- 永远为 false，
- 永远是一个常量（零或一）。

Examples:

示例：

```c++
((x+1) | (x+1))                   // (x+1) is redundant
(p->x == p->x)                    // always true
(p->x < p->x)                     // always false
(speed - speed + 1 == 12)         // speed - speed is always zero
int b = a | 4 | a                 // identical expr on both sides
((x=1) | (x=1))                   // expression is identical
(DEFINE_1 | DEFINE_1)             // same macro on the both sides
((DEF_1 + DEF_2) | (DEF_1+DEF_2)) // expressions differ in spaces only
```

```c++
((x+1) | (x+1))                   // (x+1) 是冗余的
(p->x == p->x)                    // 永远为 true
(p->x < p->x)                     // 永远为 false
(speed - speed + 1 == 12)         // speed - speed 永远为零
int b = a | 4 | a                 // 两边的表达式完全相同
((x=1) | (x=1))                   // 表达式完全相同
(DEFINE_1 | DEFINE_1)             // 两边是相同的宏
((DEF_1 + DEF_2) | (DEF_1+DEF_2)) // 表达式仅在空格上不同
```

Floats are handled except in the case that NaNs are checked like so:

浮点数也会被处理，除了在如下方式检查 NaN 的情况下：

```c++
int TestFloat(float F) {
  if (F == F)               // Identical float values used
    return 1;
  return 0;
}

int TestFloat(float F) {
  // Testing NaN.
  if (F != F && F == F)     // does not warn
    return 1;
  return 0;
}
```

```c++
int TestFloat(float F) {
  if (F == F)               // 使用了相同的浮点值
    return 1;
  return 0;
}

int TestFloat(float F) {
  // 检查 NaN。
  if (F != F && F == F)     // 不会发出警告
    return 1;
  return 0;
}
```
