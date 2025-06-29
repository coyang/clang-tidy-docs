# bugprone-spuriously-wake-up-functions

Finds cnd_wait, cnd_timedwait, wait, wait_for, or wait_until function calls when the function is not invoked from a loop that checks whether a condition predicate holds or the function has a condition parameter.

查找 cnd_wait、cnd_timedwait、wait、wait_for 或 wait_until 函数的调用，这些调用未在循环中执行以检查条件谓词是否成立，或者函数没有条件参数。

```c++
if (condition_predicate) {
    condition.wait(lk);
}
```

```c++
if (condition_predicate) {
    condition.wait(lk);
}
```

```c
if (condition_predicate) {
    if (thrd_success != cnd_wait(&condition, &lock)) {
    }
}
```

```c
if (condition_predicate) {
    if (thrd_success != cnd_wait(&condition, &lock)) {
    }
}
```

This check corresponds to the CERT C++ Coding Standard rule CON54-CPP. Wrap functions that can spuriously wake up in a loop and CERT C Coding Standard rule CON36-C. Wrap functions that can spuriously wake up in a loop.

此检查对应于 CERT C++ 编码标准规则 CON54-CPP：将可能被虚假唤醒的函数包装在循环中，以及 CERT C 编码标准规则 CON36-C：将可能被虚假唤醒的函数包装在循环中。

- [CON54-CPP. Wrap functions that can spuriously wake up in a loop](https://wiki.sei.cmu.edu/confluence/display/cplusplus/CON54-CPP.+Wrap+functions+that+can+spuriously+wake+up+in+a+loop)

- [CON36-C. Wrap functions that can spuriously wake up in a loop](https://wiki.sei.cmu.edu/confluence/display/c/CON36-C.+Wrap+functions+that+can+spuriously+wake+up+in+a+loop)
