# altera-unroll-loops

Finds inner loops that have not been unrolled, as well as fully unrolled  
loops with unknown loop bounds or a large number of iterations.

查找未展开的内部循环，以及循环边界未知或迭代次数较多的完全展开循环。

Unrolling inner loops could improve the performance of OpenCL kernels.  
However, if they have unknown loop bounds or a large number of  
iterations, they cannot be fully unrolled, and should be partially  
unrolled.

展开内部循环可以提升 OpenCL 内核的性能。  
然而，如果循环边界未知或迭代次数较多，则无法完全展开，应该进行部分展开。

Notes:

注意事项：

- This check is unable to determine the number of iterations in a  
  while or do..while loop; hence if such a loop is fully unrolled,  
  a note is emitted advising the user to partially unroll instead.

  此检查无法确定 while 或 do..while 循环的迭代次数；因此，如果此类循环被完全展开，将提示用户建议进行部分展开。

- In for loops, our check only works with simple arithmetic  
  increments ( +, -, \*, /). For all other increments, partial  
  unrolling is advised.

  对于 for 循环，此检查仅适用于简单的算术增量（+、-、\*、/）。对于其他类型的增量，建议进行部分展开。

- Depending on the exit condition, the calculations for determining if  
  the number of iterations is large may be off by 1. This should not  
  be an issue since the cut-off is generally arbitrary.

  根据退出条件的不同，判断迭代次数是否较多的计算可能会有 1 次的误差。由于阈值通常是人为设定的，这通常不是问题。

Based on the Altera SDK for OpenCL: Best Practices  
Guide.

基于《Altera OpenCL SDK 最佳实践指南》。

https://www.altera.com/en_US/pdfs/literature/hb/opencl-sdk/aocl_optimization_guide.pdf

```c++
for (int i = 0; i < 10; i++) {  // ok: outer loops should not be unrolled
                                // 正确：外部循环不应被展开
   int j = 0;
   do {  // warning: this inner do..while loop should be unrolled
         // 警告：此内部 do..while 循环应被展开
      j++;
   } while (j < 15);

   int k = 0;
   #pragma unroll
   while (k < 20) {  // ok: this inner loop is already unrolled
                     // 正确：此内部循环已被展开
      k++;
   }
}

int A[1000];
#pragma unroll
// warning: this loop is large and should be partially unrolled
// 警告：此循环较大，应进行部分展开
for (int a : A) {
   printf("%d", a);
}

#pragma unroll 5
// ok: this loop is large, but is partially unrolled
// 正确：此循环较大，但已进行部分展开
for (int a : A) {
   printf("%d", a);
}

#pragma unroll
// warning: this loop is large and should be partially unrolled
// 警告：此循环较大，应进行部分展开
for (int i = 0; i < 1000; ++i) {
   printf("%d", i);
}

#pragma unroll 5
// ok: this loop is large, but is partially unrolled
// 正确：此循环较大，但已进行部分展开
for (int i = 0; i < 1000; ++i) {
   printf("%d", i);
}

#pragma unroll
// warning: << operator not supported, recommend partial unrolling
// 警告：不支持 << 运算符，建议进行部分展开
for (int i = 0; i < 1000; i<<1) {
   printf("%d", i);
}

std::vector<int> someVector (100, 0);
int i = 0;
#pragma unroll
// note: loop may be large, recommend partial unrolling
// 注意：循环可能较大，建议进行部分展开
while (i < someVector.size()) {
   someVector[i]++;
}

#pragma unroll
// note: loop may be large, recommend partial unrolling
// 注意：循环可能较大，建议进行部分展开
while (true) {
   printf("In loop");
}

#pragma unroll 5
// ok: loop may be large, but is partially unrolled
// 正确：循环可能较大，但已进行部分展开
while (i < someVector.size()) {
   someVector[i]++;
}
```

## Options

## 选项

::: option  
MaxLoopIterations

Defines the maximum number of loop iterations that a fully unrolled loop  
can have. By default, it is set to 100.

定义完全展开的循环所允许的最大迭代次数。默认值为 100。

In practice, this refers to the integer value of the upper bound within  
the loop statement's condition expression.

在实际中，这指的是循环语句条件表达式中上限的整数值。  
:::
