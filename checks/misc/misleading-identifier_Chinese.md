# misc-misleading-identifier

Finds identifiers that contain Unicode characters with right-to-left  
direction, which can be confusing as they may change the understanding  
of a whole statement line, as described in [Trojan  
Source](https://trojansource.codes).

查找包含从右到左书写方向的 Unicode 字符的标识符，这些字符可能会引起混淆，  
因为它们可能会改变整行语句的理解方式，正如 [特洛伊源代码攻击](https://trojansource.codes) 中所描述的那样。

An example of such misleading code follows:

以下是一个具有误导性的代码示例：

```text
#include <stdio.h>

short int א = (short int)0;
short int ג = (short int)12345;

int main() {
  int א = ג; // a local variable, set to zero?
  printf("ג is %d\n", ג);
  printf("א is %d\n", א);
}
```
