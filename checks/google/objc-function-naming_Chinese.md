# google-objc-function-naming

Finds function declarations in Objective-C files that do not follow the  
pattern described in the Google Objective-C Style Guide.

在 Objective-C 文件中查找不符合 Google Objective-C 样式指南中所描述模式的函数声明。

The corresponding style guide rule can be found here:  
<https://google.github.io/styleguide/objcguide.html#function-names>

相应的样式指南规则可在以下链接中找到：  
<https://google.github.io/styleguide/objcguide.html#function-names>

All function names should be in Pascal case. Functions whose storage  
class is not static should have an appropriate prefix.

所有函数名应使用帕斯卡命名法（Pascal Case）。对于存储类不是 static 的函数，应添加适当的前缀。

The following code sample does not follow this pattern:

以下代码示例不符合该命名规范：

```objc
static bool is_positive(int i) { return i > 0; }
bool IsNegative(int i) { return i < 0; }
```

The sample above might be corrected to the following code:

上面的示例可以修改为如下代码：

```objc
static bool IsPositive(int i) { return i > 0; }
bool *ABCIsNegative(int i) { return i < 0; }
```
