# google-upgrade-googletest-case

Finds uses of deprecated Google Test version 1.9 APIs with names containing case and replaces them with equivalent APIs with suite.

查找使用已弃用的 Google Test 1.9 版本中名称包含 case 的 API，并将其替换为名称中包含 suite 的等效 API。

All names containing case are being replaced to be consistent with the meanings of "test case" and "test suite" as used by the International Software Testing Qualifications Board and ISO 29119.

所有包含 case 的名称都被替换，以符合国际软件测试资格委员会（ISTQB）和 ISO 29119 中“测试用例（test case）”和“测试套件（test suite）”的定义。

The new names are a part of Google Test version 1.9 (release pending). It is recommended that users update their dependency to version 1.9 and then use this check to remove deprecated names.

这些新名称是 Google Test 1.9 版本的一部分（尚未正式发布）。建议用户将依赖项更新到 1.9 版本，然后使用此检查工具移除已弃用的名称。

The affected APIs are:

受影响的 API 包括：

- Member functions of testing::Test, testing::TestInfo, testing::TestEventListener, testing::UnitTest, and any type inheriting from these types  
  testing::Test、testing::TestInfo、testing::TestEventListener、testing::UnitTest 及其派生类型中的成员函数
- The macros TYPED_TEST_CASE, TYPED_TEST_CASE_P, REGISTER_TYPED_TEST_CASE_P, and INSTANTIATE_TYPED_TEST_CASE_P  
  宏定义 TYPED_TEST_CASE、TYPED_TEST_CASE_P、REGISTER_TYPED_TEST_CASE_P 和 INSTANTIATE_TYPED_TEST_CASE_P
- The type alias testing::TestCase  
  类型别名 testing::TestCase

Examples of fixes created by this check:

此检查工具生成的修复示例如下：

```c++
class FooTest : public testing::Test {
public:
  static void SetUpTestCase();
  static void TearDownTestCase();
};

TYPED_TEST_CASE(BarTest, BarTypes);
```

becomes

变为

```c++
class FooTest : public testing::Test {
public:
  static void SetUpTestSuite();
  static void TearDownTestSuite();
};

TYPED_TEST_SUITE(BarTest, BarTypes);
```

For better consistency of user code, the check renames both virtual and non-virtual member functions with matching names in derived types. The check tries to provide only a warning when a fix cannot be made safely, as is the case with some template and macro uses.

为了提高用户代码的一致性，此检查工具会重命名派生类型中名称匹配的虚函数和非虚函数。当无法安全地进行修复时（例如某些模板或宏的使用场景），该工具将仅发出警告。
