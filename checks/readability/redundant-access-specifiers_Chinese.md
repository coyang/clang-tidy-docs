# readability-redundant-access-specifiers

Finds classes, structs, and unions containing redundant member (field and method) access specifiers.

查找包含冗余成员（字段和方法）访问说明符的类、结构体和联合体。

## Example

示例

```c++
class Foo {
public:
  int x;
  int y;
public:
  int z;
protected:
  int a;
public:
  int c;
}
```

In the example above, the second public declaration can be removed without any changes of behavior.

在上面的示例中，第二个 public 声明是冗余的，可以删除而不会改变程序行为。

## Options

选项

::: option
CheckFirstDeclaration

If set to true, the check will also diagnose if the first access specifier declaration is redundant (e.g. private inside class, or public inside struct or union). Default is false.

如果设置为 true，该检查器还会诊断第一个访问说明符是否冗余（例如 class 中的 private，或 struct/union 中的 public）。默认值为 false。
:::

### Example

示例

```c++
struct Bar {
public:
  int x;
}
```

If CheckFirstDeclaration option is enabled, a warning about redundant access specifier will be emitted, because public is the default member access for structs.

如果启用了 CheckFirstDeclaration 选项，将会发出关于冗余访问说明符的警告，因为 struct 的默认成员访问权限是 public。
