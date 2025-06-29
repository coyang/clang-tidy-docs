# abseil-faster-strsplit-delimiter

Finds instances of absl::StrSplit() or absl::MaxSplits() where the delimiter is a single character string literal and replaces with a character. The check will offer a suggestion to change the string literal into a character. It will also catch code using absl::ByAnyChar() for just a single character and will transform that into a single character as well.

查找 absl::StrSplit() 或 absl::MaxSplits() 中使用单字符字符串字面量作为分隔符的情况，并将其替换为字符。该检查会建议将字符串字面量更改为字符。同时，它还会检测仅使用单个字符的 absl::ByAnyChar() 调用，并将其转换为单个字符。

These changes will give the same result, but using characters rather than single character string literals is more efficient and readable.

这些更改将产生相同的结果，但使用字符而不是单字符字符串字面量更高效且更易读。

Examples:

示例：

```c++
// Original - the argument is a string literal.
// 原始代码 - 参数是一个字符串字面量。
for (auto piece : absl::StrSplit(str, "B")) {

// Suggested - the argument is a character, which causes the more efficient
// overload of absl::StrSplit() to be used.
// 建议修改 - 参数是一个字符，这将使用 absl::StrSplit() 更高效的重载版本。
for (auto piece : absl::StrSplit(str, 'B')) {


// Original - the argument is a string literal inside absl::ByAnyChar call.
// 原始代码 - 参数是在 absl::ByAnyChar 调用中的字符串字面量。
for (auto piece : absl::StrSplit(str, absl::ByAnyChar("B"))) {

// Suggested - the argument is a character, which causes the more efficient
// overload of absl::StrSplit() to be used and we do not need absl::ByAnyChar
// anymore.
// 建议修改 - 参数是一个字符，这将使用 absl::StrSplit() 更高效的重载版本，
// 并且我们不再需要使用 absl::ByAnyChar。
for (auto piece : absl::StrSplit(str, 'B')) {


// Original - the argument is a string literal inside absl::MaxSplits call.
// 原始代码 - 参数是在 absl::MaxSplits 调用中的字符串字面量。
for (auto piece : absl::StrSplit(str, absl::MaxSplits("B", 1))) {

// Suggested - the argument is a character, which causes the more efficient
// overload of absl::StrSplit() to be used.
// 建议修改 - 参数是一个字符，这将使用 absl::StrSplit() 更高效的重载版本。
for (auto piece : absl::StrSplit(str, absl::MaxSplits('B', 1))) {
```
