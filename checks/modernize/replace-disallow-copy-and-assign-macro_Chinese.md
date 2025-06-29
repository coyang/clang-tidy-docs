# modernize-replace-disallow-copy-and-assign-macro

Finds macro expansions of DISALLOW_COPY_AND_ASSIGN(Type) and replaces them with a deleted copy constructor and a deleted assignment operator.

查找 DISALLOW_COPY_AND_ASSIGN(Type) 宏的展开，并将其替换为被删除的拷贝构造函数和赋值运算符。

Before the delete keyword was introduced in C++11 it was common practice to declare a copy constructor and an assignment operator as private members. This effectively makes them unusable to the public API of a class.

在 C++11 引入 delete 关键字之前，通常的做法是将拷贝构造函数和赋值运算符声明为私有成员。这样可以有效地防止类的公共 API 使用这些函数。

With the advent of the delete keyword in C++11 we can abandon the private access of the copy constructor and the assignment operator and delete the methods entirely.

随着 C++11 中 delete 关键字的出现，我们可以不再将拷贝构造函数和赋值运算符设为私有，而是直接将这些方法删除。

When running this check on a code like this:

当对如下代码运行此检查时：

```c++
class Foo {
private:
  DISALLOW_COPY_AND_ASSIGN(Foo);
};
```

It will be transformed to this:

它将被转换为如下形式：

```c++
class Foo {
private:
  Foo(const Foo &) = delete;
  const Foo &operator=(const Foo &) = delete;
};
```

## Known Limitations

已知限制

- Notice that the migration example above leaves the private access specification untouched. You might want to run the check modernize-use-equals-delete to get warnings for deleted functions in private sections.

- 请注意，上面的迁移示例保留了 private 访问说明符。您可能希望运行 modernize-use-equals-delete 检查，以便对私有部分中被删除的函数发出警告。

## Options

选项

::: option
MacroName

A string specifying the macro name whose expansion will be replaced. Default is DISALLOW_COPY_AND_ASSIGN.

一个字符串，用于指定要替换其展开内容的宏名称。默认值为 DISALLOW_COPY_AND_ASSIGN。

:::

See: https://en.cppreference.com/w/cpp/language/function#Deleted_functions

参见：https://en.cppreference.com/w/cpp/language/function#Deleted_functions
