# android-comparison-in-temp-failure-retry

Diagnoses comparisons that appear to be incorrectly placed in the argument to the TEMP_FAILURE_RETRY macro. Having such a use is incorrect in the vast majority of cases, and will often silently defeat the purpose of the TEMP_FAILURE_RETRY macro.

诊断那些似乎被错误地放置在 TEMP_FAILURE_RETRY 宏参数中的比较操作。在绝大多数情况下，这种用法是不正确的，并且通常会悄悄地破坏 TEMP_FAILURE_RETRY 宏的目的。

For context, TEMP_FAILURE_RETRY is a convenience macro provided by both glibc and Bionic. Its purpose is to repeatedly run a syscall until it either succeeds, or fails for reasons other than being interrupted.

作为背景，TEMP_FAILURE_RETRY 是由 glibc 和 Bionic 提供的一个便利宏。它的目的是反复执行一个系统调用，直到它成功，或者因非中断原因失败为止。

Example buggy usage looks like:

示例中的错误用法如下：

```c
char cs[1];
while (TEMP_FAILURE_RETRY(read(STDIN_FILENO, cs, sizeof(cs)) != 0)) {
  // Do something with cs.
}
```

Because TEMP_FAILURE_RETRY will check for whether the result of the comparison is -1, and retry if so.

因为 TEMP_FAILURE_RETRY 会检查比较操作的结果是否为 -1，如果是，则重试。

If you encounter this, the fix is simple: lift the comparison out of the TEMP_FAILURE_RETRY argument, like so:

如果你遇到这种情况，修复方法很简单：将比较操作从 TEMP_FAILURE_RETRY 的参数中提取出来，如下所示：

```c
char cs[1];
while (TEMP_FAILURE_RETRY(read(STDIN_FILENO, cs, sizeof(cs))) != 0) {
  // Do something with cs.
}
```

## Options

## 选项

::: option
RetryMacros

A comma-separated list of the names of retry macros to be checked. Default is TEMP_FAILURE_RETRY.

一个以逗号分隔的重试宏名称列表，用于指定要检查的宏。默认值为 TEMP_FAILURE_RETRY。
:::
