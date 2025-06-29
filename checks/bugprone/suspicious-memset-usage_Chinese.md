# bugprone-suspicious-memset-usage

该检查器会发现 memset() 调用中参数可能存在错误的情况。  
考虑函数原型为 void* memset(void* destination, int fill_value, size_t byte_count)，  
该检查涵盖以下几种情况：

This check finds memset() calls with potential mistakes in their arguments.  
Considering the function as void* memset(void* destination, int fill_value, size_t byte_count),  
the following cases are covered:

**Case 1: Fill value is a character '0'**

情况 1：填充值为字符 '0'

Filling up a memory area with ASCII code 48 characters is not customary,  
possibly integer zeroes were intended instead. The check offers a  
replacement of '0' with 0. Memsetting character pointers with '0'  
is allowed.

用 ASCII 码为 48 的字符填充内存区域并不常见，  
可能原意是填充整数 0。该检查器会建议将 '0' 替换为 0。  
对于字符指针使用 '0' 进行 memset 是允许的。

**Case 2: Fill value is truncated**

情况 2：填充值被截断

Memset converts fill_value to unsigned char before using it. If  
fill_value is out of unsigned character range, it gets truncated and  
memory will not contain the desired pattern.

memset 会在使用前将 fill_value 转换为 unsigned char。  
如果 fill_value 超出了 unsigned char 的取值范围，  
它会被截断，导致内存中不会包含预期的填充值。

**Case 3: Byte count is zero**

情况 3：字节数为零

Calling memset with a literal zero in its byte_count argument is  
likely to be unintended and swapped with fill_value. The check offers  
to swap these two arguments.

当 memset 的 byte_count 参数为字面值 0 时，  
很可能是无意中将其与 fill_value 参数写反了。  
该检查器会建议交换这两个参数。

Corresponding cpplint.py check name: runtime/memset.

对应的 cpplint.py 检查项名称为：runtime/memset。

Examples:

示例：

```c++
void foo() {
  int i[5] = {1, 2, 3, 4, 5};
  int *ip = i;
  char c = '1';
  char *cp = &c;
  int v = 0;

  // Case 1
  memset(ip, '0', 1); // suspicious
  memset(cp, '0', 1); // OK

  // 情况 1
  memset(ip, '0', 1); // 可疑
  memset(cp, '0', 1); // 正确

  // Case 2
  memset(ip, 0xabcd, 1); // fill value gets truncated
  memset(ip, 0x00, 1);   // OK

  // 情况 2
  memset(ip, 0xabcd, 1); // 填充值被截断
  memset(ip, 0x00, 1);   // 正确

  // Case 3
  memset(ip, sizeof(int), v); // zero length, potentially swapped
  memset(ip, 0, 1);           // OK

  // 情况 3
  memset(ip, sizeof(int), v); // 长度为零，可能参数写反
  memset(ip, 0, 1);           // 正确
}
```
