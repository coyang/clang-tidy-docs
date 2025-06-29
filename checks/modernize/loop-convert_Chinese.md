以下是翻译后的 markdown 文件内容，保留了中英文双语格式，中文翻译位于英文下方，并以一个空行分隔：

# modernize-loop-convert

This check converts for(...; ...; ...) loops to use the new range-based loops in C++11.

该检查会将 for(...; ...; ...) 循环转换为 C++11 中的新范围基循环（range-based for loop）。

Three kinds of loops can be converted:

三种类型的循环可以被转换：

- Loops over statically allocated arrays.
- 静态分配数组上的循环。

- Loops over containers, using iterators.
- 使用迭代器遍历容器的循环。

- Loops over array-like containers, using operator[] and at().
- 使用 operator[] 和 at() 访问类数组容器的循环。

## MinConfidence option

## MinConfidence 选项

### risky

In loops where the container expression is more complex than just a reference to a declared expression (a variable, function, enum, etc.), and some part of it appears elsewhere in the loop, we lower our confidence in the transformation due to the increased risk of changing semantics. Transformations for these loops are marked as risky, and thus will only be converted if the minimum required confidence level is set to risky.

当循环中容器表达式比简单的已声明表达式（变量、函数、枚举等）的引用更复杂，且其某部分在循环中其他地方也被使用时，由于更高的语义变化风险，我们会降低对转换的信心。这类循环的转换被标记为 risky，因此仅当最小所需置信级别设置为 risky 时才会被转换。

```c++
int arr[10][20];
int l = 5;

for (int j = 0; j < 20; ++j)
  int k = arr[l][j] + l; // using l outside arr[l] is considered risky

for (int i = 0; i < obj.getVector().size(); ++i)
  obj.foo(10); // using 'obj' is considered risky
```

See Range-based loops evaluate end() only once<IncorrectRiskyTransformation> for an example of an incorrect transformation when the minimum required confidence level is set to risky.

参见 Range-based loops evaluate end() only once<IncorrectRiskyTransformation>，了解当最小置信级别设置为 risky 时发生不正确转换的示例。

### reasonable (Default)

If a loop calls .end() or .size() after each iteration, the transformation for that loop is marked as reasonable, and thus will be converted if the required confidence level is set to reasonable (default) or lower.

如果一个循环在每次迭代后调用 .end() 或 .size()，则该循环的转换被标记为 reasonable，因此当所需置信级别设置为 reasonable（默认）或更低时将会被转换。

```c++
// using size() is considered reasonable
for (int i = 0; i < container.size(); ++i)
  cout << container[i];
```

### safe

Any other loops that do not match the above criteria to be marked as risky or reasonable are marked safe, and thus will be converted if the required confidence level is set to safe or lower.

其他不符合上述 risky 或 reasonable 标准的循环将被标记为 safe，因此当所需置信级别设置为 safe 或更低时将会被转换。

```c++
int arr[] = {1,2,3};

for (int i = 0; i < 3; ++i)
  cout << arr[i];
```

## Example

## 示例

Original:

原始代码：

```c++
const int N = 5;
int arr[] = {1,2,3,4,5};
vector<int> v;
v.push_back(1);
v.push_back(2);
v.push_back(3);

// safe conversion
for (int i = 0; i < N; ++i)
  cout << arr[i];

// reasonable conversion
for (vector<int>::iterator it = v.begin(); it != v.end(); ++it)
  cout << *it;

// reasonable conversion
for (vector<int>::iterator it = begin(v); it != end(v); ++it)
  cout << *it;

// reasonable conversion
for (vector<int>::iterator it = std::begin(v); it != std::end(v); ++it)
  cout << *it;

// reasonable conversion
for (int i = 0; i < v.size(); ++i)
  cout << v[i];

// reasonable conversion
for (int i = 0; i < size(v); ++i)
  cout << v[i];
```

After applying the check with minimum confidence level set to reasonable (default):

在将最小置信级别设置为 reasonable（默认）并应用检查后：

```c++
const int N = 5;
int arr[] = {1,2,3,4,5};
vector<int> v;
v.push_back(1);
v.push_back(2);
v.push_back(3);

// safe conversion
for (auto & elem : arr)
  cout << elem;

// reasonable conversion
for (auto & elem : v)
  cout << elem;

// reasonable conversion
for (auto & elem : v)
  cout << elem;
```

