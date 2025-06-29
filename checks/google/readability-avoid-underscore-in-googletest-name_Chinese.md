# google-readability-avoid-underscore-in-googletest-name

Checks whether there are underscores in googletest test suite names and  
test names in test macros:

- TEST
- TEST_F
- TEST_P
- TYPED_TEST
- TYPED_TEST_P

The FRIEND_TEST macro is not included.

检查 googletest 测试宏中的测试套件名称和测试名称是否包含下划线：

- TEST
- TEST_F
- TEST_P
- TYPED_TEST
- TYPED_TEST_P

不包括 FRIEND_TEST 宏。

For example:

例如：

```c++
TEST(TestSuiteName, Illegal_TestName) {}
TEST(Illegal_TestSuiteName, TestName) {}
```

would trigger the check. Underscores are not  
allowed in test suite name nor test names.  
[Underscores are not allowed](https://google.github.io/googletest/faq.html#why-should-test-suite-names-and-test-names-not-contain-underscore)

上述示例会触发该检查。测试套件名称和测试名称中不允许使用下划线。  
[不允许使用下划线](https://google.github.io/googletest/faq.html#why-should-test-suite-names-and-test-names-not-contain-underscore)

The DISABLED\_ prefix, which may be used to disable test suites and  
individual tests, is removed from the test suite name and test name before checking for  
underscores.  
[disable test suites and individual tests](https://google.github.io/googletest/advanced.html#temporarily-disabling-tests)

在检查下划线之前，会先从测试套件名称和测试名称中移除 DISABLED\_ 前缀，  
该前缀可用于禁用测试套件和单个测试。  
[禁用测试套件和单个测试](https://google.github.io/googletest/advanced.html#temporarily-disabling-tests)

This check does not propose any fixes.

此检查不会提供任何修复建议。
