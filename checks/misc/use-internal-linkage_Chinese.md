以下是翻译后的 markdown 文件内容，保留了中英文双语，中文在英文下方，并以一个空行分隔：

# misc-use-internal-linkage

Detects variables and functions that can be marked as static or moved into an anonymous namespace to enforce internal linkage.

检测可以标记为 static 或移动到匿名命名空间中的变量和函数，以强制使用内部链接。

Static functions and variables are scoped to a single file. Marking functions and variables as static helps to better remove dead code. In addition, it gives the compiler more information and allows for more aggressive optimizations.

静态函数和变量的作用域仅限于单个文件。将函数和变量标记为 static 有助于更好地移除无用代码。此外，它还能为编译器提供更多信息，从而实现更激进的优化。

Example:

示例：

```c++
int v1; // can be marked as static
        // 可以标记为 static

void fn1() {} // can be marked as static
              // 可以标记为 static

namespace {
  // already in anonymous namespace
  // 已经在匿名命名空间中
  int v2;
  void fn2();
}
// already declared as extern
// 已经声明为 extern
extern int v2;

void fn3(); // without function body in all declaration, maybe external linkage
            // 所有声明中都没有函数体，可能具有外部链接
void fn3();

// export declarations
// 导出声明
export void fn4() {}
export namespace t { void fn5() {} }
export int v2;
```

## Options

## 选项

::: option
FixMode

Selects what kind of a fix the check should provide. The default is [UseStatic]{.title-ref}.

选择该检查应提供哪种类型的修复。默认值为 [UseStatic]{.title-ref}。

None

: Don’t fix automatically.

：不自动修复。

UseStatic

: Add static for internal linkage variable and function.

：为具有内部链接的变量和函数添加 static。
:::
