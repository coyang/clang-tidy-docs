# portability-avoid-pragma-once

Finds uses of #pragma once and suggests replacing them with standard  
include guards (#ifndef/#define/#endif) for improved portability.

查找使用 #pragma once 的情况，并建议用标准的包含保护（#ifndef/#define/#endif）替代，以提高可移植性。

#pragma once is a non-standard extension, despite being widely  
supported by modern compilers. Relying on it can lead to portability  
issues in some environments.

尽管现代编译器广泛支持，#pragma once 仍然是一个非标准扩展。在某些环境中依赖它可能会导致可移植性问题。

Some older or specialized C/C++ compilers, particularly in embedded  
systems, may not fully support #pragma once.

一些较旧或专用的 C/C++ 编译器，特别是在嵌入式系统中，可能并不完全支持 #pragma once。

It can also fail in certain file system configurations, like network  
drives or complex symbolic links, potentially leading to compilation  
issues.

在某些文件系统配置中，例如网络驱动器或复杂的符号链接，#pragma once 也可能失效，从而导致编译问题。

Consider the following header file:

请参考以下头文件：

```c++
// my_header.h
#pragma once // warning: avoid 'pragma once' directive; use include guards instead
```

```c++
// my_header.h
#pragma once // 警告：避免使用 'pragma once' 指令；请改用包含保护
```

The warning suggests using include guards:

该警告建议使用包含保护：

```c++
// my_header.h
#ifndef PATH_TO_MY_HEADER_H // Good: use include guards.
#define PATH_TO_MY_HEADER_H

#endif // PATH_TO_MY_HEADER_H
```

```c++
// my_header.h
#ifndef PATH_TO_MY_HEADER_H // 推荐：使用包含保护
#define PATH_TO_MY_HEADER_H

#endif // PATH_TO_MY_HEADER_H
```
