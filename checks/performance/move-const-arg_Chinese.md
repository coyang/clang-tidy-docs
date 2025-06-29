# performance-move-const-arg

The check warns  
该检查会发出警告：

- if std::move() is called with a constant argument,  
  如果 std::move() 被用于常量参数，

- if std::move() is called with an argument of a trivially-copyable type,  
  如果 std::move() 被用于一个可平凡复制（trivially-copyable）类型的参数，

- if the result of std::move() is passed as a const reference argument.  
  如果 std::move() 的结果被作为 const 引用参数传递。

In all three cases, the check will suggest a fix that removes the std::move().  
在以上三种情况下，该检查都会建议移除 std::move() 以修复问题。

Here are examples of each of the three cases:  
以下是每种情况的示例：

```c++
const string s;
return std::move(s);  // Warning: std::move of the const variable has no effect
                      // 警告：对 const 变量使用 std::move 无效

int x;
return std::move(x);  // Warning: std::move of the variable of a trivially-copyable type has no effect
                      // 警告：对可平凡复制类型变量使用 std::move 无效

void f(const string &s);
string s;
f(std::move(s));  // Warning: passing result of std::move as a const reference argument; no move will actually happen
                  // 警告：将 std::move 的结果作为 const 引用参数传递；实际上不会发生移动操作
```

## Options

## 选项

::: option
CheckTriviallyCopyableMove

If true, enables detection of trivially copyable types that do not have a move constructor. Default is true.  
如果为 true，则启用对没有移动构造函数的可平凡复制类型的检测。默认值为 true。
:::

::: option
CheckMoveToConstRef

If true, enables detection of std::move() passed as a const reference argument. Default is true.  
如果为 true，则启用对将 std::move() 的结果作为 const 引用参数传递的情况的检测。默认值为 true。
:::
