# misc-const-correctness

This check implements detection of local variables which could be
declared as `const` but are not. Declaring variables as `const` is
required or recommended by many coding guidelines, such as:
[ES.25](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#es25-declare-an-object-const-or-constexpr-unless-you-want-to-modify-its-value-later-on)
from the C++ Core Guidelines.

Please note that this check\'s analysis is type-based only. Variables
that are not modified but used to create a non-const handle that might
escape the scope are not diagnosed as potential `const`.

```c++
// Declare a variable, which is not ``const`` ...
int i = 42;
// but use it as read-only. This means that `i` can be declared ``const``.
int result = i * i;       // Before transformation
int const result = i * i; // After transformation
```

The check can analyze values, pointers and references and pointees:

```c++
// Normal values like built-ins or objects.
int potential_const_int = 42;       // Before transformation
int const potential_const_int = 42; // After transformation
int copy_of_value = potential_const_int;

MyClass could_be_const;       // Before transformation
MyClass const could_be_const; // After transformation
could_be_const.const_qualified_method();

// References can be declared const as well.
int &reference_value = potential_const_int;       // Before transformation
int const& reference_value = potential_const_int; // After transformation
int another_copy = reference_value;

// The similar semantics of pointers are analyzed.
int *pointer_variable = &potential_const_int; // Before transformation
int const*const pointer_variable = &potential_const_int; // After transformation, both pointer itself and pointee are supported.
int last_copy = *pointer_variable;
```

The automatic code transformation is only applied to variables that are
declared in single declarations. You may want to prepare your code base
with
`readability-isolate-declaration <../readability/isolate-declaration>`{.interpreted-text
role="doc"} first.

Note that there is the check
`cppcoreguidelines-avoid-non-const-global-variables <../cppcoreguidelines/avoid-non-const-global-variables>`{.interpreted-text
role="doc"} to enforce `const` correctness on all globals.

## Known Limitations

The check does not run on [C]{.title-ref} code.

The check will not analyze templated variables or variables that are
instantiation dependent. Different instantiations can result in
different `const` correctness properties and in general it is not
possible to find all instantiations of a template. The template might be
used differently in an independent translation unit.

## Options

::: option
AnalyzeValues

Enable or disable the analysis of ordinary value variables, like
`int i = 42;`. Default is [true]{.title-ref}.

```c++
// Warning
int i = 42;
// No warning
int const i = 42;

// Warning
int a[] = {42, 42, 42};
// No warning
int const a[] = {42, 42, 42};
```

:::

::: option
AnalyzeReferences

Enable or disable the analysis of reference variables, like
`int &ref = i;`. Default is [true]{.title-ref}.

```c++
int i = 42;
// Warning
int& ref = i;
// No warning
int const& ref = i;
```

:::

::: option
AnalyzePointers

Enable or disable the analysis of pointers variables, like
`int *ptr = &i;`. For specific checks, see
`WarnPointersAsValues`{.interpreted-text role="option"} and
`WarnPointersAsPointers`{.interpreted-text role="option"}. Default is
[true]{.title-ref}.
:::

::: option
WarnPointersAsValues

This option enables the suggestion for `const` of the pointer itself.
Pointer values have two possibilities to be `const`, the pointer and the
value pointing to. Default is [false]{.title-ref}.

```c++
int value = 42;

// Warning
const int * pointer_variable = &value;
// No warning
const int *const pointer_variable = &value;
```

:::

::: option
WarnPointersAsPointers

This option enables the suggestion for `const` of the value pointing to.
Default is [true]{.title-ref}.

Requires `AnalyzePointers`{.interpreted-text role="option"} to be
[true]{.title-ref}.

```c++
int value = 42;

// No warning
const int *const pointer_variable = &value;
// Warning
int *const pointer_variable = &value;
```

:::

::: option
TransformValues

Provides fixit-hints for value types that automatically add `const` if
its a single declaration. Default is [true]{.title-ref}.

```c++
// Before
int value = 42;
// After
int const value = 42;

// Before
int a[] = {42, 42, 42};
// After
int const a[] = {42, 42, 42};

// Result is modified later in its life-time. No diagnostic and fixit hint will be emitted.
int result = value * 3;
result -= 10;
```

:::

::: option
TransformReferences

Provides fixit-hints for reference types that automatically add `const`
if its a single declaration. Default is [true]{.title-ref}.

```c++
// This variable could still be a constant. But because there is a non-const reference to
// it, it can not be transformed (yet).
int value = 42;
// The reference 'ref_value' is not modified and can be made 'const int &ref_value = value;'
// Before
int &ref_value = value;
// After
int const &ref_value = value;

// Result is modified later in its life-time. No diagnostic and fixit hint will be emitted.
int result = ref_value * 3;
result -= 10;
```

:::

::: option
TransformPointersAsValues

Provides fixit-hints for pointers if their pointee is not changed. This
does not analyze if the value-pointed-to is unchanged! Default is
[false]{.title-ref}.

Requires \'WarnPointersAsValues\' to be \'true\'.

```c++
int value = 42;

// Before
const int * pointer_variable = &value;
// After
const int *const pointer_variable = &value;

// Before
const int * a[] = {&value, &value};
// After
const int *const a[] = {&value, &value};

// Before
int *ptr_value = &value;
// After
int *const ptr_value = &value;

int result = 100 * (*ptr_value); // Does not modify the pointer itself.
// This modification of the pointee is still allowed and not diagnosed.
*ptr_value = 0;

// The following pointer may not become a 'int *const'.
int *changing_pointee = &value;
changing_pointee = &result;
```

:::

::: option
TransformPointersAsPointers

Provides fix-it hints for pointers if the value it pointing to is not
changed. Default is [false]{.title-ref}.

Requires `WarnPointersAsPointers`{.interpreted-text role="option"} to be
[true]{.title-ref}.

```c++
int value = 42;

// Before
int * pointer_variable = &value;
// After
const int * pointer_variable = &value;

// Before
int * a[] = {&value, &value};
// After
const int * a[] = {&value, &value};
```

:::

::: option
AllowedTypes

A semicolon-separated list of names of types that will be excluded from
const-correctness checking. Regular expressions are accepted, e.g.
`[Rr]ef(erence)?$` matches every type with suffix `Ref`, `ref`,
`Reference` and `reference`. If a name in the list contains the sequence
[::]{.title-ref}, it is matched against the qualified type name (i.e.
`namespace::Type`), otherwise it is matched against only the type name
(i.e. `Type`). Default is empty string.
:::