## Reverse Iterator Support

## 反向迭代器支持

The converter is also capable of transforming iterator loops which use rbegin and rend for looping backwards over a container. Out of the box this will automatically happen in C++20 mode using the ranges library, however the check can be configured to work without C++20 by specifying a function to reverse a range and optionally the header file where that function lives.

该转换器还可以转换使用 rbegin 和 rend 进行反向遍历容器的迭代器循环。在 C++20 模式下，默认会使用 ranges 库自动完成此操作，但也可以通过指定一个用于反转范围的函数（以及该函数所在的头文件）来配置该检查，使其在非 C++20 模式下工作。

## Options

## 选项

::: option
UseCxx20ReverseRanges

When set to true convert loops when in C++20 or later mode using std::ranges::reverse_view. Default value is true.

当设置为 true 时，在 C++20 或更高版本模式下使用 std::ranges::reverse_view 转换循环。默认值为 true。
:::

::: option
MakeReverseRangeFunction

Specify the function used to reverse an iterator pair, the function should accept a class with rbegin and rend methods and return a class with begin and end methods that call the rbegin and rend methods respectively. Common examples are ranges::reverse_view and llvm::reverse. Default value is an empty string.

指定用于反转迭代器对的函数，该函数应接受一个具有 rbegin 和 rend 方法的类，并返回一个具有 begin 和 end 方法的类，这些方法分别调用 rbegin 和 rend。常见示例包括 ranges::reverse_view 和 llvm::reverse。默认值为空字符串。
:::

::: option
MakeReverseRangeHeader

Specifies the header file where MakeReverseRangeFunction is declared. For the previous examples this option would be set to range/v3/view/reverse.hpp and llvm/ADT/STLExtras.h respectively. If this is an empty string and MakeReverseRangeFunction is set, the check will proceed on the assumption that the function is already available in the translation unit. This can be wrapped in angle brackets to signify to add the include as a system include. Default value is an empty string.

指定声明 MakeReverseRangeFunction 的头文件。对于前面的示例，此选项应分别设置为 range/v3/view/reverse.hpp 和 llvm/ADT/STLExtras.h。如果该值为空字符串且 MakeReverseRangeFunction 已设置，则检查将假设该函数已在翻译单元中可用。可以使用尖括号包裹以表示作为系统头文件包含。默认值为空字符串。
:::

::: option
IncludeStyle

A string specifying which include-style is used, llvm or google. Default is llvm.

指定使用哪种 include 风格的字符串，可选值为 llvm 或 google。默认值为 llvm。
:::

## Limitations

## 限制

There are certain situations where the tool may erroneously perform transformations that remove information and change semantics. Users of the tool should be aware of the behavior and limitations of the check outlined by the cases below.

在某些情况下，该工具可能会错误地执行转换，从而丢失信息并改变语义。工具用户应了解以下示例中概述的该检查的行为和限制。

### Comments inside loop headers

### 循环头中的注释

Comments inside the original loop header are ignored and deleted when transformed.

原始循环头中的注释在转换时会被忽略并删除。

```c++
for (int i = 0; i < N; /* This will be deleted */ ++i) { }
```

### Range-based loops evaluate end() only once

### 范围基循环仅调用一次 end()

The C++11 range-based for loop calls .end() only once during the initialization of the loop. If in the original loop .end() is called after each iteration the semantics of the transformed loop may differ.

C++11 的范围基 for 循环在初始化时仅调用一次 .end()。如果原始循环在每次迭代后都调用 .end()，则转换后的循环语义可能会不同。

```c++
// The following is semantically equivalent to the C++11 range-based for loop,
// therefore the semantics of the header will not change.
for (iterator it = container.begin(), e = container.end(); it != e; ++it) { }

// Instead of calling .end() after each iteration, this loop will be
// transformed to call .end() only once during the initialization of the loop,
// which may affect semantics.
for (iterator it = container.begin(); it != container.end(); ++it) { }
```

