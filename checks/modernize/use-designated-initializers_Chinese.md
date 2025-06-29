# modernize-use-designated-initializers

Finds initializer lists for aggregate types which could be written as
designated initializers instead.

查找可以使用指定初始化器（designated initializers）来替代的聚合类型的初始化列表。

With plain initializer lists, it is very easy to introduce bugs when
adding new fields in the middle of a struct or class type. The same
confusion might arise when changing the order of fields.

使用普通的初始化列表时，在结构体或类中间添加新字段时很容易引入错误。更改字段顺序时也可能会引起类似的混淆。

C++20 supports the designated initializer syntax for aggregate types. By
applying it, we can always be sure that aggregates are constructed
correctly, because every variable being initialized is referenced by its
name.

C++20 支持用于聚合类型的指定初始化器语法。通过使用该语法，我们可以确保聚合类型被正确构造，因为每个被初始化的变量都是通过其名称引用的。

Example:

示例：

```
struct S { int i, j; };
```

is an aggregate type that should be initialized as

这是一个聚合类型，应该被初始化为：

```
S s{.i = 1, .j = 2};
```

instead of

而不是：

```
S s{1, 2};
```

which could easily become an issue when i and j are swapped in the
declaration of S.

当在 S 的声明中 i 和 j 的顺序被交换时，这种写法很容易引发问题。

Even when compiling in a language version older than C++20, depending on
your compiler, designated initializers are potentially supported.
Therefore, the check is by default restricted to C99/C++20 and above.
Check out the options -Wc99-designator to get support for mixed
designators in initializer list in C and -Wc++20-designator for
support of designated initializers in older C++ language modes.

即使在早于 C++20 的语言版本中进行编译，指定初始化器也可能会被编译器支持。因此，该检查默认限制在 C99/C++20 及以上版本中进行。请查看选项 -Wc99-designator 以在 C 语言中获得对混合指定初始化器的支持，以及 -Wc++20-designator 以在旧版 C++ 模式中获得对指定初始化器的支持。

## Options

## 选项

::: option
IgnoreMacros

The value false specifies that components of initializer
lists expanded from macros are not checked. The default value is
true.

值为 false 表示不会检查由宏展开的初始化列表组件。默认值为 true。

:::

::: option
IgnoreSingleElementAggregates

The value false specifies that even initializers for
aggregate types with only a single element should be checked. The
default value is true. std::array initializations are
always excluded, as the type is a standard library abstraction and not
intended to be initialized with designated initializers.

值为 false 表示即使是仅包含单个元素的聚合类型的初始化器也应被检查。默认值为 true。std::array 的初始化始终被排除在外，因为该类型是标准库的抽象，不应使用指定初始化器进行初始化。

:::

::: option
RestrictToPODTypes

The value true specifies that only Plain Old Data (POD)
types shall be checked. This makes the check applicable to even older
C++ standards. The default value is false.

值为 true 表示仅检查普通旧数据（POD）类型。这使得该检查适用于更早的 C++ 标准。默认值为 false。

:::

::: option
StrictCStandardCompliance

When set to false, the check will not restrict itself to
C99 and above. The default value is true.

当设置为 false 时，该检查将不再限制于 C99 及以上版本。默认值为 true。

:::

::: option
StrictCppStandardCompliance

When set to false, the check will not restrict itself to
C++20 and above. The default value is true.

当设置为 false 时，该检查将不再限制于 C++20 及以上版本。默认值为 true。

:::
