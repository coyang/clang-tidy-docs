以下是翻译后的 markdown 文件内容，保留了中英文双语格式，中文在英文下方，并以一个空行分隔：

# readability-uppercase-literal-suffix

# readability-uppercase-literal-suffix

[cert-dcl16-c]{.title-ref} redirects here as an alias for this check. By default, only the suffixes that begin with l (l, ll, lu, llu, but not u, ul, ull) are diagnosed by that alias.

[cert-dcl16-c]{.title-ref} 是此检查项的别名。默认情况下，只有以 l 开头的后缀（如 l、ll、lu、llu，而不是 u、ul、ull）会被该别名诊断。

[hicpp-uppercase-literal-suffix]{.title-ref} redirects here as an alias for this check.

[hicpp-uppercase-literal-suffix]{.title-ref} 也是此检查项的别名。

Detects when the integral literal or floating point (decimal or hexadecimal) literal has a non-uppercase suffix and provides a fix-it hint with the uppercase suffix.

检测整数字面量或浮点字面量（十进制或十六进制）使用了非大写的后缀，并提供使用大写后缀的修复建议。

All valid combinations of suffixes are supported.

支持所有合法的后缀组合。

```c
auto x = 1;  // OK, no suffix.

auto x = 1u; // warning: integer literal suffix 'u' is not upper-case

auto x = 1U; // OK, suffix is uppercase.

...
```

```c
auto x = 1;  // 正确，无后缀。

auto x = 1u; // 警告：整数字面量后缀 'u' 不是大写

auto x = 1U; // 正确，后缀为大写。

...
```

## Options

## 选项

::: option
NewSuffixes

Optionally, a list of the destination suffixes can be provided. When the suffix is found, a case-insensitive lookup in that list is made, and if a replacement is found that is different from the current suffix, then the diagnostic is issued. This allows for fine-grained control of what suffixes to consider and what their replacements should be.
:::

::: option
NewSuffixes

可选地，可以提供一个目标后缀列表。当发现某个后缀时，会在该列表中进行不区分大小写的查找，如果找到的替换项与当前后缀不同，则会发出诊断信息。这允许对需要考虑的后缀及其替换方式进行精细控制。
:::

### Example

### 示例

Given a list L;uL:

给定列表 L;uL：

- l -> L
- l -> L

- L will be kept as is.
- L 保持不变。

- ul -> uL
- ul -> uL

- Ul -> uL
- Ul -> uL

- UL -> uL
- UL -> uL

- uL will be kept as is.
- uL 保持不变。

- ull will be kept as is, since it is not in the list
- ull 不在列表中，因此保持不变。

- and so on.
- 依此类推。

::: option
IgnoreMacros

If this option is set to true (default is true), the check will not warn about literal suffixes inside macros.
:::

::: option
IgnoreMacros

如果此选项设置为 true（默认值为 true），则不会对宏中的字面量后缀发出警告。
:::
