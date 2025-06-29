# bugprone-redundant-branch-condition

Finds condition variables in nested if statements that were also checked in the outer if statement and were not changed.

在嵌套的 if 语句中查找在外层 if 语句中已经检查过且未被修改的条件变量。

Simple example:

简单示例：

```c
bool onFire = isBurning();
if (onFire) {
  if (onFire)
    scream();
}
```

Here onFire is checked both in the outer if and the inner if statement without a possible change between the two checks. The check warns for this code and suggests removal of the second checking of variable onFire.

在这个例子中，变量 onFire 在外层和内层的 if 语句中都被检查了，并且在两次检查之间没有发生任何可能的更改。该检查器会对这段代码发出警告，并建议移除对变量 onFire 的第二次检查。

The checker also detects redundant condition checks if the condition variable is an operand of a logical "and" (&&) or a logical "or" (||) operator:

如果条件变量是逻辑“与”（&&）或逻辑“或”（||）操作符的操作数，检查器也会检测到冗余的条件检查：

```c
bool onFire = isBurning();
if (onFire) {
  if (onFire && peopleInTheBuilding > 0)
    scream();
}
```

```c
bool onFire = isBurning();
if (onFire) {
  if (onFire || isCollapsing())
    scream();
}
```

In the first case (logical "and") the suggested fix is to remove the redundant condition variable and keep the other side of the &&. In the second case (logical "or") the whole if is removed similarly to the simple case on the top.

在第一个例子中（逻辑“与”），建议的修复方式是移除冗余的条件变量 onFire，并保留 && 的另一边。在第二个例子中（逻辑“或”），整个 if 语句会像上面的简单示例一样被移除。

The condition of the outer if statement may also be a logical "and" (&&) expression:

外层 if 语句的条件也可能是一个逻辑“与”（&&）表达式：

```c
bool onFire = isBurning();
if (onFire && fireFighters < 10) {
  if (someOtherCondition()) {
    if (onFire)
      scream();
  }
}
```

The error is also detected if both the outer statement is a logical "and" (&&) and the inner statement is a logical "and" (&&) or "or" (||). The inner if statement does not have to be a direct descendant of the outer one.

如果外层语句是逻辑“与”（&&），而内层语句是逻辑“与”（&&）或“或”（||），也会检测到该错误。内层的 if 语句不必是外层语句的直接子语句。

No error is detected if the condition variable may have been changed between the two checks:

如果在两次检查之间条件变量可能发生了更改，则不会检测出错误：

```c
bool onFire = isBurning();
if (onFire) {
  tryToExtinguish(onFire);
  if (onFire && peopleInTheBuilding > 0)
    scream();
}
```

Every possible change is considered, thus if the condition variable is not a local variable of the function, it is a volatile or it has an alias (pointer or reference) then no warning is issued.

所有可能的更改都会被考虑，因此如果条件变量不是函数的局部变量、是 volatile 类型，或者有别名（指针或引用），则不会发出警告。

## Known limitations

已知限制

The else branch is not checked currently for negated condition variable:

目前不会检查 else 分支中对条件变量的取反检查：

```c
bool onFire = isBurning();
if (onFire) {
  scream();
} else {
  if (!onFire) {
    continueWork();
  }
}
```

The checker currently only detects redundant checking of single condition variables. More complex expressions are not checked:

目前检查器仅检测单个条件变量的冗余检查。更复杂的表达式不会被检查：

```c
if (peopleInTheBuilding == 1) {
  if (peopleInTheBuilding == 1) {
    doSomething();
  }
}
```