::: {#IncorrectRiskyTransformation}
As explained above, calling member functions of the container in the body of the loop is considered risky. If the called member function modifies the container the semantics of the converted loop will differ due to .end() being called only once.

如上所述，在循环体中调用容器的成员函数被视为 risky。如果被调用的成员函数修改了容器，由于 .end() 仅被调用一次，转换后的循环语义将会不同。
:::

```c++
bool flag = false;
for (vector<T>::iterator it = vec.begin(); it != vec.end(); ++it) {
  // Add a copy of the first element to the end of the vector.
  if (!flag) {
    // This line makes this transformation 'risky'.
    vec.push_back(*it);
    flag = true;
  }
  cout << *it;
}
```

The original code above prints out the contents of the container including the newly added element while the converted loop, shown below, will only print the original contents and not the newly added element.

上述原始代码会打印出容器的内容，包括新添加的元素，而下面转换后的循环只会打印原始内容，不包括新添加的元素。

```c++
bool flag = false;
for (auto & elem : vec) {
  // Add a copy of the first element to the end of the vector.
  if (!flag) {
    // This line makes this transformation 'risky'
    vec.push_back(elem);
    flag = true;
  }
  cout << elem;
}
```

Semantics will also be affected if .end() has side effects. For example, in the case where calls to .end() are logged the semantics will change in the transformed loop if .end() was originally called after each iteration.

如果 .end() 有副作用，语义也会受到影响。例如，如果对 .end() 的调用会被记录日志，而原始循环在每次迭代后都调用 .end()，则转换后的循环语义将会改变。

```c++
iterator end() {
  num_of_end_calls++;
  return container.end();
}
```

### Overloaded operator->() with side effects

### 带副作用的 operator->() 重载

Similarly, if operator->() was overloaded to have side effects, such as logging, the semantics will change. If the iterator's operator->() was used in the original loop it will be replaced with <container element>.<member> instead due to the implicit dereference as part of the range-based for loop. Therefore any side effect of the overloaded operator->() will no longer be performed.

同样地，如果 operator->() 被重载并具有副作用（如记录日志），则语义将会改变。如果原始循环中使用了迭代器的 operator->()，在范围基 for 循环中将被替换为 <容器元素>.<成员>，因为范围基循环会隐式解引用。因此，重载 operator->() 的副作用将不再执行。

```c++
for (iterator it = c.begin(); it != c.end(); ++it) {
  it->func(); // Using operator->()
}
// Will be transformed to:
for (auto & elem : c) {
  elem.func(); // No longer using operator->()
}
```

### Pointers and references to containers

### 指向容器的指针和引用

While most of the check's risk analysis is dedicated to determining whether the iterator or container was modified within the loop, it is possible to circumvent the analysis by accessing and modifying the container through a pointer or reference.

尽管该检查的大部分风险分析都致力于确定迭代器或容器是否在循环中被修改，但仍可能通过指针或引用访问并修改容器，从而绕过分析。

If the container were directly used instead of using the pointer or reference the following transformation would have only been applied at the risky level since calling a member function of the container is considered risky. The check cannot identify expressions associated with the container that are different than the one used in the loop header, therefore the transformation below ends up being performed at the safe level.

如果直接使用容器而不是通过指针或引用，则以下转换仅会在 risky 级别下执行，因为调用容器的成员函数被认为是 risky。该检查无法识别与容器相关但与循环头中使用的表达式不同的表达式，因此以下转换最终会在 safe 级别下执行。

```c++
vector<int> vec;

vector<int> *ptr = &vec;
vector<int> &ref = vec;

for (vector<int>::iterator it = vec.begin(), e = vec.end(); it != e; ++it) {
  if (!flag) {
    // Accessing and modifying the container is considered risky, but the risk
    // level is not raised here.
    ptr->push_back(*it);
    ref.push_back(*it);
    flag = true;
  }
}
```

### OpenMP

As range-based for loops are only available since OpenMP 5, this check should not be used on code with a compatibility requirement of OpenMP prior to version 5. It is intentional that this check does not make any attempts to exclude incorrect diagnostics on OpenMP for loops prior to OpenMP 5.

由于范围基 for 循环仅在 OpenMP 5 及之后版本中可用，因此该检查不应应用于需要兼容 OpenMP 5 之前版本的代码。该检查不会尝试排除在 OpenMP 5 之前版本中对 OpenMP for 循环的错误诊断，这是有意为之。

To prevent this check to be applied (and to break) OpenMP for loops but still be applied to non-OpenMP for loops the usage of NOLINT (see clang-tidy-nolint) on the specific for loops is recommended.

为了防止该检查应用于（并破坏）OpenMP for 循环，但仍应用于非 OpenMP for 循环，建议在特定的 for 循环上使用 NOLINT（参见 clang-tidy-nolint）。
