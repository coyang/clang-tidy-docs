# bugprone-casting-through-void

Detects unsafe or redundant two-step casting operations involving
void\*, which is equivalent to reinterpret_cast as per the C++
Standard.

检测涉及 void\* 的不安全或冗余的两步类型转换操作，根据 C++ 标准，这等同于 reinterpret_cast。

Two-step type conversions via void\* are discouraged for several
reasons.

出于多种原因，不建议通过 void\* 进行两步类型转换。

- They obscure code and impede its understandability, complicating
  maintenance.

  它们会使代码变得晦涩难懂，影响可读性，增加维护难度。

- These conversions bypass valuable compiler support, erasing warnings
  related to pointer alignment. It may violate strict aliasing rule
  and leading to undefined behavior.

  这种转换绕过了编译器提供的重要支持，屏蔽了与指针对齐相关的警告，可能违反严格别名规则，从而导致未定义行为。

- In scenarios involving multiple inheritance, ambiguity and
  unexpected outcomes can arise due to the loss of type information,
  posing runtime issues.

  在涉及多重继承的场景中，由于类型信息的丢失，可能会引发歧义和意外结果，带来运行时问题。

In summary, avoiding two-step type conversions through void\* ensures
clearer code, maintains essential compiler warnings, and prevents
ambiguity and potential runtime errors, particularly in complex
inheritance scenarios. If such a cast is wanted, it shall be done via
reinterpret_cast, to express the intent more clearly.

总之，避免通过 void\* 进行两步类型转换可以使代码更清晰，保留关键的编译器警告，并防止歧义和潜在的运行时错误，尤其是在复杂继承的场景中。如果确实需要进行这样的转换，应使用 reinterpret_cast，以更清晰地表达意图。

Note: it is expected that, after applying the suggested fix and using
reinterpret_cast, the check
cppcoreguidelines-pro-type-reinterpret-cast
will emit a warning. This is intentional: reinterpret_cast
is a dangerous operation that can easily break the strict aliasing rules
when dereferencing the casted pointer, invoking Undefined Behavior. The
warning is there to prompt users to carefully analyze whether the usage
of reinterpret_cast is safe, in which case the warning may be
suppressed.

注意：在应用建议的修复并使用 reinterpret_cast 之后，检查项 cppcoreguidelines-pro-type-reinterpret-cast 预计会发出警告。这是有意为之：reinterpret_cast 是一种危险的操作，在对转换后的指针进行解引用时，很容易违反严格别名规则，从而引发未定义行为。该警告旨在提示用户仔细分析 reinterpret_cast 的使用是否安全，如果确认安全，可以抑制该警告。

Examples:

示例：

```c++
using IntegerPointer = int *;
double *ptr;

static_cast<IntegerPointer>(static_cast<void *>(ptr)); // WRONG
// 错误：通过 void* 进行两步转换

reinterpret_cast<IntegerPointer>(reinterpret_cast<void *>(ptr)); // WRONG
// 错误：通过 void* 进行两步 reinterpret_cast 转换

(IntegerPointer)(void *)ptr; // WRONG
// 错误：C 风格的两步转换

IntegerPointer(static_cast<void *>(ptr)); // WRONG
// 错误：构造函数风格的两步转换

reinterpret_cast<IntegerPointer>(ptr); // OK, clearly expresses intent.
// 正确：清晰表达了转换意图
                                       // NOTE: dereferencing this pointer violates
                                       // the strict aliasing rules, invoking
                                       // Undefined Behavior.
// 注意：对该指针进行解引用会违反严格别名规则，导致未定义行为
```
