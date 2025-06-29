# cert-msc51-cpp

This check flags all pseudo-random number engines, engine adaptor instantiations and srand() when initialized or seeded with default argument, constant expression or any user-configurable type. Pseudo-random number engines seeded with a predictable value may cause vulnerabilities e.g. in security protocols. This is a CERT security rule, see MSC51-CPP. Ensure your random number generator is properly seeded and MSC32-C. Properly seed pseudorandom number generators.

此检查会标记所有伪随机数引擎、引擎适配器实例化以及在使用默认参数、常量表达式或任何用户可配置类型初始化或设定种子（seed）时调用的 srand()。使用可预测值设定种子的伪随机数引擎可能会在安全协议中引发漏洞。这是一条 CERT 安全规则，参见 MSC51-CPP. 确保你的随机数生成器已正确设定种子 和 MSC32-C. 正确设定伪随机数生成器的种子。

Examples:

示例：

```c++
void foo() {
  std::mt19937 engine1; // Diagnose, always generate the same sequence
                        // 诊断：总是生成相同的序列

  std::mt19937 engine2(1); // Diagnose
                           // 诊断

  engine1.seed(); // Diagnose
                  // 诊断

  engine2.seed(1); // Diagnose
                   // 诊断

  std::time_t t;
  engine1.seed(std::time(&t)); // Diagnose, system time might be controlled by user
                               // 诊断：系统时间可能被用户控制

  int x = atoi(argv[1]);
  std::mt19937 engine3(x);  // Will not warn
                            // 不会警告
}
```

## Options

选项

::: option
DisallowedSeedTypes

A comma-separated list of the type names which are disallowed. Default value is [time_t,std::time_t]{.title-ref}.

不允许使用的类型名称列表，使用逗号分隔。默认值为 [time_t,std::time_t]{.title-ref}。
:::
