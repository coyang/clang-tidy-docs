# bugprone-unchecked-optional-access

_Note_: This check uses a flow-sensitive static analysis to produce its
results. Therefore, it may be more resource intensive (RAM, CPU) than
the average clang-tidy check.

This check identifies unsafe accesses to values contained in
`std::optional<T>`, `absl::optional<T>`, `base::Optional<T>`,
`folly::Optional<T>`, `bsl::optional`, or
`BloombergLP::bdlb::NullableValue` objects. Below we will refer to all
these types collectively as `optional<T>`.

An access to the value of an `optional<T>` occurs when one of its
`value`, `operator*`, or `operator->` member functions is invoked. To
align with common misconceptions, the check considers these member
functions as equivalent, even though there are subtle differences
related to exceptions versus undefined behavior. See _Additional notes_,
below, for more information on this topic.

An access to the value of an `optional<T>` is considered safe if and
only if code in the local scope (for example, a function body) ensures
that the `optional<T>` has a value in all possible execution paths that
can reach the access. That should happen either through an explicit
check, using the `optional<T>::has_value` member function, or by
constructing the `optional<T>` in a way that shows that it unambiguously
holds a value (e.g using `std::make_optional` which always returns a
populated `std::optional<T>`).

Below we list some examples, starting with unsafe optional access
patterns, followed by safe access patterns.

## Unsafe access patterns

### Access the value without checking if it exists

The check flags accesses to the value that are not locally guarded by
existence check:

```c++
void f(std::optional<int> opt) {
  use(*opt); // unsafe: it is unclear whether `opt` has a value.
}
```

### Access the value in the wrong branch

The check is aware of the state of an optional object in different
branches of the code. For example:

```c++
void f(std::optional<int> opt) {
  if (opt.has_value()) {
  } else {
    use(opt.value()); // unsafe: it is clear that `opt` does *not* have a value.
  }
}
```

### Assume a function result to be stable

The check is aware that function results might not be stable. That is,
consecutive calls to the same function might return different values.
For example:

```c++
void f(Foo foo) {
  if (foo.take().has_value()) {
    use(*foo.take()); // unsafe: it is unclear whether `foo.take()` has a value.
  }
}
```

#### Exception: accessor methods

The check assumes _accessor_ methods of a class are stable, with a
heuristic to determine which methods are accessors. Specifically,
parameter-free `const` methods and smart pointer-like APIs (non `const`
overloads of `*` when there is a parallel `const` overload) are treated
as accessors. Note that this is not guaranteed to be safe \-- but, it is
widely used (safely) in practice. Calls to non `const` methods are
assumed to modify the state of the object and affect the stability of
earlier accessor calls.

### Rely on invariants of uncommon APIs

The check is unaware of invariants of uncommon APIs. For example:

```c++
void f(Foo foo) {
  if (foo.HasProperty("bar")) {
    use(*foo.GetProperty("bar")); // unsafe: it is unclear whether `foo.GetProperty("bar")` has a value.
  }
}
```

### Check if a value exists, then pass the optional to another function

The check relies on local reasoning. The check and value access must
both happen in the same function. An access is considered unsafe even if
the caller of the function performing the access ensures that the
optional has a value. For example:

```c++
void g(std::optional<int> opt) {
  use(*opt); // unsafe: it is unclear whether `opt` has a value.
}

void f(std::optional<int> opt) {
  if (opt.has_value()) {
    g(opt);
  }
}
```

## Safe access patterns

### Check if a value exists, then access the value

The check recognizes all straightforward ways for checking if a value
exists and accessing the value contained in an optional object. For
example:

```c++
void f(std::optional<int> opt) {
  if (opt.has_value()) {
    use(*opt);
  }
}
```

### Check if a value exists, then access the value from a copy

The criteria that the check uses is semantic, not syntactic. It
recognizes when a copy of the optional object being accessed is known to
have a value. For example:

```c++
void f(std::optional<int> opt1) {
  if (opt1.has_value()) {
    std::optional<int> opt2 = opt1;
    use(*opt2);
  }
}
```

### Ensure that a value exists using common macros

The check is aware of common macros like `CHECK` and `DCHECK`. Those can
be used to ensure that an optional object has a value. For example:

```c++
void f(std::optional<int> opt) {
  DCHECK(opt.has_value());
  use(*opt);
}
```

### Ensure that a value exists, then access the value in a correlated branch

The check is aware of correlated branches in the code and can figure out
when an optional object is ensured to have a value on all execution
paths that lead to an access. For example:

```c++
void f(std::optional<int> opt) {
  bool safe = false;
  if (opt.has_value() && SomeOtherCondition()) {
    safe = true;
  }
  // ... more code...
  if (safe) {
    use(*opt);
  }
}
```

## Stabilize function results

Function results are not assumed to be stable across calls, except for
const accessor methods. For more complex accessors (non-const, or depend
on multiple params) it is best to store the result of the function call
in a local variable and use that variable to access the value. For
example:

```c++
void f(Foo foo) {
  if (const auto& foo_opt = foo.take(); foo_opt.has_value()) {
    use(*foo_opt);
  }
}
```

## Do not rely on uncommon-API invariants

When uncommon APIs guarantee that an optional has contents, do not rely
on it \--instead, check explicitly that the optional object has a value.
For example:

```c++
void f(Foo foo) {
  if (const auto& property = foo.GetProperty("bar")) {
    use(*property);
  }
}
```

instead of the [HasProperty]{.title-ref}, [GetProperty]{.title-ref}
pairing we saw above.

## Do not rely on caller-performed checks

If you know that all of a function\'s callers have checked that an
optional argument has a value, either change the function to take the
value directly or check the optional again in the local scope of the
callee. For example:

```c++
void g(int val) {
  use(val);
}

void f(std::optional<int> opt) {
  if (opt.has_value()) {
    g(*opt);
  }
}
```

and

```c++
struct S {
  std::optional<int> opt;
  int x;
};

void g(const S &s) {
  if (s.opt.has_value() && s.x > 10) {
    use(*s.opt);
}

void f(S s) {
  if (s.opt.has_value()) {
    g(s);
  }
}
```

## Additional notes

### Aliases created via `using` declarations

The check is aware of aliases of optional types that are created via
`using` declarations. For example:

```c++
using OptionalInt = std::optional<int>;

void f(OptionalInt opt) {
  use(opt.value()); // unsafe: it is unclear whether `opt` has a value.
}
```

### Lambdas

The check does not currently report unsafe optional accesses in lambdas.
A future version will expand the scope to lambdas, following the rules
outlined above. It is best to follow the same principles when using
optionals in lambdas.

### Access with `operator*()` vs. `value()`

Given that `value()` has well-defined behavior (either throwing an
exception or terminating the program), why treat it the same as
`operator*()` which causes undefined behavior (UB)? That is, why is it
considered unsafe to access an optional with `value()`, if it\'s not
provably populated with a value? For that matter, why is `CHECK()`
followed by `operator*()` any better than `value()`, given that they are
semantically equivalent (on configurations that disable exceptions)?

The answer is that we assume most users do not realize the difference
between `value()` and `operator*()`. Shifting to `operator*()` and some
form of explicit value-presence check or explicit program termination
has two advantages:

> - Readability. The check, and any potential side effects like
>   program shutdown, are very clear in the code. Separating access
>   from checks can actually make the checks more obvious.
> - Performance. A single check can cover many or even all accesses
>   within scope. This gives the user the best of both worlds \-- the
>   safety of a dynamic check, but without incurring redundant costs.
