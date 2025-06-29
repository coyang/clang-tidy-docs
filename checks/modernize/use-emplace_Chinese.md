# modernize-use-emplace

The check flags insertions to an STL-style container done by calling the
push_back, push, or push_front methods with an
explicitly-constructed temporary of the container element type. In this
case, the corresponding emplace equivalent methods result in less
verbose and potentially more efficient code. Right now the check
doesn't support insert. It also doesn't support insert functions
for associative containers because replacing insert with emplace may
result in speed regression, but it might get support with some addition flag in the future.

该检查会标记使用 push_back、push 或 push_front 方法向 STL 风格容器插入显式构造的临时元素的情况。在这种情况下，使用对应的 emplace 方法可以使代码更简洁，并可能提高效率。目前该检查不支持 insert 方法。它也不支持关联容器的 insert 函数，因为将 insert 替换为 emplace 可能会导致性能下降，但未来可能会通过添加选项来支持。

The ContainersWithPushBack、ContainersWithPush 和 ContainersWithPushFront 选项用于指定支持 push_back、push 和 push_front 操作的容器类型。这些选项的默认值如下：

- ContainersWithPushBack：std::vector、std::deque 和 std::list。
- ContainersWithPush：std::stack、std::queue 和 std::priority_queue。
- ContainersWithPushFront：std::forward_list、std::list 和 std::deque。

该检查还会报告 emplace 类方法使用不当的情况，例如在调用 emplace_back 时仍显式调用构造函数。这会创建一个临时对象，最好的情况是移动，最坏的情况是复制。几乎所有 STL 中的 emplace 类函数都包含在检查范围内，std::map 和 std::unordered_map 的 try_emplace 是个例外，因为它的行为与其他方法略有不同。可以通过 EmplacyFunctions 选项添加更多容器，只要容器定义了 value_type 类型，并且 emplace 类函数构造的是 value_type 对象。

Before:

之前：

```c++
std::vector<MyClass> v;
v.push_back(MyClass(21, 37));
v.emplace_back(MyClass(21, 37));

std::vector<std::pair<int, int>> w;

w.push_back(std::pair<int, int>(21, 37));
w.push_back(std::make_pair(21L, 37L));
w.emplace_back(std::make_pair(21L, 37L));
```

After:

之后：

```c++
std::vector<MyClass> v;
v.emplace_back(21, 37);
v.emplace_back(21, 37);

std::vector<std::pair<int, int>> w;
w.emplace_back(21, 37);
w.emplace_back(21L, 37L);
w.emplace_back(21L, 37L);
```

By default, the check is able to remove unnecessary std::make_pair and
std::make_tuple calls from push_back calls on containers of
std::pair and std::tuple. Custom tuple-like types can be modified by
the TupleTypes 选项；自定义 make 函数可以通过 TupleMakeFunctions 选项进行修改。

默认情况下，该检查可以从针对 std::pair 和 std::tuple 容器的 push_back 调用中移除不必要的 std::make_pair 和 std::make_tuple 调用。自定义的类似 tuple 的类型可以通过 TupleTypes 选项进行配置；自定义的 make 函数可以通过 TupleMakeFunctions 选项进行配置。

The other situation is when we pass arguments that will be converted to
a type inside a container.

另一种情况是我们传递的参数会被转换为容器中的某种类型。

Before:

之前：

```c++
std::vector<boost::optional<std::string> > v;
v.push_back("abc");
```

After:

之后：

```c++
std::vector<boost::optional<std::string> > v;
v.emplace_back("abc");
```

In some cases the transformation would be valid, but the code wouldn't
be exception safe. In this case the calls of push_back won't be
replaced.

在某些情况下，转换是合法的，但代码不具备异常安全性。在这种情况下，push_back 的调用不会被替换。

```c++
std::vector<std::unique_ptr<int>> v;
v.push_back(std::unique_ptr<int>(new int(0)));
auto *ptr = new int(1);
v.push_back(std::unique_ptr<int>(ptr));
```

This is because replacing it with emplace_back could cause a leak of
this pointer if emplace_back would throw exception before emplacement
(e.g. not enough memory to add a new element).

