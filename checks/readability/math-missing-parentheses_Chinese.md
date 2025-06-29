# readability-math-missing-parentheses

Check for missing parentheses in mathematical expressions that involve  
operators of different priorities.

检查涉及不同优先级运算符的数学表达式中是否缺少括号。

Parentheses in mathematical expressions clarify the order of operations,  
especially with different-priority operators. Lengthy or multiline  
expressions can obscure this order, leading to coding errors. IDEs can  
aid clarity by highlighting parentheses. Explicitly using parentheses  
also clarifies what the developer had in mind when writing the  
expression. Ensuring their presence reduces ambiguity and errors,  
promoting clearer and more maintainable code.

数学表达式中的括号可以明确运算的顺序，特别是在涉及不同优先级的运算符时。  
较长或多行的表达式可能会使运算顺序变得模糊，从而导致编码错误。  
集成开发环境（IDE）可以通过高亮括号来帮助提高可读性。  
显式地使用括号还可以更清楚地表达开发者在编写表达式时的意图。  
确保括号的存在可以减少歧义和错误，从而提升代码的清晰度和可维护性。

Before:  
之前：

```c++
int x = 1 + 2 * 3 - 4 / 5;
```

After:  
之后：

```c++
int x = 1 + (2 * 3) - (4 / 5);
```
