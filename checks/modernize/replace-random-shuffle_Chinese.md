# modernize-replace-random-shuffle

This check will find occurrences of std::random_shuffle and replace it with std::shuffle. In C++17 std::random_shuffle will no longer be available and thus we need to replace it.

此检查将查找 std::random_shuffle 的使用，并将其替换为 std::shuffle。在 C++17 中，std::random_shuffle 将不再可用，因此我们需要进行替换。

Below are two examples of what kind of occurrences will be found and two examples of what it will be replaced with.

下面是两个会被检测到的使用示例，以及两个替换后的示例。

```c++
std::vector<int> v;

// First example
std::random_shuffle(vec.begin(), vec.end());

// Second example
std::random_shuffle(vec.begin(), vec.end(), randomFunc);
```

两个示例都将被替换为：

```c++
std::shuffle(vec.begin(), vec.end(), std::mt19937(std::random_device()()));
```

The second example will also receive a warning that randomFunc is no longer supported in the same way as before so if the user wants the same functionality, the user will need to change the implementation of the randomFunc.

第二个示例还会收到一个警告，提示 randomFunc 不再以之前的方式支持，因此如果用户希望保持相同的功能，需要修改 randomFunc 的实现。

One thing to be aware of here is that std::random_device is quite expensive to initialize. So if you are using the code in a performance critical place, you probably want to initialize it elsewhere. Another thing is that the seeding quality of the suggested fix is quite poor: std::mt19937 has an internal state of 624 32-bit integers, but is only seeded with a single integer. So if you require higher quality randomness, you should consider seeding better, for example:

需要注意的一点是，std::random_device 的初始化开销较大。因此，如果你在性能敏感的地方使用这段代码，建议在其他地方初始化它。另一个问题是，建议的修复方式中种子的质量较差：std::mt19937 拥有 624 个 32 位整数的内部状态，但只用一个整数进行初始化。如果你需要更高质量的随机性，应该考虑更好的种子初始化方式，例如：

```c++
std::shuffle(v.begin(), v.end(), []() {
  std::mt19937::result_type seeds[std::mt19937::state_size];
  std::random_device device;
  std::uniform_int_distribution<typename std::mt19937::result_type> dist;
  std::generate(std::begin(seeds), std::end(seeds), [&] { return dist(device); });
  std::seed_seq seq(std::begin(seeds), std::end(seeds));
  return std::mt19937(seq);
}());
```
