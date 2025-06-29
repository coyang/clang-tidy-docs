# bugprone-tagged-union-member-count

Gives warnings for tagged unions, where the number of tags is different
from the number of data members inside the union.

A struct or a class is considered to be a tagged union if it has exactly
one union data member and exactly one enum data member and any number of
other data members that are neither unions or enums.

Example:

```c++
enum Tags {
  Tag1,
  Tag2,
};

struct TaggedUnion { // warning: tagged union has more data members (3) than tags (2)
  enum Tags Kind;
  union {
    int I;
    float F;
    char *Str;
  } Data;
};
```

## How enum constants are counted

The main complicating factor when counting the number of enum constants
is that some of them might be auxiliary values that purposefully don\'t
have a corresponding union data member and are used for something else.
For example the last enum constant sometimes explicitly \"points to\"
the last declared valid enum constant or tracks how many enum constants
have been declared.

For an illustration:

```c++
enum TagWithLast {
  Tag1 = 0,
  Tag2 = 1,
  Tag3 = 2,
  LastTag = 2
};

enum TagWithCounter {
  Tag1, // is 0
  Tag2, // is 1
  Tag3, // is 2
  TagCount, // is 3
};
```

The check counts the number of distinct values among the enum constants
and not the enum constants themselves. This way the enum constants that
are essentially just aliases of other enum constants are not included in
the final count.

Handling of counting enum constants (ones like `TagCount` in the
previous code example) is done by decreasing the number of enum values
by one if the name of the last enum constant starts with a prefix or
ends with a suffix specified in `CountingEnumPrefixes`{.interpreted-text
role="option"}, `CountingEnumSuffixes`{.interpreted-text role="option"}
and it\'s value is one less than the total number of distinct values in
the enum.

When the final count is adjusted based on this heuristic then a
diagnostic note is emitted that shows which enum constant matched the
criteria.

The heuristic can be disabled entirely
(`EnableCountingEnumHeuristic`{.interpreted-text role="option"}) or
configured to follow your naming convention
(`CountingEnumPrefixes`{.interpreted-text role="option"},
`CountingEnumSuffixes`{.interpreted-text role="option"}). The strings
specified in `CountingEnumPrefixes`{.interpreted-text role="option"},
`CountingEnumSuffixes`{.interpreted-text role="option"} are matched case
insensitively.

Example counts:

```c++
// Enum count is 3, because the value 2 is counted only once
enum TagWithLast {
  Tag1 = 0,
  Tag2 = 1,
  Tag3 = 2,
  LastTag = 2
};

// Enum count is 3, because TagCount is heuristically excluded
enum TagWithCounter {
  Tag1, // is 0
  Tag2, // is 1
  Tag3, // is 2
  TagCount, // is 3
};
```

## Options

::: option
EnableCountingEnumHeuristic
:::

This option enables or disables the counting enum heuristic. It uses the
prefixes and suffixes specified in the options
`CountingEnumPrefixes`{.interpreted-text role="option"},
`CountingEnumSuffixes`{.interpreted-text role="option"} to find counting
enum constants by using them for prefix and suffix matching.

This option is enabled by default.

When `EnableCountingEnumHeuristic`{.interpreted-text role="option"} is
\`false\`:

```c++
enum TagWithCounter {
  Tag1,
  Tag2,
  Tag3,
  TagCount,
};

struct TaggedUnion {
  TagWithCounter Kind;
  union {
    int A;
    long B;
    char *Str;
    float F;
  } Data;
};
```

When `EnableCountingEnumHeuristic`{.interpreted-text role="option"} is
\`true\`:

```c++
enum TagWithCounter {
  Tag1,
  Tag2,
  Tag3,
  TagCount,
};

struct TaggedUnion { // warning: tagged union has more data members (4) than tags (3)
  TagWithCounter Kind;
  union {
    int A;
    long B;
    char *Str;
    float F;
  } Data;
};
```

::: option
CountingEnumPrefixes
:::

See `CountingEnumSuffixes`{.interpreted-text role="option"} below.

::: option
CountingEnumSuffixes
:::

CountingEnumPrefixes and CountingEnumSuffixes are lists of semicolon
separated strings that are used to search for possible counting enum
constants. These strings are matched case insensitively as prefixes and
suffixes respectively on the names of the enum constants. If
`EnableCountingEnumHeuristic`{.interpreted-text role="option"} is
[false]{.title-ref} then these options do nothing.

The default value of `CountingEnumSuffixes`{.interpreted-text
role="option"} is [count]{.title-ref} and of
`CountingEnumPrefixes`{.interpreted-text role="option"} is the empty
string.

When `EnableCountingEnumHeuristic`{.interpreted-text role="option"} is
[true]{.title-ref} and `CountingEnumSuffixes`{.interpreted-text
role="option"} is \`count;size\`:

```c++
enum TagWithCounterCount {
  Tag1,
  Tag2,
  Tag3,
  TagCount,
};

struct TaggedUnionCount { // warning: tagged union has more data members (4) than tags (3)
  TagWithCounterCount Kind;
  union {
    int A;
    long B;
    char *Str;
    float F;
  } Data;
};

enum TagWithCounterSize {
  Tag11,
  Tag22,
  Tag33,
  TagSize,
};

struct TaggedUnionSize { // warning: tagged union has more data members (4) than tags (3)
  TagWithCounterSize Kind;
  union {
    int A;
    long B;
    char *Str;
    float F;
  } Data;
};
```

When `EnableCountingEnumHeuristic`{.interpreted-text role="option"} is
[true]{.title-ref} and `CountingEnumPrefixes`{.interpreted-text
role="option"} is [maxsize;last\_]{.title-ref}

```c++
enum TagWithCounterLast {
  Tag1,
  Tag2,
  Tag3,
  last_tag,
};

struct TaggedUnionLast { // warning: tagged union has more data members (4) than tags (3)
  TagWithCounterLast tag;
  union {
    int I;
    short S;
    char *C;
    float F;
  } Data;
};

enum TagWithCounterMaxSize {
  Tag1,
  Tag2,
  Tag3,
  MaxSizeTag,
};

struct TaggedUnionMaxSize { // warning: tagged union has more data members (4) than tags (3)
  TagWithCounterMaxSize tag;
  union {
    int I;
    short S;
    char *C;
    float F;
  } Data;
};
```

::: option
StrictMode
:::

When enabled, the check will also give a warning, when the number of
tags is greater than the number of union data members.

This option is disabled by default.

When `StrictMode`{.interpreted-text role="option"} is \`false\`:

```c++
struct TaggedUnion {
  enum {
    Tag1,
    Tag2,
    Tag3,
  } Tags;
  union {
    int I;
    float F;
  } Data;
};
```

When `StrictMode`{.interpreted-text role="option"} is \`true\`:

```c++
struct TaggedUnion { // warning: tagged union has fewer data members (2) than tags (3)
  enum {
    Tag1,
    Tag2,
    Tag3,
  } Tags;
  union {
    int I;
    float F;
  } Data;
};
```
