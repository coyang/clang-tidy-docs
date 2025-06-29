# portability-simd-intrinsics

Finds SIMD intrinsics calls and suggests `std::experimental::simd`
([P0214](https://wg21.link/p0214)) alternatives.

If the option `Suggest`{.interpreted-text role="option"} is set to
[true]{.title-ref}, for

```c++
_mm_add_epi32(a, b); // x86
vec_add(a, b);       // Power
```

the check suggests an alternative: `operator+` on
`std::experimental::simd` objects.

Otherwise, it just complains the intrinsics are non-portable (and there
are [P0214](https://wg21.link/p0214) alternatives).

Many architectures provide SIMD operations (e.g. x86 SSE/AVX, Power
AltiVec/VSX, ARM NEON). It is common that SIMD code implementing the
same algorithm, is written in multiple target-dispatching pieces to
optimize for different architectures or micro-architectures.

The C++ standard proposal [P0214](https://wg21.link/p0214) and its
extensions cover many common SIMD operations. By migrating from
target-dependent intrinsics to [P0214](https://wg21.link/p0214)
operations, the SIMD code can be simplified and pieces for different
targets can be unified.

Refer to [P0214](https://wg21.link/p0214) for introduction and
motivation for the data-parallel standard library.

## Options

::: option
Suggest

If this option is set to [true]{.title-ref} (default is
[false]{.title-ref}), the check will suggest
[P0214](https://wg21.link/p0214) alternatives, otherwise it only points
out the intrinsic function is non-portable.
:::

::: option
Std

The namespace used to suggest [P0214](https://wg21.link/p0214)
alternatives. If not specified, [std::]{.title-ref} for
[-std=c++20]{.title-ref} and [std::experimental::]{.title-ref} for
[-std=c++11]{.title-ref}.
:::
