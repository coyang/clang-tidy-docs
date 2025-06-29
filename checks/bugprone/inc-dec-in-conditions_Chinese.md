# bugprone-inc-dec-in-conditions

Detects when a variable is both incremented/decremented and referenced inside a complex condition and suggests moving them outside to avoid ambiguity in the variable's value.

检测变量在复杂条件中既被递增/递减又被引用的情况，并建议将其移出条件表达式，以避免变量值的歧义。

When a variable is modified and also used in a complex condition, it can lead to unexpected behavior. The side-effect of changing the variable's value within the condition can make the code difficult to reason about. Additionally, the developer's intended timing for the modification of the variable may not be clear, leading to misunderstandings and errors. This can be particularly problematic when the condition involves logical operators like && and ||, where the order of evaluation can further complicate the situation.

当一个变量在复杂条件中被修改并被使用时，可能会导致意外的行为。在条件表达式中改变变量值的副作用会使代码难以理解。此外，开发者对变量修改的预期时机可能不明确，从而导致误解和错误。当条件中涉及逻辑运算符如 && 和 || 时，求值顺序可能会进一步使情况复杂化。

Consider the following example:

请看以下示例：

```c++
int i = 0;
// ...
if (i++ < 5 && i > 0) {
  // do something
}
```

In this example, the result of the expression may not be what the developer intended. The original intention of the developer could be to increment i after the entire condition is evaluated, but in reality, i will be incremented before i > 0 is executed. This can lead to unexpected behavior and bugs in the code. To fix this issue, the developer should separate the increment operation from the condition and perform it separately. For example, they can increment i in a separate statement before or after the condition is evaluated. This ensures that the value of i is predictable and consistent throughout the code.

在这个例子中，表达式的结果可能不是开发者所期望的。开发者原本可能希望在整个条件判断完成后再对 i 进行递增，但实际上，i 会在执行 i > 0 之前就被递增。这可能导致意外的行为和代码中的错误。为了解决这个问题，开发者应将递增操作从条件表达式中分离出来，单独执行。例如，他们可以在条件判断之前或之后单独对 i 进行递增操作。这样可以确保 i 的值在整个代码中是可预测且一致的。

```c++
int i = 0;
// ...
i++;
if (i <= 5 && i > 0) {
  // do something
}
```

Another common issue occurs when multiple increments or decrements are performed on the same variable inside a complex condition. For example:

另一个常见问题是，在复杂条件中对同一个变量执行多次递增或递减操作。例如：

```c++
int i = 4;
// ...
if (i++ < 5 || --i > 2) {
  // do something
}
```

There is a potential issue with this code due to the order of evaluation in C++. The || operator used in the condition statement guarantees that if the first operand evaluates to true, the second operand will not be evaluated. This means that if i were initially 4, the first operand i < 5 would evaluate to true and the second operand i > 2 would not be evaluated. As a result, the decrement operation --i would not be executed and i would hold value 5, which may not be the intended behavior for the developer.

由于 C++ 中的求值顺序，这段代码存在潜在问题。条件语句中使用的 || 运算符保证如果第一个操作数的结果为 true，则不会对第二个操作数进行求值。这意味着如果 i 初始值为 4，第一个操作数 i < 5 的结果为 true，第二个操作数 i > 2 将不会被求值。因此，递减操作 --i 将不会被执行，i 的值将变为 5，这可能不是开发者所期望的行为。

To avoid this potential issue, the both increment and decrement operation on i should be moved outside the condition statement.

为避免这种潜在问题，应该将对 i 的递增和递减操作移出条件语句之外。
