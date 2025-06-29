# bugprone-tagged-union-member-count

Gives warnings for tagged unions, where the number of tags is different from the number of data members inside the union.

对标记联合（tagged union）发出警告，当标签（tag）的数量与联合体（union）中的数据成员数量不一致时。

A struct or a class is considered to be a tagged union if it has exactly one union data member and exactly one enum data member and any number of other data members that are neither unions or enums.

当一个结构体或类中恰好包含一个 union 数据成员和一个 enum 数据成员，并且包含任意数量的其他非 union 和非 enum 的数据成员时，它被视为一个标记联合。

Example:

示例：

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

枚举常量是如何计数的

The main complicating factor when counting the number of enum constants is that some of them might be auxiliary values that purposefully don't have a corresponding union data member and are used for something else. For example the last enum constant sometimes explicitly "points to" the last declared valid enum constant or tracks how many enum constants have been declared.

在统计枚举常量数量时的主要复杂因素是，其中一些可能是辅助值，它们有意不对应任何 union 数据成员，而是用于其他目的。例如，最后一个枚举常量有时会显式“指向”最后一个有效声明的枚举常量，或者用于记录已声明的枚举常量数量。

For an illustration:

示例说明：

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

The check counts the number of distinct values among the enum constants and not the enum constants themselves. This way the enum constants that are essentially just aliases of other enum constants are not included in the final count.

该检查统计的是枚举常量中不同值的数量，而不是枚举常量本身的数量。这样，本质上只是其他枚举常量别名的枚举常量不会被计入最终数量。

Handling of counting enum constants (ones like TagCount in the previous code example) is done by decreasing the number of enum values by one if the name of the last enum constant starts with a prefix or ends with a suffix specified in CountingEnumPrefixes、CountingEnumSuffixes，并且其值比枚举中不同值的总数少一。

对于像上例中的 TagCount 这样的枚举常量，处理方式是：如果最后一个枚举常量的名称以 CountingEnumPrefixes 中指定的前缀开头，或以 CountingEnumSuffixes 中指定的后缀结尾，并且其值比枚举中不同值的总数少一，则将枚举值数量减一。

When the final count is adjusted based on this heuristic then a diagnostic note is emitted that shows which enum constant matched the criteria.

当根据此启发式规则调整最终计数时，会发出诊断注释，指出哪个枚举常量符合该规则。

The heuristic can be disabled entirely (EnableCountingEnumHeuristic) or configured to follow your naming convention (CountingEnumPrefixes, CountingEnumSuffixes). The strings specified in CountingEnumPrefixes, CountingEnumSuffixes are matched case insensitively.

该启发式规则可以完全禁用（通过 EnableCountingEnumHeuristic），也可以配置为遵循你的命名约定（通过 CountingEnumPrefixes 和 CountingEnumSuffixes）。在匹配时，CountingEnumPrefixes 和 CountingEnumSuffixes 中指定的字符串不区分大小写。

Example counts:

示例计数：

```c++
// Enum count is 3, because the value 2 is counted only once
// 枚举计数为 3，因为值 2 只计算一次
enum TagWithLast {
  Tag1 = 0,
  Tag2 = 1,
  Tag3 = 2,
  LastTag = 2
};

// Enum count is 3, because TagCount is heuristically excluded
// 枚举计数为 3，因为 TagCount 被启发式规则排除
enum TagWithCounter {
  Tag1, // is 0
  Tag2, // is 1
  Tag3, // is 2
  TagCount, // is 3
};
```

## Options

选项

::: option
EnableCountingEnumHeuristic
:::

This option enables or disables the counting enum heuristic. It uses the prefixes and suffixes specified in the options CountingEnumPrefixes, CountingEnumSuffixes to find counting enum constants by using them for prefix and suffix matching.

此选项用于启用或禁用枚举计数的启发式规则。它使用 CountingEnumPrefixes 和 CountingEnumSuffixes 中指定的前缀和后缀，通过前缀和后缀匹配来识别计数用的枚举常量。

This option is enabled by default.

该选项默认启用。

When EnableCountingEnumHeuristic is false:

当 EnableCountingEnumHeuristic 为 false 时：

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

When EnableCountingEnumHeuristic is true:

当 EnableCountingEnumHeuristic 为 true 时：

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

See CountingEnumSuffixes below.

参见下方的 CountingEnumSuffixes。

::: option
CountingEnumSuffixes
:::

CountingEnumPrefixes and CountingEnumSuffixes are lists of semicolon separated strings that are used to search for possible counting enum constants. These strings are matched case insensitively as prefixes and suffixes respectively on the names of the enum constants. If EnableCountingEnumHeuristic is false then these options do nothing.

CountingEnumPrefixes 和 CountingEnumSuffixes 是由分号分隔的字符串列表，用于查找可能是计数用途的枚举常量。这些字符串分别作为前缀和后缀在枚举常量名称中进行不区分大小写的匹配。如果 EnableCountingEnumHeuristic 为 false，则这些选项无效。

The default value of CountingEnumSuffixes is count and of CountingEnumPrefixes is the empty string.

CountingEnumSuffixes 的默认值是 count，CountingEnumPrefixes 的默认值是空字符串。

When EnableCountingEnumHeuristic is true and CountingEnumSuffixes is count;size:

当 EnableCountingEnumHeuristic 为 true 且 CountingEnumSuffixes 为 count;size 时：

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

When EnableCountingEnumHeuristic is true and CountingEnumPrefixes is maxsize;last\_

当 EnableCountingEnumHeuristic 为 true 且 CountingEnumPrefixes 为 maxsize;last\_ 时：

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

When enabled, the check will also give a warning, when the number of tags is greater than the number of union data members.

启用该选项后，当标签数量大于 union 数据成员数量时，也会发出警告。

This option is disabled by default.

该选项默认禁用。

When StrictMode is false:

当 StrictMode 为 false 时：

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

When StrictMode is true:

当 StrictMode 为 true 时：

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
