# misc-non-copyable-objects

# misc-non-copyable-objects

[cert-fio38-c]{.title-ref} redirects here as an alias for this check.

[cert-fio38-c]{.title-ref} 是此检查项的别名，重定向至本页面。

The check flags dereferences and non-pointer declarations of objects  
that are not meant to be passed by value, such as C FILE objects or  
POSIX pthread_mutex_t objects.

该检查项会标记对不应按值传递的对象的解引用操作和非指针声明，例如 C 的 FILE 对象或 POSIX 的 pthread_mutex_t 对象。

This check corresponds to CERT C++ Coding Standard rule FIO38-C. Do not  
copy a FILE object.

此检查项对应于 CERT C++ 编码标准规则 FIO38-C：不要复制 FILE 对象。

链接：https://www.securecoding.cert.org/confluence/display/c/FIO38-C.+Do+not+copy+a+FILE+object
