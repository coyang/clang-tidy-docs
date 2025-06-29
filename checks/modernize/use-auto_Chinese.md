# modernize-use-auto

# modernize-use-auto

This check is responsible for using the auto type specifier for variable declarations to improve code readability and maintainability. For example:

此检查用于在变量声明中使用 auto 类型说明符，以提高代码的可读性和可维护性。例如：

```c++
std::vector<int>::iterator I = my_container.begin();

// transforms to:

auto I = my_container.begin();
```

The auto type specifier will only be introduced in situations where the variable type matches the type of the initializer expression. In other words auto should deduce the same type that was originally spelled in the source. However, not every situation should be transformed:

auto 类型说明符仅在变量类型与初始化表达式的类型匹配时引入。换句话说，auto 应该推导出与源代码中原始声明相同的类型。然而，并非所有情况都应该被转换：

```c++
int val = 42;
InfoStruct &I = SomeObject.getInfo();

// Should not become:

auto val = 42;
auto &I = SomeObject.getInfo();
```

In this example using auto for builtins doesn't improve readability. In other situations it makes the code less self-documenting impairing readability and maintainability. As a result, auto is used only introduced in specific situations described below.

在此示例中，对内建类型使用 auto 并不会提升可读性。在其他情况下，它可能会降低代码的自说明性，从而影响可读性和可维护性。因此，auto 仅在以下特定情况下使用。

## Iterators

## 迭代器

Iterator type specifiers tend to be long and used frequently, especially in loop constructs. Since the functions generating iterators have a common format, the type specifier can be replaced without obscuring the meaning of code while improving readability and maintainability.

迭代器类型说明符通常较长且使用频繁，尤其是在循环结构中。由于生成迭代器的函数具有通用格式，因此可以替换类型说明符而不会影响代码含义，同时提升可读性和可维护性。

```c++
for (std::vector<int>::iterator I = my_container.begin(),
                                E = my_container.end();
     I != E; ++I) {
}

// becomes

for (auto I = my_container.begin(), E = my_container.end(); I != E; ++I) {
}
```

The check will only replace iterator type-specifiers when all of the following conditions are satisfied:

仅当满足以下所有条件时，此检查才会替换迭代器类型说明符：

- The iterator is for one of the standard containers in std namespace:
  - array
  - deque
  - forward_list
  - list
  - vector
  - map
  - multimap
  - set
  - multiset
  - unordered_map
  - unordered_multimap
  - unordered_set
  - unordered_multiset
  - queue
  - priority_queue
  - stack

- 迭代器属于 std 命名空间中的标准容器之一：
  - array（数组）
  - deque（双端队列）
  - forward_list（单向链表）
  - list（双向链表）
  - vector（向量）
  - map（映射）
  - multimap（多重映射）
  - set（集合）
  - multiset（多重集合）
  - unordered_map（无序映射）
  - unordered_multimap（无序多重映射）
  - unordered_set（无序集合）
  - unordered_multiset（无序多重集合）
  - queue（队列）
  - priority_queue（优先队列）
  - stack（栈）

- The iterator is one of the possible iterator types for standard containers:
  - iterator
  - reverse_iterator
  - const_iterator
  - const_reverse_iterator

- 迭代器是标准容器的以下迭代器类型之一：
  - iterator（正向迭代器）
  - reverse_iterator（反向迭代器）
  - const_iterator（常量正向迭代器）
  - const_reverse_iterator（常量反向迭代器）

- In addition to using iterator types directly, typedefs or other ways of referring to those types are also allowed. However, implementation-specific types for which a type like std::vector<int>::iterator is itself a typedef will not be transformed. Consider the following examples:

- 除了直接使用迭代器类型外，也允许使用 typedef 或其他引用这些类型的方式。然而，对于实现相关的类型（如 std::vector<int>::iterator 是 typedef 的情况）将不会被转换。请参考以下示例：

```c++
// The following direct uses of iterator types will be transformed.
std::vector<int>::iterator I = MyVec.begin();
{
  using namespace std;
  list<int>::iterator I = MyList.begin();
}

// The type specifier for J would transform to auto since it's a typedef
// to a standard iterator type.
typedef std::map<int, std::string>::const_iterator map_iterator;
map_iterator J = MyMap.begin();

// The following implementation-specific iterator type for which
// std::vector<int>::iterator could be a typedef would not be transformed.
__gnu_cxx::__normal_iterator<int*, std::vector> K = MyVec.begin();
```

- The initializer for the variable being declared is not a braced initializer list. Otherwise, use of auto would cause the type of the variable to be deduced as std::initializer_list.

- 被声明变量的初始化器不能是花括号初始化列表。否则，使用 auto 会导致变量类型被推导为 std::initializer_list。

## New expressions

## new 表达式

Frequently, when a pointer is declared and initialized with new, the pointee type is written twice: in the declaration type and in the new expression. In this case, the declaration type can be replaced with auto improving readability and maintainability.

通常，当指针通过 new 初始化时，指向类型会在声明和 new 表达式中重复出现。在这种情况下，可以使用 auto 替换声明类型，从而提高可读性和可维护性。

