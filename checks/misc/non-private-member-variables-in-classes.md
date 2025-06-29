# misc-non-private-member-variables-in-classes

[cppcoreguidelines-non-private-member-variables-in-classes]{.title-ref}
redirects here as an alias for this check.

Finds classes that contain non-static data members in addition to
user-declared non-static member functions and diagnose all data members
declared with a non-`public` access specifier. The data members should
be declared as `private` and accessed through member functions instead
of exposed to derived classes or class consumers.

## Options

::: option
IgnoreClassesWithAllMemberVariablesBeingPublic

When [true]{.title-ref}, allows to completely ignore classes if **all**
the member variables in that class declared with a `public` access
specifier. Default is [false]{.title-ref}.
:::

::: option
IgnorePublicMemberVariables

When [true]{.title-ref}, allows to ignore (not diagnose) **all** the
member variables declared with a `public` access specifier. Default is
[false]{.title-ref}.
:::
