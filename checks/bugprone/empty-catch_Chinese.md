# bugprone-empty-catch

Detects and suggests addressing issues with empty catch statements.

检测并建议修复空的 catch 语句中的问题。

```c++
try {
  // Some code that can throw an exception
} catch(const std::exception&) {
}
```

Having empty catch statements in a codebase can be a serious problem that developers should be aware of. Catch statements are used to handle exceptions that are thrown during program execution. When an exception is thrown, the program jumps to the nearest catch statement that matches the type of the exception.

代码中存在空的 catch 语句可能是一个严重的问题，开发人员应当引起重视。catch 语句用于处理程序执行过程中抛出的异常。当异常被抛出时，程序会跳转到最近的、类型匹配的 catch 语句。

Empty catch statements, also known as "swallowing" exceptions, catch the exception but do nothing with it. This means that the exception is not handled properly, and the program continues to run as if nothing happened. This can lead to several issues, such as:

空的 catch 语句，也被称为“吞掉”异常，会捕获异常但不做任何处理。这意味着异常没有被正确处理，程序会像什么都没发生一样继续运行。这可能导致以下几个问题：

- Hidden Bugs: If an exception is caught and ignored, it can lead to hidden bugs that are difficult to diagnose and fix. The root cause of the problem may not be apparent, and the program may continue to behave in unexpected ways.

  隐藏的错误：如果异常被捕获但被忽略，可能会导致难以诊断和修复的隐藏错误。问题的根本原因可能不明显，程序可能会继续以不可预期的方式运行。

- Security Issues: Ignoring exceptions can lead to security issues, such as buffer overflows or null pointer dereferences. Hackers can exploit these vulnerabilities to gain access to sensitive data or execute malicious code.

  安全问题：忽略异常可能导致安全问题，例如缓冲区溢出或空指针解引用。黑客可能会利用这些漏洞访问敏感数据或执行恶意代码。

- Poor Code Quality: Empty catch statements can indicate poor code quality and a lack of attention to detail. This can make the codebase difficult to maintain and update, leading to longer development cycles and increased costs.

  代码质量差：空的 catch 语句可能表明代码质量差、缺乏细节关注。这会使代码库难以维护和更新，导致开发周期延长和成本增加。

- Unreliable Code: Code that ignores exceptions is often unreliable and can lead to unpredictable behavior. This can cause frustration for users and erode trust in the software.

  不可靠的代码：忽略异常的代码通常不可靠，可能导致不可预测的行为。这会让用户感到沮丧，并削弱对软件的信任。

To avoid these issues, developers should always handle exceptions properly. This means either fixing the underlying issue that caused the exception or propagating the exception up the call stack to a higher-level handler. If an exception is not important, it should still be logged or reported in some way so that it can be tracked and addressed later.

为了避免这些问题，开发人员应始终正确处理异常。这意味着要么修复导致异常的根本问题，要么将异常向上传递到更高层的处理器。如果异常不重要，也应以某种方式记录或报告，以便后续跟踪和处理。

If the exception is something that can be handled locally, then it should be handled within the catch block. This could involve logging the exception or taking other appropriate action to ensure that the exception is not ignored.

如果异常可以在本地处理，则应在 catch 块中进行处理。这可能包括记录异常或采取其他适当的措施，以确保异常不会被忽略。

Here is an example:

以下是一个示例：

```c++
try {
  // Some code that can throw an exception
} catch (const std::exception& ex) {
  // Properly handle the exception, e.g.:
  std::cerr << "Exception caught: " << ex.what() << std::endl;
}
```

If the exception cannot be handled locally and needs to be propagated up the call stack, it should be re-thrown or new exception should be thrown.

如果异常无法在本地处理并需要向上传递，则应重新抛出该异常或抛出新的异常。

Here is an example:

以下是一个示例：

```c++
try {
  // Some code that can throw an exception
} catch (const std::exception& ex) {
  // Re-throw the exception
  throw;
}
```

In some cases, catching the exception at this level may not be necessary, and it may be appropriate to let the exception propagate up the call stack. This can be done simply by not using try/catch block.

在某些情况下，在当前层级捕获异常可能并非必要，允许异常向上传递可能更合适。这可以通过不使用 try/catch 块来实现。

Here is an example:

以下是一个示例：

```c++
void function() {
  // Some code that can throw an exception
}

void callerFunction() {
  try {
    function();
  } catch (const std::exception& ex) {
    // Handling exception on higher level
    std::cerr << "Exception caught: " << ex.what() << std::endl;
  }
}
```

Other potential solution to avoid empty catch statements is to modify the code to avoid throwing the exception in the first place. This can be achieved by using a different API, checking for error conditions beforehand, or handling errors in a different way that does not involve exceptions. By eliminating the need for try-catch blocks, the code becomes simpler and less error-prone.

避免空 catch 语句的另一种可能解决方案是修改代码，从根本上避免抛出异常。这可以通过使用不同的 API、预先检查错误条件，或采用不依赖异常的其他错误处理方式来实现。通过消除对 try-catch 块的需求，代码将变得更简单、更不易出错。

Here is an example:

以下是一个示例：

```c++
// Old code:
try {
  mapContainer["Key"].callFunction();
} catch(const std::out_of_range&) {
}

// New code
if (auto it = mapContainer.find("Key"); it != mapContainer.end()) {
  it->second.callFunction();
}
```

In conclusion, empty catch statements are a bad practice that can lead to hidden bugs, security issues, poor code quality, and unreliable code. By handling exceptions properly, developers can ensure that their code is robust, secure, and maintainable.

总之，空的 catch 语句是一种不良实践，可能导致隐藏错误、安全问题、代码质量差以及代码不可靠。通过正确处理异常，开发人员可以确保其代码健壮、安全且易于维护。

## Options

选项

::: option
IgnoreCatchWithKeywords

This option can be used to ignore specific catch statements containing certain keywords. If a catch statement body contains (case-insensitive) any of the keywords listed in this semicolon-separated option, then the catch will be ignored, and no warning will be raised. Default value: [@TODO;@FIXME]{.title-ref}.

此选项可用于忽略包含特定关键字的 catch 语句。如果 catch 语句体中包含（不区分大小写）此分号分隔选项中列出的任意关键字，则该 catch 语句将被忽略，不会发出警告。默认值：[@TODO;@FIXME]{.title-ref}。
:::

::: option
AllowEmptyCatchForExceptions

This option can be used to ignore empty catch statements for specific exception types. By default, the check will raise a warning if an empty catch statement is detected, regardless of the type of exception being caught. However, in certain situations, such as when a developer wants to intentionally ignore certain exceptions or handle them in a different way, it may be desirable to allow empty catch statements for specific exception types. To configure this option, a semicolon-separated list of exception type names should be provided. If an exception type name in the list is caught in an empty catch statement, no warning will be raised. Default value: empty string.

此选项可用于忽略针对特定异常类型的空 catch 语句。默认情况下，如果检测到空的 catch 语句，无论捕获的异常类型如何，都会发出警告。然而，在某些情况下，例如开发人员希望有意忽略某些异常或以不同方式处理它们时，允许针对特定异常类型使用空的 catch 语句可能是合理的。要配置此选项，应提供一个以分号分隔的异常类型名称列表。如果列表中的异常类型在空的 catch 语句中被捕获，则不会发出警告。默认值：空字符串。
:::
