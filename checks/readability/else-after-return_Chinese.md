# readability-else-after-return

[LLVM Coding Standards](https://llvm.org/docs/CodingStandards.html) advises to reduce indentation where possible and where it makes understanding code easier. Early exit is one of the suggested enforcements of that. Please do not use else or else if after something that interrupts control flow - like return, break, continue, throw.

[LLVM 编码规范](https://llvm.org/docs/CodingStandards.html) 建议在可能的情况下减少缩进层级，以提高代码的可读性。提前退出（early exit）是实现这一目标的推荐方式之一。请不要在会中断控制流的语句（如 return、break、continue、throw）之后使用 else 或 else if。

The following piece of code illustrates how the check works. This piece of code:

下面的代码片段展示了该检查器的工作方式。原始代码如下：

```c++
void foo(int Value) {
  int Local = 0;
  for (int i = 0; i < 42; i++) {
    if (Value == 1) {
      return;
    } else {
      Local++;
    }

    if (Value == 2)
      continue;
    else
      Local++;

    if (Value == 3) {
      throw 42;
    } else {
      Local++;
    }
  }
}
```

Would be transformed into:

将被转换为：

```c++
void foo(int Value) {
  int Local = 0;
  for (int i = 0; i < 42; i++) {
    if (Value == 1) {
      return;
    }
    Local++;

    if (Value == 2)
      continue;
    Local++;

    if (Value == 3) {
      throw 42;
    }
    Local++;
  }
}
```

## Options

## 选项

::: option
WarnOnUnfixable

When true, emit a warning for cases where the check can't output a Fix-It. These can occur with declarations inside the else branch that would have an extended lifetime if the else branch was removed. Default value is true.

当设置为 true 时，对于无法自动修复（Fix-It）的情况会发出警告。这种情况通常出现在 else 分支中包含变量声明，而移除 else 分支会导致变量生命周期延长。默认值为 true。
:::

::: option
WarnOnConditionVariables

When true, the check will attempt to refactor a variable defined inside the condition of the if statement that is used in the else branch defining them just before the if statement. This can only be done if the if statement is the last statement in its parent's scope. Default value is true.

当设置为 true 时，检查器会尝试将 if 条件中定义的变量（如果该变量在 else 分支中被使用）提取到 if 语句之前进行定义。此操作仅在 if 语句是其父作用域中的最后一条语句时才会执行。默认值为 true。
:::

## LLVM alias

## LLVM 别名

There is an alias of this check called llvm-else-after-return. In that version the options WarnOnUnfixable and WarnOnConditionVariables are both set to false by default.

该检查器有一个别名叫做 llvm-else-after-return。在该版本中，选项 WarnOnUnfixable 和 WarnOnConditionVariables 的默认值均为 false。

This check helps to enforce this LLVM Coding Standards recommendation.

此检查器有助于强制执行 LLVM 编码规范中的以下建议：

[Don't use else after a return](https://llvm.org/docs/CodingStandards.html#don-t-use-else-after-a-return)  
[不要在 return 语句之后使用 else]
