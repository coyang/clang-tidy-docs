# performance-unnecessary-copy-initialization

Finds local variable declarations that are initialized using the copy constructor of a non-trivially-copyable type but it would suffice to obtain a const reference.

查找使用非平凡可复制类型的拷贝构造函数进行初始化的局部变量声明，而实际上使用 const 引用即可满足需求。

The check is only applied if it is safe to replace the copy by a const reference. This is the case when the variable is const qualified or when it is only used as a const, i.e. only const methods or operators are invoked on it, or it is used as const reference or value argument in constructors or function calls.

仅当将拷贝替换为 const 引用是安全的情况下才会进行此检查。具体而言，当变量被声明为 const，或仅以 const 方式使用（即仅调用其 const 方法或运算符，或作为构造函数或函数调用中的 const 引用或值参数）时，才会触发此检查。

Example:

示例：

```c++
const string& constReference();
void Function() {
  // The warning will suggest making this a const reference.
  // 警告将建议将此变量改为 const 引用。
  const string UnnecessaryCopy = constReference();
}

struct Foo {
  const string& name() const;
};
void Function(const Foo& foo) {
  // The warning will suggest making this a const reference.
  // 警告将建议将此变量改为 const 引用。
  string UnnecessaryCopy1 = foo.name();
  UnnecessaryCopy1.find("bar");

  // The warning will suggest making this a const reference.
  // 警告将建议将此变量改为 const 引用。
  string UnnecessaryCopy2 = UnnecessaryCopy1;
  UnnecessaryCopy2.find("bar");
}
```

## Options

## 选项

::: option
AllowedTypes

A semicolon-separated list of names of types allowed to be initialized by copying. Regular expressions are accepted, e.g. [Rr]ef(erence)?$ matches every type with suffix Ref, ref, Reference and reference. The default is empty. If a name in the list contains the sequence ::, it is matched against the qualified type name (i.e. namespace::Type), otherwise it is matched against only the type name (i.e. Type).

允许通过拷贝进行初始化的类型名称列表，使用分号分隔。支持正则表达式，例如 [Rr]ef(erence)?$ 可匹配所有以 Ref、ref、Reference 和 reference 结尾的类型。默认值为空。如果列表中的名称包含 ::，则与限定类型名（如 namespace::Type）进行匹配；否则仅与类型名（如 Type）进行匹配。
:::

::: option
ExcludedContainerTypes

A semicolon-separated list of names of types whose methods are allowed to return the const reference the variable is copied from. When an expensive to copy variable is copy initialized by the return value from a type on this list the check does not trigger. This can be used to exclude types known to be const incorrect or where the lifetime or immutability of returned references is not tied to mutations of the container. An example are view types that don't own the underlying data. Like for AllowedTypes above, regular expressions are accepted and the inclusion of :: determines whether the qualified typename is matched or not.

允许其方法返回 const 引用（即变量从中拷贝初始化）的类型名称列表，使用分号分隔。当一个代价高昂的拷贝变量是由该列表中的类型的返回值进行拷贝初始化时，将不会触发此检查。此选项可用于排除已知 const 不正确的类型，或返回引用的生命周期或不可变性不依赖于容器变更的类型。例如，不拥有底层数据的视图类型。与上面的 AllowedTypes 类似，支持正则表达式，并且是否包含 :: 决定是否匹配限定类型名。
:::
