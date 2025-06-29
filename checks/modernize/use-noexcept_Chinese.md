# modernize-use-noexcept

This check replaces deprecated dynamic exception specifications with the appropriate noexcept specification (introduced in C++11). By default this check will replace throw() with noexcept, and throw(<exception>[,...]) or throw(...) with noexcept(false).

此检查会将已弃用的动态异常规范替换为适当的 noexcept 规范（在 C++11 中引入）。默认情况下，该检查会将 throw() 替换为 noexcept，将 throw(<exception>[,...]) 或 throw(...) 替换为 noexcept(false)。

## Example

示例

```c++
void foo() throw();
void bar() throw(int) {}
```

transforms to:

转换为：

```c++
void foo() noexcept;
void bar() noexcept(false) {}
```

## Options

选项

::: option
ReplacementString

Users can use ReplacementString to specify a macro to use instead of noexcept. This is useful when maintaining source code that uses custom exception specification marking other than noexcept. Fix-it hints will only be generated for non-throwing specifications.

用户可以使用 ReplacementString 来指定一个宏，用于替代 noexcept。当维护使用自定义异常规范标记（而不是 noexcept）的源代码时，这非常有用。仅对非抛出规范生成修复建议。

:::

### Example

示例

```c++
void bar() throw(int);
void foo() throw();
```

transforms to:

转换为：

```c++
void bar() throw(int);  // No fix-it generated.
void foo() NOEXCEPT;
```

if the ReplacementString option is set to NOEXCEPT.

如果 ReplacementString 选项设置为 NOEXCEPT。

::: option
UseNoexceptFalse
:::

Enabled by default, disabling will generate fix-it hints that remove throwing dynamic exception specs, e.g., throw(<something>), completely without providing a replacement text, except for destructors and delete operators that are noexcept(true) by default.

该选项默认启用，禁用后将生成修复建议，完全移除抛出型动态异常规范（如 throw(<something>)），而不提供替代文本。但析构函数和 delete 运算符除外，它们默认是 noexcept(true)。

### Example

示例

```c++
void foo() throw(int) {}

struct bar {
  void foobar() throw(int);
  void operator delete(void *ptr) throw(int);
  void operator delete[](void *ptr) throw(int);
  ~bar() throw(int);
}
```

transforms to:

转换为：

```c++
void foo() {}

struct bar {
  void foobar();
  void operator delete(void *ptr) noexcept(false);
  void operator delete[](void *ptr) noexcept(false);
  ~bar() noexcept(false);
}
```

if the UseNoexceptFalse option is set to false.

如果 UseNoexceptFalse 选项设置为 false。
