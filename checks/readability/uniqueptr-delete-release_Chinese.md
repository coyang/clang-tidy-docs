# readability-uniqueptr-delete-release

Replace delete <unique_ptr>.release() with <unique_ptr> = nullptr.  
将 delete <unique_ptr>.release() 替换为 <unique_ptr> = nullptr。

The latter is shorter, simpler and does not require use of raw pointer APIs.  
后者更简洁、更简单，并且不需要使用原始指针 API。

```c++
std::unique_ptr<int> P;
delete P.release();

// becomes
// 变为

std::unique_ptr<int> P;
P = nullptr;
```

## Options

## 选项

::: option  
PreferResetCall  
PreferResetCall

If true, refactor by calling the reset member function instead of assigning to nullptr. Default value is false.  
如果为 true，则通过调用 reset 成员函数进行重构，而不是赋值为 nullptr。默认值为 false。

```c++
std::unique_ptr<int> P;
delete P.release();

// becomes
// 变为

std::unique_ptr<int> P;
P.reset();
```

:::
