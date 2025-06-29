# misc-header-include-cycle

Check detects cyclic #include dependencies between user-defined headers.

检查检测用户自定义头文件之间的循环 #include 依赖关系。

```c++
// Header A.hpp
#pragma once
#include "B.hpp"

// Header B.hpp
#pragma once
#include "C.hpp"

// Header C.hpp
#pragma once
#include "A.hpp"

// Include chain: A->B->C->A
// 包含链：A->B->C->A
```

Header files are a crucial part of many C++ programs as they provide a way to organize declarations and definitions shared across multiple source files. However, header files can also create problems when they become entangled in complex dependency cycles. Such cycles can cause issues with compilation times, unnecessary rebuilds, and make it harder to understand the overall structure of the code.

头文件是许多 C++ 程序的重要组成部分，因为它们提供了一种在多个源文件之间组织共享声明和定义的方式。然而，当头文件陷入复杂的依赖循环时，也可能带来问题。这类循环会导致编译时间增加、不必要的重新构建，并使理解代码整体结构变得更加困难。

To address these issues, a check has been developed to detect cyclic dependencies between header files, also known as "include cycles". An include cycle occurs when a header file A includes header file B, and B (or any subsequent included header file) includes back header file A, resulting in a circular dependency cycle.

为了解决这些问题，开发了一个检查器，用于检测头文件之间的循环依赖关系，也称为“包含循环”。当头文件 A 包含头文件 B，而 B（或其后包含的任何头文件）又反过来包含头文件 A 时，就会形成一个循环依赖关系。

This check operates at the preprocessor level and specifically analyzes user-defined headers and their dependencies. It focuses solely on detecting include cycles while disregarding other types or function dependencies. This specialized analysis helps identify and prevent issues related to header file organization.

该检查器在预处理器级别运行，专门分析用户自定义头文件及其依赖关系。它仅关注检测包含循环，而忽略其他类型或函数依赖关系。这种专门的分析有助于识别和预防与头文件组织相关的问题。

By detecting include cycles early in the development process, developers can identify and resolve these issues before they become more difficult and time-consuming to fix. This can lead to faster compile times, improved code quality, and a more maintainable codebase overall. Additionally, by ensuring that header files are organized in a way that avoids cyclic dependencies, developers can make their code easier to understand and modify over time.

通过在开发早期检测包含循环，开发人员可以在问题变得更难解决和更耗时之前识别并解决这些问题。这可以带来更快的编译时间、更高的代码质量以及更易维护的代码库。此外，通过确保头文件以避免循环依赖的方式进行组织，开发人员可以使代码更易于理解和修改。

It's worth noting that only user-defined headers and their dependencies are analyzed. System includes such as standard library headers and third-party library headers are excluded. System includes are usually well-designed and free of include cycles, and ignoring them helps to focus on potential issues within the project's own codebase. This limitation doesn't diminish the ability to detect #include cycles within the analyzed code.

值得注意的是，检查器仅分析用户自定义头文件及其依赖关系。系统包含项（如标准库头文件和第三方库头文件）被排除在外。系统包含项通常设计良好，不存在包含循环，忽略它们有助于专注于项目自身代码库中的潜在问题。这个限制不会影响对分析代码中 #include 循环的检测能力。

Developers should carefully review any warnings or feedback provided by this solution. While the analysis aims to identify and prevent include cycles, there may be situations where exceptions or modifications are necessary. It's important to exercise judgment and consider the specific context of the codebase when making adjustments.

开发人员应仔细审查该工具提供的任何警告或反馈。虽然该分析旨在识别和防止包含循环，但在某些情况下可能需要例外或修改。在进行调整时，重要的是要运用判断力并考虑代码库的具体上下文。

## Options

## 选项

::: option
IgnoredFilesList

Provides a way to exclude specific files/headers from the warnings raised by a check. This can be achieved by specifying a semicolon-separated list of regular expressions or filenames. This option can be used as an alternative to //NOLINT when using it is not possible. The default value of this option is an empty string, indicating that no files are ignored by default.

提供一种方式将特定文件/头文件从检查器发出的警告中排除。这可以通过指定一个以分号分隔的正则表达式或文件名列表来实现。当无法使用 //NOLINT 时，可以使用此选项作为替代。该选项的默认值为空字符串，表示默认情况下不忽略任何文件。
:::
