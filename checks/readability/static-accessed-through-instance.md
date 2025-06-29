# readability-static-accessed-through-instance

Checks for member expressions that access static members through
instances, and replaces them with uses of the appropriate qualified-id.

Example:

The following code:

```c++
struct C {
  static void foo();
  static int x;
  enum { E1 };
  enum E { E2 };
};

C *c1 = new C();
c1->foo();
c1->x;
c1->E1;
c1->E2;
```

is changed to:

```c++
C *c1 = new C();
C::foo();
C::x;
C::E1;
C::E2;
```

The [\--fix]{.title-ref} commandline option provides default support for
safe fixes, whereas [\--fix-notes]{.title-ref} enables fixes that may
replace expressions with side effects, potentially altering the
program\'s behavior.
