# google-build-using-namespace

Finds using namespace directives.

查找 using namespace 指令。

The check implements the following rule of the Google C++ Style Guide:  
该检查实现了 Google C++ 风格指南中的以下规则：

> You may not use a using-directive to make all names from a namespace available.  
> 不允许使用 using 指令将某个命名空间中的所有名称引入当前作用域。

```c++
// Forbidden -- This pollutes the namespace.
using namespace foo;

// 禁止使用 -- 这会污染命名空间。
using namespace foo;
```

Corresponding cpplint.py check name: build/namespaces.  
对应的 cpplint.py 检查名称：build/namespaces。
