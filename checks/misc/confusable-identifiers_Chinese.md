# misc-confusable-identifiers

Warn about confusable identifiers, i.e. identifiers that are visually close to each other, but use different Unicode characters. This detects a potential attack described in [CVE-2021-42574](https://www.cve.org/CVERecord?id=CVE-2021-42574).

警告可能混淆的标识符，即那些在视觉上非常相似但使用了不同 Unicode 字符的标识符。这可以检测到 [CVE-2021-42574](https://www.cve.org/CVERecord?id=CVE-2021-42574) 中描述的潜在攻击。

Example:

示例：

```text
int fo; // Initial character is U+0066 (LATIN SMALL LETTER F).
int 𝐟o; // Initial character is U+1D41F (MATHEMATICAL BOLD SMALL F) not U+0066 (LATIN SMALL LETTER F).
```

```text
int fo; // 首字符是 U+0066（拉丁小写字母 F）。
int 𝐟o; // 首字符是 U+1D41F（数学粗体小写字母 F），而不是 U+0066（拉丁小写字母 F）。
```
