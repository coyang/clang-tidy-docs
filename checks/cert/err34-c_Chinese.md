# cert-err34-c

This check flags calls to string-to-number conversion functions that do not verify the validity of the conversion, such as atoi() or scanf(). It does not flag calls to strtol(), or other, related conversion functions that do perform better error checking.

此检查会标记那些调用字符串转数字的函数但未验证转换有效性的情况，例如 atoi() 或 scanf()。它不会标记调用 strtol() 或其他具有更好错误检查机制的相关转换函数的情况。

```c
#include <stdlib.h>

void func(const char *buff) {
  int si;

  if (buff) {
    si = atoi(buff); /* 'atoi' used to convert a string to an integer, but function will
                         not report conversion errors; consider using 'strtol' instead. */
  } else {
    /* Handle error */
  }
}
```

此检查对应于 CERT C 编码标准规则 [ERR34-C. Detect errors when converting a string to a number](https://www.securecoding.cert.org/confluence/display/c/ERR34-C.+Detect+errors+when+converting+a+string+to+a+number)。

此检查对应于 CERT C 编码标准规则 [ERR34-C. 在将字符串转换为数字时检测错误](https://www.securecoding.cert.org/confluence/display/c/ERR34-C.+Detect+errors+when+converting+a+string+to+a+number)。
