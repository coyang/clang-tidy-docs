# misc-misleading-bidirectional

Warn about unterminated bidirectional unicode sequence, detecting  
potential attack as described in the [Trojan  
Source](https://www.trojansource.codes) attack.

警告未终止的双向 Unicode 序列，检测如 [特洛伊源代码攻击](https://www.trojansource.codes) 中所描述的潜在攻击。

Example:

示例：

```c++
#include <iostream>

int main() {
    bool isAdmin = false;
    /*‮ } ⁦if (isAdmin)⁩ ⁦ begin admins only */
        std::cout << "You are an admin.\n";
    /* end admins only ‮ { ⁦*/
    return 0;
}
```
