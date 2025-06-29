# bugprone-suspicious-include

The check detects various cases when an include refers to what appears  
to be an implementation file, which often leads to hard-to-track-down  
ODR violations.

该检查会检测多种情况，当 include 引用的似乎是一个实现文件时，  
这通常会导致难以追踪的 ODR（One Definition Rule）违规问题。

Examples:  
示例：

```c++
#include "Dinosaur.hpp"     // OK, .hpp files tend not to have definitions.
                            // 正确，.hpp 文件通常不包含定义。

#include "Pterodactyl.h"    // OK, .h files tend not to have definitions.
                            // 正确，.h 文件通常不包含定义。

#include "Velociraptor.cpp" // Warning, filename is suspicious.
                            // 警告，文件名可疑。

#include_next <stdio.c>     // Warning, filename is suspicious.
                            // 警告，文件名可疑。
```