```c++
TypeName *my_pointer = new TypeName(my_param);

// becomes

auto *my_pointer = new TypeName(my_param);
```

The check will also replace the declaration type in multiple declarations, if the following conditions are satisfied:

如果满足以下条件，此检查也会替换多变量声明中的类型：

- All declared variables have the same type (i.e. all of them are pointers to the same type).
- All declared variables are initialized with a new expression.
- The types of all the new expressions are the same than the pointee of the declaration type.

- 所有声明的变量具有相同的类型（即它们都是指向相同类型的指针）。
- 所有声明的变量都使用 new 表达式进行初始化。
- 所有 new 表达式的类型与声明类型的指向类型相同。

```c++
TypeName *my_first_pointer = new TypeName, *my_second_pointer = new TypeName;

// becomes

auto *my_first_pointer = new TypeName, *my_second_pointer = new TypeName;
```

## Cast expressions

## 类型转换表达式

Frequently, when a variable is declared and initialized with a cast, the variable type is written twice: in the declaration type and in the cast expression. In this case, the declaration type can be replaced with auto improving readability and maintainability.

通常，当变量通过类型转换初始化时，变量类型会在声明和转换表达式中重复出现。在这种情况下，可以使用 auto 替换声明类型，从而提高可读性和可维护性。

```c++
TypeName *my_pointer = static_cast<TypeName>(my_param);

// becomes

auto *my_pointer = static_cast<TypeName>(my_param);
```

The check handles static_cast, dynamic_cast, const_cast, reinterpret_cast, functional casts, C-style casts and function templates that behave as casts, such as llvm::dyn_cast, boost::lexical_cast and gsl::narrow_cast. Calls to function templates are considered to behave as casts if the first template argument is explicit and is a type, and the function returns that type, or a pointer or reference to it.

该检查支持 static_cast、dynamic_cast、const_cast、reinterpret_cast、函数式转换、C 风格转换以及行为类似转换的函数模板，如 llvm::dyn_cast、boost::lexical_cast 和 gsl::narrow_cast。如果函数模板的第一个模板参数是显式类型，且函数返回该类型或其指针/引用，则视为类型转换。

## Known Limitations

## 已知限制

- If the initializer is an explicit conversion constructor, the check will not replace the type specifier even though it would be safe to do so.
- User-defined iterators are not handled at this time.

- 如果初始化器是显式转换构造函数，即使替换类型说明符是安全的，该检查也不会进行替换。
- 当前不支持用户自定义的迭代器。

## Options

## 选项

::: option
MinTypeNameLength

If the option is set to non-zero (default 5), the check will ignore type names having a length less than the option value. The option affects expressions only, not iterators. Spaces between multi-lexeme type names (long int) are considered as one. If the RemoveStars 选项（见下文）设置为 true，则类型中的 \* 也计入类型名长度。

如果该选项设置为非零值（默认值为 5），则检查将忽略长度小于该值的类型名。该选项仅影响表达式，不影响迭代器。多词类型名中的空格（如 long int）计为一个字符。如果 RemoveStars（见下文）设置为 true，则类型中的 \* 也计入类型名长度。
:::

```c++
// MinTypeNameLength = 0, RemoveStars=0

int a = static_cast<int>(foo());            // ---> auto a = ...
// length(bool *) = 4
bool *b = new bool;                         // ---> auto *b = ...
unsigned c = static_cast<unsigned>(foo());  // ---> auto c = ...

// MinTypeNameLength = 5, RemoveStars=0

int a = static_cast<int>(foo());                 // ---> int  a = ...
bool b = static_cast<bool>(foo());               // ---> bool b = ...
bool *pb = static_cast<bool*>(foo());            // ---> bool *pb = ...
unsigned c = static_cast<unsigned>(foo());       // ---> auto c = ...
// length(long <on-or-more-spaces> int) = 8
long int d = static_cast<long int>(foo());       // ---> auto d = ...

// MinTypeNameLength = 5, RemoveStars=1

int a = static_cast<int>(foo());                 // ---> int  a = ...
// length(int * * ) = 5
int **pa = static_cast<int**>(foo());            // ---> auto pa = ...
bool b = static_cast<bool>(foo());               // ---> bool b = ...
bool *pb = static_cast<bool*>(foo());            // ---> auto pb = ...
unsigned c = static_cast<unsigned>(foo());       // ---> auto c = ...
long int d = static_cast<long int>(foo());       // ---> auto d = ...
```

::: option
RemoveStars

If the option is set to true (default is false), the check will remove stars from the non-typedef pointer types when replacing type names with auto. Otherwise, the check will leave stars. For example:

如果该选项设置为 true（默认值为 false），则在用 auto 替换非 typedef 指针类型时，检查将移除星号（\*）。否则，保留星号。例如：
:::

```c++
TypeName *my_first_pointer = new TypeName, *my_second_pointer = new TypeName;

// RemoveStars = 0

auto *my_first_pointer = new TypeName, *my_second_pointer = new TypeName;

// RemoveStars = 1

auto my_first_pointer = new TypeName, my_second_pointer = new TypeName;
```
