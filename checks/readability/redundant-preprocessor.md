# readability-redundant-preprocessor

Finds potentially redundant preprocessor directives. At the moment the
following cases are detected:

- [#ifdef]{.title-ref} .. [#endif]{.title-ref} pairs which are nested
  inside an outer pair with the same condition. For example:

```c++
#ifdef FOO
#ifdef FOO // inner ifdef is considered redundant
void f();
#endif
#endif
```

- Same for [#ifndef]{.title-ref} .. [#endif]{.title-ref} pairs. For
  example:

```c++
#ifndef FOO
#ifndef FOO // inner ifndef is considered redundant
void f();
#endif
#endif
```

- [#ifndef]{.title-ref} inside an [#ifdef]{.title-ref} with the same
  condition:

```c++
#ifdef FOO
#ifndef FOO // inner ifndef is considered redundant
void f();
#endif
#endif
```

- [#ifdef]{.title-ref} inside an [#ifndef]{.title-ref} with the same
  condition:

```c++
#ifndef FOO
#ifdef FOO // inner ifdef is considered redundant
void f();
#endif
#endif
```

- [#if]{.title-ref} .. [#endif]{.title-ref} pairs which are nested
  inside an outer pair with the same condition. For example:

```c++
#define FOO 4
#if FOO == 4
#if FOO == 4 // inner if is considered redundant
void f();
#endif
#endif
```
