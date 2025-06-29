# bugprone-throw-keyword-missing

Warns about a potentially missing throw keyword. If a temporary object is created, but the object's type derives from (or is the same as) a class that has 'EXCEPTION', 'Exception' or 'exception' in its name, we can assume that the programmer's intention was to throw that object.

警告可能遗漏的 throw 关键字。如果创建了一个临时对象，而该对象的类型是从名称中包含 'EXCEPTION'、'Exception' 或 'exception' 的类派生的（或与其相同），我们可以推测程序员的意图是抛出该对象。

Example:

示例：

```c++
void f(int i) {
  if (i < 0) {
    // Exception is created but is not thrown.
    // 异常对象被创建但未被抛出。
    std::runtime_error("Unexpected argument");
  }
}
```
