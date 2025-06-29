# readability-redundant-declaration

Finds redundant variable and function declarations.

查找冗余的变量和函数声明。

```c++
extern int X;
extern int X;
```

becomes

变为

```c++
extern int X;
```

Such redundant declarations can be removed without changing program behavior. They can for instance be unintentional left overs from previous refactorings when code has been moved around. Having redundant declarations could in worst case mean that there are typos in the code that cause bugs.

此类冗余声明可以在不改变程序行为的情况下被移除。例如，它们可能是之前重构代码时无意留下的残余。当代码被移动时，可能会遗留这些声明。最糟糕的情况下，冗余声明可能意味着代码中存在拼写错误，从而导致程序错误。

Normally the code can be automatically fixed, clang-tidy can remove the second declaration. However there are 2 cases when you need to fix the code manually:

通常情况下，代码可以被自动修复，clang-tidy 可以移除第二个声明。但在以下两种情况下，你需要手动修复代码：

- When the declarations are in different header files;
- 当声明位于不同的头文件中时；

- When multiple variables are declared together.
- 当多个变量被一起声明时。

## Options

## 选项

::: option
IgnoreMacros

If set to true, the check will not give warnings inside macros. Default is true.

如果设置为 true，该检查将不会在宏内部给出警告。默认值为 true。
:::
