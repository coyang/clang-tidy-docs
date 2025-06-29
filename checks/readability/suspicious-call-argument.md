# readability-suspicious-call-argument

Finds function calls where the arguments passed are provided out of
order, based on the difference between the argument name and the
parameter names of the function.

Given a function call `f(foo, bar);` and a function signature
`void f(T tvar, U uvar)`, the arguments `foo` and `bar` are swapped if
`foo` (the argument name) is more similar to `uvar` (the other
parameter) than `tvar` (the parameter it is currently passed to) **and**
`bar` is more similar to `tvar` than `uvar`.

Warnings might indicate either that the arguments are swapped, or that
the names\' cross-similarity might hinder code comprehension.

## Heuristics

The following heuristics are implemented in the check. If **any** of the
enabled heuristics deem the arguments to be provided out of order, a
warning will be issued.

The heuristics themselves are implemented by considering pairs of
strings, and are symmetric, so in the following there is no distinction
on which string is the argument name and which string is the parameter
name.

### Equality

The most trivial heuristic, which compares the two strings for
case-insensitive equality.

### Abbreviation {#abbreviation_heuristic}

Common abbreviations can be specified which will deem the strings
similar if the abbreviated and the abbreviation stand together. For
example, if `src` is registered as an abbreviation for `source`, then
the following code example will be warned about.

```c++
void foo(int source, int x);

foo(b, src);
```

The abbreviations to recognise can be configured with the
`Abbreviations<opt_Abbreviations>`{.interpreted-text role="ref"} check
option. This heuristic is case-insensitive.

### Prefix

The _prefix_ heuristic reports if one of the strings is a sufficiently
long prefix of the other string, e.g. `target` to `targetPtr`. The
similarity percentage is the length ratio of the prefix to the longer
string, in the previous example, it would be [6 / 9 =
66.66\...]{.title-ref}%.

This heuristic can be configured with
`bounds<opt_Bounds>`{.interpreted-text role="ref"}. The default bounds
are: below [25]{.title-ref}% dissimilar and above [30]{.title-ref}%
similar. This heuristic is case-insensitive.

### Suffix

Analogous to the [Prefix]{.title-ref} heuristic. In the case of
`oldValue` and `value` compared, the similarity percentage is [8 / 5 =
62.5]{.title-ref}%.

This heuristic can be configured with
`bounds<opt_Bounds>`{.interpreted-text role="ref"}. The default bounds
are: below [25]{.title-ref}% dissimilar and above [30]{.title-ref}%
similar. This heuristic is case-insensitive.

### Substring

The substring heuristic combines the prefix and the suffix heuristic,
and tries to find the _longest common substring_ in the two strings
provided. The similarity percentage is the ratio of the found longest
common substring against the _longer_ of the two input strings. For
example, given `val` and `rvalue`, the similarity is [3 / 6 =
50]{.title-ref}%. If no characters are common in the two string,
[0]{.title-ref}%.

This heuristic can be configured with
`bounds<opt_Bounds>`{.interpreted-text role="ref"}. The default bounds
are: below [40]{.title-ref}% dissimilar and above [50]{.title-ref}%
similar. This heuristic is case-insensitive.

### Levenshtein distance (as [Levenshtein]{.title-ref})

