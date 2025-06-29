# llvm-twine-local

Looks for local Twine variables which are prone to use after frees and should be generally avoided.

查找容易在释放后被使用的局部 Twine 变量，这类用法通常应当避免。

```c++
static Twine Moo = Twine("bark") + "bah";

// becomes

static std::string Moo = (Twine("bark") + "bah").str();
```

```c++
static Twine Moo = Twine("bark") + "bah";

// 转换为

static std::string Moo = (Twine("bark") + "bah").str();
```
