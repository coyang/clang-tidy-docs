# bugprone-bitwise-pointer-cast

Warns about code that tries to cast between pointers by means of std::bit_cast or memcpy.

警告使用 std::bit_cast 或 memcpy 在指针之间进行类型转换的代码。

The motivation is that std::bit_cast is advertised as the safe alternative to type punning via reinterpret_cast in modern C++. However, one should not blindly replace reinterpret_cast with std::bit_cast, as follows:

其动机是 std::bit_cast 被宣传为现代 C++ 中通过 reinterpret_cast 进行类型双关（type punning）的安全替代方案。然而，不应盲目地将 reinterpret_cast 替换为 std::bit_cast，如下所示：

```c++
int x{};
-float y = *reinterpret_cast<float*>(&x);
+float y = *std::bit_cast<float*>(&x);
```

The drop-in replacement behaves exactly the same as reinterpret_cast, and Undefined Behavior is still invoked. std::bit_cast is copying the bytes of the input pointer, not the pointee, into an output pointer of a different type, which may violate the strict aliasing rules. However, simply looking at the code, it looks "safe", because it uses std::bit_cast which is advertised as safe.

这种替换方式的行为与 reinterpret_cast 完全相同，仍然会触发未定义行为。std::bit_cast 是将输入指针的字节（而不是其指向的对象）复制到不同类型的输出指针中，这可能违反严格别名规则。然而，单从代码表面来看，它看起来是“安全”的，因为它使用了被宣传为安全的 std::bit_cast。

The solution to safe type punning is to apply std::bit_cast on value types, not on pointer types:

安全进行类型双关的解决方案是将 std::bit_cast 应用于值类型，而不是指针类型：

```c++
int x{};
float y = std::bit_cast<float>(x);
```

This way, the bytes of the input object are copied into the output object, which is much safer. Do note that Undefined Behavior can still occur, if there is no value of type To corresponding to the value representation produced. Compilers may be able to optimize this copy and generate identical assembly to the original reinterpret_cast version.

通过这种方式，输入对象的字节被复制到输出对象中，这样要安全得多。但请注意，如果生成的值表示在目标类型 To 中没有对应的值，仍然可能发生未定义行为。编译器可能会优化此复制操作，并生成与原始 reinterpret_cast 版本相同的汇编代码。

Code before C++20 may backport std::bit_cast by means of memcpy, or simply call memcpy directly, which is equally problematic. This is also detected by this check:

在 C++20 之前的代码中，可能通过 memcpy 回溯实现 std::bit_cast，或者直接调用 memcpy，这同样存在问题。此检查也会检测到这种情况：

```c++
int* x{};
float* y{};
std::memcpy(&y, &x, sizeof(x));
```

Alternatively, if a cast between pointers is truly wanted, reinterpret_cast should be used, to clearly convey the intent and enable warnings from compilers and linters, which should be addressed accordingly.

或者，如果确实需要在指针之间进行类型转换，应使用 reinterpret_cast，以清晰地表达意图，并启用编译器和代码检查工具的警告，从而可以相应地进行处理。
