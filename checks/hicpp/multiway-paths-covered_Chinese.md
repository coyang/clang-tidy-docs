# hicpp-multiway-paths-covered

This check discovers situations where code paths are not fully-covered.  
它会检查代码路径未被完全覆盖的情况。

It furthermore suggests using if instead of switch if the code will be more clear.  
此外，如果使用 if 比 switch 更清晰，它会建议使用 if。

The rule 6.1.2 and rule 6.1.4 of the High Integrity C++ Coding Standard are enforced.  
该检查遵循《高完整性 C++ 编码标准》中的规则 6.1.2 和规则 6.1.4。

if-else if chains that miss a final else branch might lead to unexpected program execution and be the result of a logical error.  
缺少最终 else 分支的 if-else if 结构可能导致程序执行异常，并可能是逻辑错误的结果。

If the missing else branch is intended you can leave it empty with a clarifying comment.  
如果确实有意省略 else 分支，可以保留一个空的 else 并添加注释说明。

This warning can be noisy on some code bases, so it is disabled by default.  
该警告在某些代码库中可能会产生较多噪声，因此默认是禁用的。

```c++
void f1() {
  int i = determineTheNumber();

   if(i > 0) {
     // Some Calculation
   } else if (i < 0) {
     // Precondition violated or something else.
   }
   // ...
}
```

类似的逻辑也适用于未覆盖所有可能路径的 switch 语句。

```c++
// The missing default branch might be a logical error. It can be kept empty
// if there is nothing to do, making it explicit.
void f2(int i) {
  switch (i) {
  case 0: // something
    break;
  case 1: // something else
    break;
  }
  // All other numbers?
}

// Violates this rule as well, but already emits a compiler warning (-Wswitch).
enum Color { Red, Green, Blue, Yellow };
void f3(enum Color c) {
  switch (c) {
  case Red: // We can't drive for now.
    break;
  case Green:  // We are allowed to drive.
    break;
  }
  // Other cases missing
```

// 缺失 default 分支可能是逻辑错误。如果确实无事可做，可以保留一个空的 default 分支以明确意图。
// 以下代码也违反了该规则，但编译器已经会发出警告（-Wswitch）。

```c++
enum Color { Red, Green, Blue, Yellow };
void f3(enum Color c) {
  switch (c) {
  case Red: // We can't drive for now.
    break;
  case Green:  // We are allowed to drive.
    break;
  }
  // Other cases missing
}
```

The rule 6.1.4 requires every switch statement to have at least two case labels other than a default label.  
规则 6.1.4 要求每个 switch 语句至少包含两个非 default 的 case 标签。

Otherwise, the switch could be better expressed with an if statement.  
否则，该 switch 更适合用 if 语句来表达。

Degenerated switch statements without any labels are caught as well.  
退化的 switch 语句（即没有任何标签）也会被检测出来。

```c++
// Degenerated switch that could be better written as `if`
int i = 42;
switch(i) {
  case 1: // do something here
  default: // do something else here
}

// Should rather be the following:
if (i == 1) {
  // do something here
}
else {
  // do something here
}
```

```c++
// A completely degenerated switch will be diagnosed.
int i = 42;
switch(i) {}
```

// 完全退化的 switch 语句将被诊断出来。

## Options

## 选项

::: option  
WarnOnMissingElse

Boolean flag that activates a warning for missing else branches.  
用于启用缺失 else 分支警告的布尔标志。

Default is false.  
默认值为 false。

:::
