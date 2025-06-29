# clang-analyzer-nullability.NullableReturnedFromNonnull

Warns when a nullable pointer is returned from a function that has  
\_Nonnull return type.

当一个函数声明返回类型为 \_Nonnull，但实际返回了一个可为空（nullable）的指针时，发出警告。

The clang-analyzer-nullability.NullableReturnedFromNonnull check is an alias, please see Clang Static Analyzer Available Checkers for more information.

clang-analyzer-nullability.NullableReturnedFromNonnull 检查器是一个别名，请参阅 Clang 静态分析器可用检查器 以获取更多信息。

[Clang Static Analyzer Available Checkers](https://clang.llvm.org/docs/analyzer/checkers.html#nullability-nullablereturnedfromnonnull)

[Clang 静态分析器可用检查器](https://clang.llvm.org/docs/analyzer/checkers.html#nullability-nullablereturnedfromnonnull)
