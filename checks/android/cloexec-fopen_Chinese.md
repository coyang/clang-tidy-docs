# android-cloexec-fopen

fopen() should include e in their mode string; so re would be  
valid. This is equivalent to having set FD_CLOEXEC on that descriptor.

fopen() 应该在模式字符串中包含 e；因此 re 是有效的。  
这相当于在该文件描述符上设置了 FD_CLOEXEC。

Examples:  
示例：

```c++
fopen("fn", "r");

// becomes
// 变为

fopen("fn", "re");
```
