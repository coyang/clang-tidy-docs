# bugprone-use-after-move

Warns if an object is used after it has been moved, for example:

如果一个对象在被移动之后仍然被使用，该检查器会发出警告，例如：

```c++
std::string str = "Hello, world!\n";
std::vector<std::string> messages;
messages.emplace_back(std::move(str));
std::cout << str;
```

The last line will trigger a warning that str is used after it has been moved.

最后一行会触发一个警告，提示 str 在被移动之后仍被使用。

The check does not trigger a warning if the object is reinitialized after the move and before the use. For example, no warning will be output for this code:

如果对象在移动之后、使用之前被重新初始化，则不会触发警告。例如，以下代码不会输出警告：

```c++
messages.emplace_back(std::move(str));
str = "Greetings, stranger!\n";
std::cout << str;
```

Subsections below explain more precisely what exactly the check considers to be a move, use, and reinitialization.

以下小节将更精确地解释该检查器如何判断移动、使用和重新初始化。

The check takes control flow into account. A warning is only emitted if the use can be reached from the move. This means that the following code does not produce a warning:

该检查器会考虑控制流。只有在使用可以从移动路径到达的情况下才会发出警告。这意味着以下代码不会产生警告：

```c++
if (condition) {
  messages.emplace_back(std::move(str));
} else {
  std::cout << str;
}
```

On the other hand, the following code does produce a warning:

另一方面，以下代码会产生警告：

```c++
for (int i = 0; i < 10; ++i) {
  std::cout << str;
  messages.emplace_back(std::move(str));
}
```

(The use-after-move happens on the second iteration of the loop.)

（在循环的第二次迭代中发生了移动后使用。）

In some cases, the check may not be able to detect that two branches are mutually exclusive. For example (assuming that i is an int):

在某些情况下，该检查器可能无法检测两个分支是互斥的。例如（假设 i 是一个 int）：

```c++
if (i == 1) {
  messages.emplace_back(std::move(str));
}
if (i == 2) {
  std::cout << str;
}
```

In this case, the check will erroneously produce a warning, even though it is not possible for both the move and the use to be executed. More formally, the analysis is flow-sensitive but not path-sensitive.

在这种情况下，尽管移动和使用不可能同时执行，该检查器仍会错误地发出警告。更正式地说，该分析是流敏感的，但不是路径敏感的。

## Silencing erroneous warnings

## 消除错误警告

An erroneous warning can be silenced by reinitializing the object after the move:

可以通过在移动之后重新初始化对象来消除错误警告：

```c++
if (i == 1) {
  messages.emplace_back(std::move(str));
  str = "";
}
if (i == 2) {
  std::cout << str;
}
```

If you want to avoid the overhead of actually reinitializing the object, you can create a dummy function that causes the check to assume the object was reinitialized:

如果你想避免实际重新初始化对象所带来的开销，可以创建一个虚拟函数，使检查器认为对象已经被重新初始化：

```c++
template <class T>
void IS_INITIALIZED(T&) {}
```

You can use this as follows:

你可以如下使用它：

```c++
if (i == 1) {
  messages.emplace_back(std::move(str));
}
if (i == 2) {
  IS_INITIALIZED(str);
  std::cout << str;
}
```

The check will not output a warning in this case because passing the object to a function as a non-const pointer or reference counts as a reinitialization (see section Reinitialization below).

在这种情况下，检查器不会输出警告，因为将对象作为非常量指针或引用传递给函数会被视为重新初始化（参见下文的“重新初始化”部分）。

## Unsequenced moves, uses, and reinitializations

## 未排序的移动、使用和重新初始化

In many cases, C++ does not make any guarantees about the order in which sub-expressions of a statement are evaluated. This means that in code like the following, it is not guaranteed whether the use will happen before or after the move:

在许多情况下，C++ 不保证语句中子表达式的求值顺序。这意味着在如下代码中，无法保证使用发生在移动之前还是之后：

```c++
void f(int i, std::vector<int> v);
std::vector<int> v = { 1, 2, 3 };
f(v[1], std::move(v));
```

In this kind of situation, the check will note that the use and move are unsequenced.

在这种情况下，检查器会指出使用和移动是未排序的。

The check will also take sequencing rules into account when reinitializations occur in the same statement as moves or uses. A reinitialization is only considered to reinitialize a variable if it is guaranteed to be evaluated after the move and before the use.

当重新初始化与移动或使用出现在同一语句中时，检查器也会考虑求值顺序。只有在重新初始化被保证在移动之后、使用之前执行时，才会被视为有效的重新初始化。

## Move

## 移动

The check currently only considers calls of std::move on local variables or function parameters. It does not check moves of member variables or global variables.

该检查器目前只考虑对局部变量或函数参数调用 std::move 的情况。它不会检查成员变量或全局变量的移动。

Any call of std::move on a variable is considered to cause a move of that variable, even if the result of std::move is not passed to an rvalue reference parameter.

对变量调用 std::move 会被视为该变量发生了移动，即使 std::move 的结果并未传递给右值引用参数。

This means that the check will flag a use-after-move even on a type that does not define a move constructor or move assignment operator. This is intentional. Developers may use std::move on such a type in the expectation that the type will add move semantics in the future. If such a std::move has the potential to cause a use-after-move, we want to warn about it even if the type does not implement move semantics yet.

这意味着即使某个类型没有定义移动构造函数或移动赋值运算符，该检查器也会在使用 std::move 后发出使用后移动的警告。这是有意为之。开发者可能会在期望该类型将来添加移动语义的前提下使用 std::move。如果这样的 std::move 有可能导致移动后使用，我们希望即使该类型尚未实现移动语义也发出警告。

