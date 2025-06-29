# readability-duplicate-include

Looks for duplicate includes and removes them. The check maintains a  
list of included files and looks for duplicates. If a macro is defined  
or undefined then the list of included files is cleared.

查找重复的 include 并将其移除。该检查会维护一个已包含文件的列表，并查找其中的重复项。如果定义或取消定义了宏，则已包含文件的列表会被清空。

Examples:  
示例：

```c++
#include <memory>
#include <vector>
#include <memory>
```

becomes  
变为

```c++
#include <memory>
#include <vector>
```

Because of the intervening macro definitions, this code remains  
unchanged:

由于中间有宏定义的干预，以下代码保持不变：

```c++
#undef NDEBUG
#include "assertion.h"
// ...code with assertions enabled

#define NDEBUG
#include "assertion.h"
// ...code with assertions disabled
```
