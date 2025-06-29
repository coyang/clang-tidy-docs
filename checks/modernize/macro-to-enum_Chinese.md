以下是完整翻译后的 markdown 文件内容，保留了代码块格式，并在英文下方添加了对应的简体中文翻译，中英文之间以一个空行分隔：

# modernize-macro-to-enum

Replaces groups of adjacent macros with an unscoped anonymous enum.  
使用未限定的匿名枚举替换一组相邻的宏定义。

Using an unscoped anonymous enum ensures that everywhere the macro token was used previously, the enumerator name may be safely used.  
使用未限定的匿名枚举可以确保在之前使用宏标记的所有地方，都可以安全地使用枚举器名称。

This check can be used to enforce the C++ core guideline [Enum.1: Prefer enumerations over macros](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#enum1-prefer-enumerations-over-macros), within the constraints outlined below.  
此检查可用于在以下约束条件下，强制执行 C++ 核心指南中的 [Enum.1：优先使用枚举而非宏](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#enum1-prefer-enumerations-over-macros)。

Potential macros for replacement must meet the following constraints:  
可替换的宏必须满足以下约束条件：

- Macros must expand only to integral literal tokens or expressions of literal tokens. The expression may contain any of the unary operators -, +, ~ or !, any of the binary operators ,, -, +, _, /, %, &, |, ^, <, >, <=, >=, ==, !=, ||, &&, <<, >> or <=>, the ternary operator ?: and its GNU extension. Parenthesized expressions are also recognized. This recognizes most valid expressions. In particular, expressions with the sizeof operator are not recognized.  
  宏必须仅展开为整型字面量标记或由字面量标记组成的表达式。表达式可以包含以下任意一元运算符：-、+、~ 或 !，任意二元运算符：,、-、+、_、/、%、&、|、^、<、>、<=、>=、==、!=、||、&&、<<、>> 或 <=>，三元运算符 ?: 及其 GNU 扩展。也支持带括号的表达式。这涵盖了大多数有效表达式。特别地，包含 sizeof 运算符的表达式不被识别。

- Macros must be defined on sequential source file lines, or with only comment lines in between macro definitions.  
  宏必须在源文件中连续定义，或仅在宏定义之间包含注释行。

- Macros must all be defined in the same source file.  
  所有宏必须定义在同一个源文件中。

- Macros must not be defined within a conditional compilation block. (Conditional include guards are exempt from this constraint.)  
  宏不能定义在条件编译块中。（条件包含保护不受此限制。）

- Macros must not be defined adjacent to other preprocessor directives.  
  宏不能与其他预处理指令相邻定义。

- Macros must not be used in any conditional preprocessing directive.  
  宏不能在任何条件预处理指令中使用。

- Macros must not be used as arguments to other macros.  
  宏不能作为其他宏的参数使用。

- Macros must not be undefined.  
  宏不能被取消定义（undef）。

- Macros must be defined at the top-level, not inside any declaration or definition.  
  宏必须在顶层定义，不能定义在任何声明或定义内部。

Each cluster of macros meeting the above constraints is presumed to be a set of values suitable for replacement by an anonymous enum. From there, a developer can give the anonymous enum a name and continue refactoring to a scoped enum if desired. Comments on the same line as a macro definition or between subsequent macro definitions are preserved in the output. No formatting is assumed in the provided replacements, although clang-tidy can optionally format all fixes.  
每组满足上述约束条件的宏被视为一组适合用匿名枚举替换的值。开发者可以在此基础上为匿名枚举命名，并根据需要继续重构为限定作用域的枚举。与宏定义在同一行的注释或后续宏定义之间的注释将在输出中保留。生成的替换内容不强制格式化，但 clang-tidy 可选择对所有修复进行格式化。

::: warning

Initializing expressions are assumed to be valid initializers for an enum. C requires that enum values fit into an int, but this may not be the case for some accepted constant expressions. For instance 1 << 40 will not fit into an int when the size of an int is 32 bits.  
初始化表达式被假定为枚举的有效初始化器。C 语言要求枚举值必须适合一个 int 类型，但某些被接受的常量表达式可能不符合此要求。例如，当 int 的大小为 32 位时，1 << 40 将无法适配 int 类型。

:::

Examples:  
示例：

```c++
#define RED   0xFF0000
#define GREEN 0x00FF00
#define BLUE  0x0000FF

#define TM_NONE (-1) // No method selected.
#define TM_ONE 1    // Use tailored method one.
#define TM_TWO 2    // Use tailored method two.  Method two
                    // is preferable to method one.
#define TM_THREE 3  // Use tailored method three.
```

变为：

```c++
enum {
RED = 0xFF0000,
GREEN = 0x00FF00,
BLUE = 0x0000FF
};

enum {
TM_NONE = (-1), // No method selected.
TM_ONE = 1,    // Use tailored method one.
TM_TWO = 2,    // Use tailored method two.  Method two
                    // is preferable to method one.
TM_THREE = 3  // Use tailored method three.
};
```