Furthermore, if the result of std::move is passed to an rvalue reference parameter, this will always be considered to cause a move, even if the function that consumes this parameter does not move from it, or if it does so only conditionally. For example, in the following situation, the check will assume that a move always takes place:

此外，如果 std::move 的结果被传递给右值引用参数，这将始终被视为发生了移动，即使接收该参数的函数并未真正从中移动，或仅在某些条件下才移动。例如，在以下情况下，检查器将假设始终发生了移动：

```c++
std::vector<std::string> messages;
void f(std::string &&str) {
  // Only remember the message if it isn't empty.
  if (!str.empty()) {
    messages.emplace_back(std::move(str));
  }
}
std::string str = "";
f(std::move(str));
```

The check will assume that the last line causes a move, even though, in this particular case, it does not. Again, this is intentional.

检查器会认为最后一行发生了移动，尽管在这个特定情况下并没有。再次强调，这是有意为之。

There is one special case: A call to std::move inside a try_emplace call is conservatively assumed not to move. This is to avoid spurious warnings, as the check has no way to reason about the bool returned by try_emplace.

有一个特殊情况：在 try_emplace 调用中使用 std::move 会被保守地认为没有发生移动。这是为了避免产生虚假的警告，因为检查器无法判断 try_emplace 返回的布尔值。

When analyzing the order in which moves, uses and reinitializations happen (see section Unsequenced moves, uses, and reinitializations), the move is assumed to occur in whichever function the result of the std::move is passed to.

在分析移动、使用和重新初始化的发生顺序时（参见“未排序的移动、使用和重新初始化”部分），移动被认为发生在 std::move 的结果被传递到的函数中。

The check also handles perfect-forwarding with std::forward so the following code will also trigger a use-after-move warning.

该检查器也支持对 std::forward 的完美转发分析，因此以下代码也会触发使用后移动的警告。

```c++
void consume(int);

void f(int&& i) {
  consume(std::forward<int>(i));
  consume(std::forward<int>(i)); // use-after-move
}
```

## Use

## 使用

Any occurrence of the moved variable that is not a reinitialization (see below) is considered to be a use.

任何对已移动变量的使用，只要不是重新初始化（见下文），都被视为一次使用。

An exception to this are objects of type std::unique_ptr, std::shared_ptr, std::weak_ptr, std::optional, and std::any. An exception to this are objects of type std::unique_ptr, std::shared_ptr, std::weak_ptr, std::optional, and std::any, which can be reinitialized via reset. For smart pointers specifically, the moved-from objects have a well-defined state of being nullptrs, and only operator\*, operator-> and operator[] are considered bad accesses as they would be dereferencing a nullptr.

std::unique_ptr、std::shared_ptr、std::weak_ptr、std::optional 和 std::any 类型的对象是个例外。这些对象可以通过 reset 重新初始化。对于智能指针来说，被移动后的对象处于明确的 nullptr 状态，只有 operator\*、operator-> 和 operator[] 被视为不良访问，因为它们会解引用一个 nullptr。

If multiple uses occur after a move, only the first of these is flagged.

如果在移动之后发生多次使用，只有第一次使用会被标记。

## Reinitialization

## 重新初始化

The check considers a variable to be reinitialized in the following cases:

在以下情况下，检查器认为变量已被重新初始化：

- The variable occurs on the left-hand side of an assignment.
- 变量出现在赋值语句的左侧。

- The variable is passed to a function as a non-const pointer or non-const lvalue reference. (It is assumed that the variable may be an out-parameter for the function.)
- 变量作为非常量指针或非常量左值引用传递给函数。（假设该变量可能是函数的输出参数。）

- clear() or assign() is called on the variable and the variable is of one of the standard container types basic_string, vector, deque, forward_list, list, set, map, multiset, multimap, unordered_set, unordered_map, unordered_multiset, unordered_multimap.
- 对变量调用 clear() 或 assign()，且变量属于以下标准容器类型之一：basic_string、vector、deque、forward_list、list、set、map、multiset、multimap、unordered_set、unordered_map、unordered_multiset、unordered_multimap。

- reset() is called on the variable and the variable is of type std::unique_ptr, std::shared_ptr, std::weak_ptr, std::optional, or std::any.
- 对变量调用 reset()，且变量类型为 std::unique_ptr、std::shared_ptr、std::weak_ptr、std::optional 或 std::any。

- A member function marked with the [[clang::reinitializes]] attribute is called on the variable.
- 对变量调用了带有 [[clang::reinitializes]] 属性的成员函数。

If the variable in question is a struct and an individual member variable of that struct is written to, the check does not consider this to be a reinitialization -- even if, eventually, all member variables of the struct are written to. For example:

如果该变量是一个结构体，并且只对其某个成员变量进行了赋值，检查器不会认为这是一次重新初始化 —— 即使最终所有成员变量都被赋值。例如：

```c++
struct S {
  std::string str;
  int i;
};
S s = { "Hello, world!\n", 42 };
S s_other = std::move(s);
s.str = "Lorem ipsum";
s.i = 99;
```

The check will not consider s to be reinitialized after the last line; instead, the line that assigns to s.str will be flagged as a use-after-move. This is intentional as this pattern of reinitializing a struct is error-prone. For example, if an additional member variable is added to S, it is easy to forget to add the reinitialization for this additional member. Instead, it is safer to assign to the entire struct in one go, and this will also avoid the use-after-move warning.

检查器不会认为 s 在最后一行之后被重新初始化；相反，赋值给 s.str 的那一行会被标记为使用后移动。这是有意为之，因为这种逐个成员重新初始化结构体的方式容易出错。例如，如果向 S 添加了一个新的成员变量，很容易忘记对其进行重新初始化。更安全的做法是一次性给整个结构体赋值，这也可以避免使用后移动的警告。
