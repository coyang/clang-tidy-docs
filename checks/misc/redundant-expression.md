# misc-redundant-expression

Detect redundant expressions which are typically errors due to
copy-paste.

Depending on the operator expressions may be

- redundant,
- always `true`,
- always `false`,
- always a constant (zero or one).

Examples:

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

Floats are handled except in the case that NaNs are checked like so:

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
