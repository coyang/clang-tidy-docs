# google-objc-global-variable-declaration

Finds global variable declarations in Objective-C files that do not follow the pattern of variable names in Google's Objective-C Style Guide.

查找 Objective-C 文件中不符合 Google Objective-C 样式指南中变量命名规范的全局变量声明。

The corresponding style guide rule:  
<https://google.github.io/styleguide/objcguide.html#variable-names>

对应的样式指南规则：  
<https://google.github.io/styleguide/objcguide.html#variable-names>

All the global variables should follow the pattern of g[A-Z]._ (variables) or k[A-Z]._ (constants). The check will suggest a variable name that follows the pattern if it can be inferred from the original name.

所有全局变量应遵循 g[A-Z]._（变量）或 k[A-Z]._（常量）的命名模式。如果可以从原始名称推断出符合规范的名称，检查工具将建议一个符合该模式的变量名。

For code:

对于如下代码：

```objc
static NSString* myString = @"hello";
```

The fix will be:

修复后的代码为：

```objc
static NSString* gMyString = @"hello";
```

Another example of constant:

另一个常量的示例：

```objc
static NSString* const myConstString = @"hello";
```

The fix will be:

修复后的代码为：

```objc
static NSString* const kMyConstString = @"hello";
```

However for code that prefixed with non-alphabetical characters like:

然而，对于以下这种以非字母字符开头的代码：

```objc
static NSString* __anotherString = @"world";
```

The check will give a warning message but will not be able to suggest a fix. The user needs to fix it on their own.

检查工具会给出警告信息，但无法提供修复建议。用户需要自行进行修复。
