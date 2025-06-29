# cert-msc50-cpp

Pseudorandom number generators use mathematical algorithms to produce a  
sequence of numbers with good statistical properties, but the numbers  
produced are not genuinely random. The std::rand() function takes a  
seed (number), runs a mathematical operation on it and returns the  
result. By manipulating the seed the result can be predictable. This  
check warns for the usage of std::rand().

伪随机数生成器使用数学算法生成具有良好统计特性的数字序列，但生成的数字并不是真正的随机数。std::rand() 函数接受一个种子（数字），对其执行数学运算并返回结果。通过操控种子，结果可能是可预测的。此检查会对使用 std::rand() 发出警告。

代码块示例：

```cpp
#include <cstdlib>
#include <ctime>

int main() {
  std::srand(std::time(nullptr));
  int r = std::rand();
}
```

```cpp
#include <cstdlib>
#include <ctime>

int main() {
  std::srand(std::time(nullptr));
  int r = std::rand();
}
```

In this example, std::rand() is used to generate a pseudorandom number.  
This is discouraged because the algorithm and seed can be predictable,  
leading to potential security vulnerabilities.

在此示例中，使用 std::rand() 来生成伪随机数。由于其算法和种子可能是可预测的，因此不推荐使用，这可能导致潜在的安全漏洞。

Recommended alternative:

推荐的替代方案：

```cpp
#include <random>

int main() {
  std::random_device rd;
  std::mt19937 gen(rd());
  std::uniform_int_distribution<> distrib(1, 100);
  int r = distrib(gen);
}
```

```cpp
#include <random>

int main() {
  std::random_device rd;
  std::mt19937 gen(rd());
  std::uniform_int_distribution<> distrib(1, 100);
  int r = distrib(gen);
}
```

This code uses the C++11 random library, which provides better randomness  
and is more suitable for security-sensitive applications.

此代码使用 C++11 的随机数库，它提供了更好的随机性，更适合对安全性要求较高的应用程序。
