# openmp-use-default-none

Finds OpenMP directives that are allowed to contain a default clause,  
but either don't specify it or the clause is specified but with the  
kind other than none, and suggests to use the default(none) clause.

查找允许包含 default 子句的 OpenMP 指令，  
但这些指令要么未指定该子句，要么指定了除 none 以外的类型，  
并建议使用 default(none) 子句。

Using default(none) clause forces developers to explicitly specify  
data sharing attributes for the variables referenced in the construct,  
thus making it obvious which variables are referenced, and what is their  
data sharing attribute, thus increasing readability and possibly making  
errors easier to spot.

使用 default(none) 子句会强制开发者显式指定构造中引用变量的数据共享属性，  
从而清晰地表明哪些变量被引用，以及它们的数据共享属性是什么，  
这有助于提高代码可读性，并可能更容易发现错误。

## Example

## 示例

```c++
// ``for`` directive cannot have ``default`` clause, no diagnostics.
void n0(const int a) {
#pragma omp for
  for (int b = 0; b < a; b++)
    ;
}

// ``for`` 指令不能包含 ``default`` 子句，因此不会触发诊断。
void n0(const int a) {
#pragma omp for
  for (int b = 0; b < a; b++)
    ;
}

// ``parallel`` directive.

// ``parallel`` directive can have ``default`` clause, but said clause is not
// specified, diagnosed.
void p0_0() {
#pragma omp parallel
  ;
  // WARNING: OpenMP directive ``parallel`` does not specify ``default``
  //          clause. Consider specifying ``default(none)`` clause.
}

// ``parallel`` 指令可以包含 ``default`` 子句，但未指定该子句，触发诊断。
void p0_0() {
#pragma omp parallel
  ;
  // 警告：OpenMP 指令 ``parallel`` 未指定 ``default`` 子句。
  //       建议指定 ``default(none)`` 子句。
}

// ``parallel`` directive can have ``default`` clause, and said clause is
// specified, with ``none`` kind, all good.
void p0_1() {
#pragma omp parallel default(none)
  ;
}

// ``parallel`` 指令可以包含 ``default`` 子句，且已指定为 ``none`` 类型，一切正常。
void p0_1() {
#pragma omp parallel default(none)
  ;
}

// ``parallel`` directive can have ``default`` clause, and said clause is
// specified, but with ``shared`` kind, which is not ``none``, diagnose.
void p0_2() {
#pragma omp parallel default(shared)
  ;
  // WARNING: OpenMP directive ``parallel`` specifies ``default(shared)``
  //          clause. Consider using ``default(none)`` clause instead.
}

// ``parallel`` 指令可以包含 ``default`` 子句，且已指定为 ``shared`` 类型，
// 但不是 ``none``，触发诊断。
void p0_2() {
#pragma omp parallel default(shared)
  ;
  // 警告：OpenMP 指令 ``parallel`` 指定了 ``default(shared)`` 子句。
  //       建议改用 ``default(none)`` 子句。
}

// ``parallel`` directive can have ``default`` clause, and said clause is
// specified, but with ``firstprivate`` kind, which is not ``none``, diagnose.
void p0_3() {
#pragma omp parallel default(firstprivate)
  ;
  // WARNING: OpenMP directive ``parallel`` specifies ``default(firstprivate)``
  //          clause. Consider using ``default(none)`` clause instead.
}

// ``parallel`` 指令可以包含 ``default`` 子句，且已指定为 ``firstprivate`` 类型，
// 但不是 ``none``，触发诊断。
void p0_3() {
#pragma omp parallel default(firstprivate)
  ;
  // 警告：OpenMP 指令 ``parallel`` 指定了 ``default(firstprivate)`` 子句。
  //       建议改用 ``default(none)`` 子句。
}
```
