# bugprone-swapped-arguments

Finds potentially swapped arguments by examining implicit conversions.  
通过检查隐式转换来发现可能被交换的参数。

It analyzes the types of the arguments being passed to a function and compares them to the expected types of the corresponding parameters. If there is a mismatch or an implicit conversion that indicates a potential swap, a warning is raised.  
它会分析传递给函数的参数类型，并将其与对应参数的期望类型进行比较。如果存在类型不匹配或隐式转换表明可能发生了参数交换，则会发出警告。

```c++
void printNumbers(int a, float b);

int main() {
  // Swapped arguments: float passed as int, int as float)
  printNumbers(10.0f, 5);
  return 0;
}
```

Covers a wide range of implicit conversions, including:  
涵盖了多种隐式转换类型，包括：

- User-defined conversions  
  用户自定义转换

- Conversions from floating-point types to boolean or integral types  
  从浮点类型转换为布尔类型或整数类型

- Conversions from integral types to boolean or floating-point types  
  从整数类型转换为布尔类型或浮点类型

- Conversions from boolean to integer types or floating-point types  
  从布尔类型转换为整数类型或浮点类型

- Conversions from (member) pointers to boolean  
  从（成员）指针转换为布尔类型

It is important to note that for most argument swaps, the types need to match exactly. However, there are exceptions to this rule. Specifically, when the swapped argument is of integral type, an exact match is not always necessary. Implicit casts from other integral types are also accepted. Similarly, when dealing with floating-point arguments, implicit casts between different floating-point types are considered acceptable.  
需要注意的是，对于大多数参数交换，类型必须完全匹配。然而，这一规则也有例外。特别是当被交换的参数是整数类型时，不一定要求完全匹配。来自其他整数类型的隐式转换也是可以接受的。同样地，在处理浮点类型参数时，不同浮点类型之间的隐式转换也被视为可接受的。

To avoid confusion, swaps where both swapped arguments are of integral types or both are of floating-point types do not trigger the warning. In such cases, it's assumed that the developer intentionally used different integral or floating-point types and does not raise a warning. This approach prevents false positives and provides flexibility in handling situations where varying integral or floating-point types are intentionally utilized.  
为避免混淆，当被交换的两个参数都是整数类型或都是浮点类型时，将不会触发警告。在这种情况下，假设开发者是有意使用不同的整数或浮点类型，因此不会发出警告。这种做法可以防止误报，并在有意使用不同整数或浮点类型的情况下提供更大的灵活性。
