# hicpp-explicit-conversions

This check is an alias for
`google-explicit-constructor <../google/explicit-constructor>`{.interpreted-text
role="doc"}. Used to enforce parts of [rule
5.4.1](https://www.perforce.com/resources/qac/high-integrity-cpp-coding-standard-expressions).
This check will enforce that constructors and conversion operators are
marked [explicit]{.title-ref}. Other forms of casting checks are
implemented in other places. The following checks can be used to check
for more forms of casting:

- `cppcoreguidelines-pro-type-static-cast-downcast <../cppcoreguidelines/pro-type-static-cast-downcast>`{.interpreted-text
  role="doc"}
- `cppcoreguidelines-pro-type-reinterpret-cast <../cppcoreguidelines/pro-type-reinterpret-cast>`{.interpreted-text
  role="doc"}
- `cppcoreguidelines-pro-type-const-cast <../cppcoreguidelines/pro-type-const-cast>`{.interpreted-text
  role="doc"}
- `cppcoreguidelines-pro-type-cstyle-cast <../cppcoreguidelines/pro-type-cstyle-cast>`{.interpreted-text
  role="doc"}
