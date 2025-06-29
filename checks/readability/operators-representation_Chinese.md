# readability-operators-representation

Enforces consistent token representation for invoked binary, unary and overloaded operators in C++ code. The check supports both traditional and alternative representations of operators, such as && and and, || and or, and so on.

强制在 C++ 代码中对调用的二元、一元和重载运算符使用一致的符号表示方式。该检查支持运算符的传统表示法和替代表示法，例如 && 和 and，|| 和 or 等。

In the realm of C++ programming, developers have the option to choose between two distinct representations for operators: traditional token representation and alternative token representation. Traditional tokens utilize symbols, such as &&, ||, and !, while alternative tokens employ more descriptive words like and, or, and not.

在 C++ 编程领域，开发者可以在两种不同的运算符表示方式之间进行选择：传统符号表示法和替代符号表示法。传统表示法使用符号，例如 &&、|| 和 !，而替代表示法使用更具描述性的单词，如 and、or 和 not。

In the following mapping table, a comprehensive list of traditional and alternative tokens, along with their corresponding representations, is presented:

在下表中，列出了传统符号和替代符号的完整对应关系：

| Traditional | Alternative |
| ----------- | ----------- | --- |
| &&          | and         |
| &=          | and_eq      |
| &           | bitand      |
|             | bitor       |
| ~           | compl       |
| !           | not         |
| !=          | not_eq      |
|             |             | or  |
| =           | or_eq       |
| ^           | xor         |
| ^=          | xor_eq      |

: Token Representation Mapping Table  
: 符号表示法映射表

## Example

## 示例

```c++
// Traditional Token Representation:
// 传统符号表示法：

if (!a||!b)
{
    // do something
    // 执行某些操作
}

// Alternative Token Representation:
// 替代符号表示法：

if (not a or not b)
{
    // do something
    // 执行某些操作
}
```

## Options

## 配置选项

Due to the distinct benefits and drawbacks of each representation, the default configuration doesn't enforce either. Explicit configuration is needed.

由于每种表示法各有优缺点，默认配置不会强制使用任一方式。需要显式配置。

To configure check to enforce Traditional Token Representation for all operators set options to [&&;&=;&;|;~;!;!=;||;|=;^;^=]{.title-ref}.

若要配置检查以强制所有运算符使用传统符号表示法，请将选项设置为 [&&;&=;&;|;~;!;!=;||;|=;^;^=]{.title-ref}。

To configure check to enforce Alternative Token Representation for all operators set options to [and;and_eq;bitand;bitor;compl;not;not_eq;or;or_eq;xor;xor_eq]{.title-ref}.

若要配置检查以强制所有运算符使用替代符号表示法，请将选项设置为 [and;and_eq;bitand;bitor;compl;not;not_eq;or;or_eq;xor;xor_eq]{.title-ref}。

Developers do not need to enforce all operators, and can mix the representations as desired by specifying a semicolon-separated list of both traditional and alternative tokens in the configuration, such as [and;||;not]{.title-ref}.

开发者无需强制所有运算符使用统一表示法，可以根据需要混合使用传统和替代符号，只需在配置中指定以分号分隔的符号列表，例如 [and;||;not]{.title-ref}。

::: option  
BinaryOperators

This option allows you to specify a semicolon-separated list of binary operators for which you want to enforce specific token representation. The default value is empty string.

此选项允许你指定一个以分号分隔的二元运算符列表，用于强制使用特定的符号表示方式。默认值为空字符串。

:::

::: option  
OverloadedOperators

This option allows you to specify a semicolon-separated list of overloaded operators for which you want to enforce specific token representation. The default value is empty string.

此选项允许你指定一个以分号分隔的重载运算符列表，用于强制使用特定的符号表示方式。默认值为空字符串。

:::
