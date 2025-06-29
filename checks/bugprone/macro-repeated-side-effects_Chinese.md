# bugprone-macro-repeated-side-effects

Checks for repeated argument with side effects in macros.

检查宏中具有副作用的参数被重复使用的情况。

This check warns when a macro uses the same argument more than once and that argument has side effects. This can lead to unexpected behavior because the argument expression may be evaluated multiple times.

当一个宏多次使用同一个具有副作用的参数时，此检查会发出警告。这可能导致意外行为，因为该参数表达式可能会被多次求值。

For example:

例如：

```cpp
#define SQUARE(x) ((x) * (x))
int i = 4;
int result = SQUARE(i++); // i is incremented twice!
```

```cpp
#define SQUARE(x) ((x) * (x))
int i = 4;
int result = SQUARE(i++); // i 被递增了两次！
```

In this case, i++ is evaluated twice, which is likely not what the programmer intended.

在这种情况下，i++ 被求值了两次，这很可能不是程序员的本意。

Macros that evaluate their arguments multiple times should be avoided, or the arguments passed to them should not have side effects.

应避免使用多次求值其参数的宏，或者传递给它们的参数不应具有副作用。

This check helps identify such problematic macro usages.

此检查有助于识别此类有问题的宏用法。