这是因为如果 emplace_back 在构造元素之前抛出异常（例如内存不足），替换为 emplace_back 可能会导致指针泄漏。

For more info read item 42 - "Consider emplacement instead of
insertion." of Scott Meyers "Effective Modern C++".

更多信息请参阅 Scott Meyers 的《Effective Modern C++》中的第 42 条：“考虑使用 emplacement 替代插入”。

The default smart pointers that are considered are std::unique_ptr,
std::shared_ptr, std::auto_ptr. To specify other smart pointers or
other classes use the SmartPointers 选项。

默认考虑的智能指针包括 std::unique_ptr、std::shared_ptr 和 std::auto_ptr。要指定其他智能指针或类，请使用 SmartPointers 选项。

Check also doesn't fire if any argument of the constructor call would
be:

- a bit-field (bit-fields can't bind to rvalue/universal reference)
- a new expression (to avoid leak)
- if the argument would be converted via derived-to-base cast.

如果构造函数调用中的任何参数是以下情况，该检查也不会触发：

- 位域（位域不能绑定到右值或万能引用）
- new 表达式（为了避免内存泄漏）
- 参数通过派生类到基类的转换

This check requires C++11 or higher to run.

该检查要求使用 C++11 或更高版本。

## Options

## 选项

::: option
ContainersWithPushBack

Semicolon-separated list of class names of custom containers that
support push_back.

支持 push_back 的自定义容器类名列表，用分号分隔。
:::

::: option
ContainersWithPush

Semicolon-separated list of class names of custom containers that
support push.

支持 push 的自定义容器类名列表，用分号分隔。
:::

::: option
ContainersWithPushFront

Semicolon-separated list of class names of custom containers that
support push_front.

支持 push_front 的自定义容器类名列表，用分号分隔。
:::

::: option
IgnoreImplicitConstructors

When true, the check will ignore implicitly constructed
arguments of push_back, e.g.

当设置为 true 时，该检查将忽略 push_back 中隐式构造的参数，例如：

```c++
std::vector<std::string> v;
v.push_back("a"); // Ignored when IgnoreImplicitConstructors is `true`.
```

Default is false.

默认值为 false。
:::

::: option
SmartPointers

Semicolon-separated list of class names of custom smart pointers.

自定义智能指针类名列表，用分号分隔。
:::

::: option
TupleTypes

Semicolon-separated list of std::tuple-like class names.

类似 std::tuple 的类名列表，用分号分隔。
:::

::: option
TupleMakeFunctions

Semicolon-separated list of std::make_tuple-like function names. Those
function calls will be removed from push_back calls and turned into
emplace_back.

类似 std::make_tuple 的函数名列表，用分号分隔。这些函数调用将从 push_back 中移除，并替换为 emplace_back。
:::

::: option
EmplacyFunctions

Semicolon-separated list of containers without their template parameters
and some emplace-like method of the container. Example:
vector::emplace_back. Those methods will be checked for improper use
and the check will report when a temporary is unnecessarily created. All
STL containers with such member functions are supported by default.

不带模板参数的容器名及其某个 emplace 类方法的列表，用分号分隔。例如：vector::emplace_back。这些方法将被检查是否使用不当，当不必要地创建临时对象时会报告。所有带有此类成员函数的 STL 容器默认都受支持。
:::

### Example

示例：

```c++
std::vector<MyTuple<int, bool, char>> x;
x.push_back(MakeMyTuple(1, false, 'x'));
x.emplace_back(MakeMyTuple(1, false, 'x'));
```

transforms to:

转换为：

```c++
std::vector<MyTuple<int, bool, char>> x;
x.emplace_back(1, false, 'x');
x.emplace_back(1, false, 'x');
```

when TupleTypes is set to MyTuple,
TupleMakeFunctions is set to MakeMyTuple, and
EmplacyFunctions is set to vector::emplace_back.

当 TupleTypes 设置为 MyTuple，TupleMakeFunctions 设置为 MakeMyTuple，EmplacyFunctions 设置为 vector::emplace_back 时。
