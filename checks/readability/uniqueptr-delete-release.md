# readability-uniqueptr-delete-release

Replace `delete <unique_ptr>.release()` with `<unique_ptr> = nullptr`.
The latter is shorter, simpler and does not require use of raw pointer
APIs.

```c++
std::unique_ptr<int> P;
delete P.release();

// becomes

std::unique_ptr<int> P;
P = nullptr;
```

## Options

::: option
PreferResetCall

If [true]{.title-ref}, refactor by calling the reset member function
instead of assigning to `nullptr`. Default value is [false]{.title-ref}.

```c++
std::unique_ptr<int> P;
delete P.release();

// becomes

std::unique_ptr<int> P;
P.reset();
```

:::
