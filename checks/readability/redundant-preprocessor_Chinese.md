# readability-redundant-preprocessor

Finds potentially redundant preprocessor directives. At the moment the following cases are detected:

查找可能冗余的预处理指令。目前可以检测以下几种情况：

- #ifdef .. #endif pairs which are nested inside an outer pair with the same condition. For example:

- 嵌套在具有相同条件的外层 #ifdef .. #endif 对中的 #ifdef .. #endif 对。例如：

```c++
#ifdef FOO
#ifdef FOO // inner ifdef is considered redundant
void f();
#endif
#endif
```

```c++
#ifdef FOO
#ifdef FOO // 内层 ifdef 被认为是冗余的
void f();
#endif
#endif
```

- Same for #ifndef .. #endif pairs. For example:

- 同样适用于 #ifndef .. #endif 对。例如：

```c++
#ifndef FOO
#ifndef FOO // inner ifndef is considered redundant
void f();
#endif
#endif
```

```c++
#ifndef FOO
#ifndef FOO // 内层 ifndef 被认为是冗余的
void f();
#endif
#endif
```

- #ifndef inside an #ifdef with the same condition:

- 在具有相同条件的 #ifdef 中嵌套 #ifndef：

```c++
#ifdef FOO
#ifndef FOO // inner ifndef is considered redundant
void f();
#endif
#endif
```

```c++
#ifdef FOO
#ifndef FOO // 内层 ifndef 被认为是冗余的
void f();
#endif
#endif
```

- #ifdef inside an #ifndef with the same condition:

- 在具有相同条件的 #ifndef 中嵌套 #ifdef：

```c++
#ifndef FOO
#ifdef FOO // inner ifdef is considered redundant
void f();
#endif
#endif
```

```c++
#ifndef FOO
#ifdef FOO // 内层 ifdef 被认为是冗余的
void f();
#endif
#endif
```

- #if .. #endif pairs which are nested inside an outer pair with the same condition. For example:

- 嵌套在具有相同条件的外层 #if .. #endif 对中的 #if .. #endif 对。例如：

```c++
#define FOO 4
#if FOO == 4
#if FOO == 4 // inner if is considered redundant
void f();
#endif
#endif
```

```c++
#define FOO 4
#if FOO == 4
#if FOO == 4 // 内层 if 被认为是冗余的
void f();
#endif
#endif
```
