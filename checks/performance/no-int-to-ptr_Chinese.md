# performance-no-int-to-ptr

Diagnoses every integer to pointer cast.

诊断所有从整数到指针的类型转换。

While casting an (integral) pointer to an integer is obvious - you just get the integral value of the pointer, casting an integer to an (integral) pointer is deceivingly different. While you will get a pointer with that integral value, if you got that integral value via a pointer-to-integer cast originally, the new pointer will lack the provenance information from the original pointer.

虽然将（整数类型的）指针转换为整数是显而易见的 —— 你只是获取了指针的整数值，但将一个整数转换为（整数类型的）指针却有着看似相似却本质不同的行为。如果你最初是通过指针到整数的转换获得该整数值的，那么新生成的指针将缺失原始指针的来源信息（provenance）。

So while (integral) pointer to integer casts are effectively no-ops, and are transparent to the optimizer, integer to (integral) pointer casts are NOT transparent, and may conceal information from optimizer.

因此，（整数类型的）指针到整数的转换实际上是无操作的，对优化器是透明的；而整数到（整数类型的）指针的转换则不是透明的，可能会对优化器隐藏信息。

While that may be the intention, it is not always so. For example, let's take a look at a routine to align the pointer up to the multiple of 16: The obvious, naive implementation for that is:

虽然有时这种行为是有意为之，但并不总是如此。例如，我们来看一个将指针对齐到 16 的倍数的例程：一个显而易见但天真的实现如下：

```c++
char* src(char* maybe_underbiased_ptr) {
  uintptr_t maybe_underbiased_intptr = (uintptr_t)maybe_underbiased_ptr;
  uintptr_t aligned_biased_intptr = maybe_underbiased_intptr + 15;
  uintptr_t aligned_intptr = aligned_biased_intptr & (~15);
  return (char*)aligned_intptr; // warning: avoid integer to pointer casts [performance-no-int-to-ptr]
}
```

The check will rightfully diagnose that cast.

该检查器会正确地诊断出这处类型转换的问题。

But when provenance concealment is not the goal of the code, but an accident, this example can be rewritten as follows, without using integer to pointer cast:

但如果代码的目的并非故意隐藏指针来源信息，而是无意为之，那么该示例可以改写如下，避免使用整数到指针的类型转换：

```c++
char*
tgt(char* maybe_underbiased_ptr) {
    uintptr_t maybe_underbiased_intptr = (uintptr_t)maybe_underbiased_ptr;
    uintptr_t aligned_biased_intptr = maybe_underbiased_intptr + 15;
    uintptr_t aligned_intptr = aligned_biased_intptr & (~15);
    uintptr_t bias = aligned_intptr - maybe_underbiased_intptr;
    return maybe_underbiased_ptr + bias;
}
```
