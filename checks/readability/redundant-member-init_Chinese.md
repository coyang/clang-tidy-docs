# readability-redundant-member-init

Finds member initializations that are unnecessary because the same  
default constructor would be called if they were not present.

查找那些不必要的成员初始化，因为即使没有这些初始化，也会调用相同的默认构造函数。

## Example

## 示例

```c++
// Explicitly initializing the member s and v is unnecessary.
class Foo {
public:
  Foo() : s() {}

private:
  std::string s;
  std::vector<int> v {};
};
```

```c++
// 显式初始化成员 s 和 v 是不必要的。
class Foo {
public:
  Foo() : s() {}

private:
  std::string s;
  std::vector<int> v {};
};
```

## Options

## 选项

::: option  
IgnoreBaseInCopyConstructors

Default is [false]{.title-ref}.

When [true]{.title-ref}, the check will ignore unnecessary base class  
initializations within copy constructors, since some compilers issue  
warnings/errors when base classes are not explicitly initialized in copy  
constructors. For example, gcc with -Wextra or -Werror=extra  
issues warning or error  
base class 'Bar' should be explicitly initialized in the copy constructor  
if Bar() were removed in the following example:  
:::

::: 选项  
IgnoreBaseInCopyConstructors

默认值为 [false]{.title-ref}。

当设置为 [true]{.title-ref} 时，该检查将忽略拷贝构造函数中不必要的基类初始化，  
因为某些编译器在拷贝构造函数中未显式初始化基类时会发出警告或错误。  
例如，使用 gcc 并启用 -Wextra 或 -Werror=extra 时，  
如果在以下示例中移除了 Bar()，则会发出如下警告或错误：  
base class 'Bar' should be explicitly initialized in the copy constructor  
:::

```c++
// Explicitly initializing member s and base class Bar is unnecessary.
struct Foo : public Bar {
  // Remove s() below. If IgnoreBaseInCopyConstructors!=0, keep Bar().
  Foo(const Foo& foo) : Bar(), s() {}
  std::string s;
};
```

```c++
// 显式初始化成员 s 和基类 Bar 是不必要的。
struct Foo : public Bar {
  // 移除下面的 s()。如果 IgnoreBaseInCopyConstructors != 0，则保留 Bar()。
  Foo(const Foo& foo) : Bar(), s() {}
  std::string s;
};
```
