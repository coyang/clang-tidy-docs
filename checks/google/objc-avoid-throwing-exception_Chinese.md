# google-objc-avoid-throwing-exception

Finds uses of throwing exceptions usages in Objective-C files.

查找在 Objective-C 文件中使用抛出异常的情况。

For the same reason as the Google C++ style guide, we prefer not throwing exceptions from Objective-C code.

出于与 Google C++ 风格指南相同的原因，我们不建议在 Objective-C 代码中抛出异常。

The corresponding C++ style guide rule:  
<https://google.github.io/styleguide/cppguide.html#Exceptions>

对应的 C++ 风格指南规则：  
<https://google.github.io/styleguide/cppguide.html#Exceptions>

Instead, prefer passing in NSError \*\* and return BOOL to indicate success or failure.

我们建议使用 NSError \*\* 参数并返回 BOOL 值来表示操作成功或失败。

A counterexample:

一个反面示例：

```objc
- (void)readFile {
  if ([self isError]) {
    @throw [NSException exceptionWithName:...];
  }
}
```

Instead, returning an error via NSError \*\* is preferred:

推荐的做法是通过 NSError \*\* 返回错误信息：

```objc
- (BOOL)readFileWithError:(NSError **)error {
  if ([self isError]) {
    *error = [NSError errorWithDomain:...];
    return NO;
  }
  return YES;
}
```

The corresponding style guide rule:  
<https://google.github.io/styleguide/objcguide.html#avoid-throwing-exceptions>

对应的风格指南规则：  
<https://google.github.io/styleguide/objcguide.html#avoid-throwing-exceptions>
