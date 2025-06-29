# abseil-no-namespace

Ensures code does not open namespace absl as that violates Abseil's compatibility guidelines. Code should not open namespace absl as that conflicts with Abseil's compatibility guidelines and may result in breakage.

确保代码不打开 namespace absl，因为这违反了 Abseil 的兼容性指南。代码不应打开 namespace absl，因为这与 Abseil 的兼容性指南相冲突，可能会导致程序出错。

Any code that uses:

任何使用以下形式的代码：

```c++
namespace absl {
 ...
}
```

will be prompted with a warning.

将会触发警告提示。

See the full Abseil compatibility guidelines for more information.

有关更多信息，请参阅完整的 Abseil 兼容性指南：  
[https://abseil.io/about/compatibility](https://abseil.io/about/compatibility)