The [Levenshtein
distance](http://en.wikipedia.org/wiki/Levenshtein_distance) describes
how many single-character changes (additions, changes, or removals) must
be applied to transform one string into another.

The Levenshtein distance is translated into a similarity percentage by
dividing it with the length of the _longer_ string, and taking its
complement with regards to [100]{.title-ref}%. For example, given
`something` and `anything`, the distance is [4]{.title-ref} edits, and
the similarity percentage is [100]{.title-ref}% [- 4 / 9 =
55.55\...]{.title-ref}%.

This heuristic can be configured with
`bounds<opt_Bounds>`{.interpreted-text role="ref"}. The default bounds
are: below [50]{.title-ref}% dissimilar and above [66]{.title-ref}%
similar. This heuristic is case-sensitive.

### Jaro\--Winkler distance (as [JaroWinkler]{.title-ref})

The [Jaro\--Winkler
distance](http://en.wikipedia.org/wiki/Jaro–Winkler_distance) is an edit
distance like the Levenshtein distance. It is calculated from the amount
of common characters that are sufficiently close to each other in
position, and to-be-changed characters. The original definition of Jaro
has been extended by Winkler to weigh prefix similarities more. The
similarity percentage is expressed as an average of the common and
non-common characters against the length of both strings.

This heuristic can be configured with
`bounds<opt_Bounds>`{.interpreted-text role="ref"}. The default bounds
are: below [75]{.title-ref}% dissimilar and above [85]{.title-ref}%
similar. This heuristic is case-insensitive.

### Sørensen\--Dice coefficient (as [Dice]{.title-ref})

The [Sørensen\--Dice
coefficient](http://en.wikipedia.org/wiki/Sørensen–Dice_coefficient) was
originally defined to measure the similarity of two sets. Formally, the
coefficient is calculated by dividing [2 \* #(intersection)]{.title-ref}
with [#(set1) + #(set2)]{.title-ref}, where [#()]{.title-ref} is the
cardinality function of sets. This metric is applied to strings by
creating bigrams (substring sequences of length 2) of the two strings
and using the set of bigrams for the two strings as the two sets.

This heuristic can be configured with
`bounds<opt_Bounds>`{.interpreted-text role="ref"}. The default bounds
are: below [60]{.title-ref}% dissimilar and above [70]{.title-ref}%
similar. This heuristic is case-insensitive.

## Options

::: option
MinimumIdentifierNameLength

Sets the minimum required length the argument and parameter names need
to have. Names shorter than this length will be ignored. Defaults to
[3]{.title-ref}.
:::

::: {#opt_Abbreviations}
::: option
Abbreviations

For the **Abbreviation** heuristic
(`see here<abbreviation_heuristic>`{.interpreted-text role="ref"}), this
option configures the abbreviations in the
[\"abbreviation=abbreviated_value\"]{.title-ref} format. The option is a
string, with each value joined by [\";\"]{.title-ref}.

By default, the following abbreviations are set:

> - [addr=address]{.title-ref}
> - [arr=array]{.title-ref}
> - [attr=attribute]{.title-ref}
> - [buf=buffer]{.title-ref}
> - [cl=client]{.title-ref}
> - [cnt=count]{.title-ref}
> - [col=column]{.title-ref}
> - [cpy=copy]{.title-ref}
> - [dest=destination]{.title-ref}
> - [dist=distance]{.title-ref}
> - [dst=distance]{.title-ref}
> - [elem=element]{.title-ref}
> - [hght=height]{.title-ref}
> - [i=index]{.title-ref}
> - [idx=index]{.title-ref}
> - [len=length]{.title-ref}
> - [ln=line]{.title-ref}
> - [lst=list]{.title-ref}
> - [nr=number]{.title-ref}
> - [num=number]{.title-ref}
> - [pos=position]{.title-ref}
> - [ptr=pointer]{.title-ref}
> - [ref=reference]{.title-ref}
> - [src=source]{.title-ref}
> - [srv=server]{.title-ref}
> - [stmt=statement]{.title-ref}
> - [str=string]{.title-ref}
> - [val=value]{.title-ref}
> - [var=variable]{.title-ref}
> - [vec=vector]{.title-ref}
> - [wdth=width]{.title-ref}
>   :::
>   :::

The configuration options for each implemented heuristic (see above) is
constructed dynamically. In the following,
[\<HeuristicName\>]{.title-ref} refers to one of the keys from the
heuristics implemented.

::: option
\<HeuristicName\>

[True]{.title-ref} or [False]{.title-ref}, whether a particular
heuristic, such as [Equality]{.title-ref} or [Levenshtein]{.title-ref}
is enabled.

Defaults to [True]{.title-ref} for every heuristic.
:::

::: {#opt_Bounds}
::: option
\<HeuristicName\>DissimilarBelow, \<HeuristicName\>SimilarAbove

A value between [0]{.title-ref} and [100]{.title-ref}, expressing a
percentage. The bounds set what percentage of similarity the heuristic
must deduce for the two identifiers to be considered similar or
dissimilar by the check.

Given arguments `arg1` and `arg2` passed to `param1` and `param2`,
respectively, the bounds check is performed in the following way: If the
similarity of the currently passed argument order (`arg1` to `param1`)
is **below** the [DissimilarBelow]{.title-ref} threshold, and the
similarity of the suggested swapped order (`arg1` to `param2`) is
**above** the [SimilarAbove]{.title-ref} threshold, the swap is
reported.

For the defaults of each heuristic,
`see above<heuristics>`{.interpreted-text role="ref"}.
:::
:::

## Name synthesis

When comparing the argument names and parameter names, the following
logic is used to gather the names for comparison:

Parameter names are the identifiers as written in the source code.

Argument names are:

> - If a variable is passed, the variable\'s name.
> - If a subsequent function call\'s return value is used as argument,
>   the called function\'s name.
> - Otherwise, empty string.

Empty argument or parameter names are ignored by the heuristics.
