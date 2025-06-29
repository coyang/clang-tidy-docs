# zircon-temporary-objects

Warns on construction of specific temporary objects in the Zircon kernel. If the object should be flagged, the fully qualified type name must be explicitly passed to the check.

在 Zircon 内核中，当构造特定的临时对象时发出警告。如果某个对象需要被标记，则必须显式传递其完全限定类型名称以进行检查。

For example, given the list of classes "Foo" and "NS::Bar", all of the following will trigger the warning:

例如，给定类列表 "Foo" 和 "NS::Bar"，以下所有情况都会触发警告：

```c++
Foo();
Foo F = Foo();
func(Foo());

namespace NS {

Bar();

}
```

With the same list, the following will not trigger the warning:

使用相同的类列表，以下情况将不会触发警告：

```c++
Foo F;                 // Non-temporary construction okay
Foo F(param);          // Non-temporary construction okay
Foo *F = new Foo();    // New construction okay

Bar();                 // Not NS::Bar, so okay
NS::Bar B;             // Non-temporary construction okay
```

Note that objects must be explicitly specified in order to be flagged, and so objects that inherit a specified object will not be flagged.

请注意，只有显式指定的对象才会被标记，因此继承了被指定对象的类不会被标记。

This check matches temporary objects without regard for inheritance and so a prohibited base class type does not similarly prohibit derived class types.

此检查不考虑继承关系，仅匹配临时对象，因此禁止的基类类型不会自动禁止其派生类类型。

```c++
class Derived : Foo {} // Derived is not explicitly disallowed
Derived();             // and so temporary construction is okay
```

class Derived : Foo {} // Derived 并未被显式禁止  
Derived(); // 因此构造临时对象是允许的

## Options

## 选项

::: option
Names

A semi-colon-separated list of fully-qualified names of C++ classes that should not be constructed as temporaries. Default is empty.

一个由分号分隔的 C++ 类的完全限定名称列表，这些类不应被构造为临时对象。默认值为空。
:::
