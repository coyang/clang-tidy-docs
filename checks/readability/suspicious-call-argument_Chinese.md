以下是翻译后的 Markdown 文件内容，保留了中英文双语，中文位于英文下方，并以一个空行分隔：

# readability-suspicious-call-argument

Finds function calls where the arguments passed are provided out of order, based on the difference between the argument name and the parameter names of the function.

根据参数名与函数形参名之间的差异，查找函数调用中参数传递顺序可能错误的情况。

Given a function call f(foo, bar); and a function signature void f(T tvar, U uvar), the arguments foo and bar are swapped if foo (the argument name) is more similar to uvar (the other parameter) than tvar (the parameter it is currently passed to) and bar is more similar to tvar than uvar.

例如，给定函数调用 f(foo, bar); 和函数签名 void f(T tvar, U uvar)，如果 foo（实参名）与 uvar（另一个形参）更相似，而不是 tvar（当前接收 foo 的形参），并且 bar 与 tvar 比 uvar 更相似，则说明 foo 和 bar 的顺序可能被交换了。

Warnings might indicate either that the arguments are swapped, or that the names' cross-similarity might hinder code comprehension.

警告可能表示参数顺序被交换，或者参数名之间的相似性可能影响代码的可读性。

## Heuristics

The following heuristics are implemented in the check. If any of the enabled heuristics deem the arguments to be provided out of order, a warning will be issued.

以下启发式规则已在检查中实现。如果启用的任一规则判断参数顺序错误，则会发出警告。

The heuristics themselves are implemented by considering pairs of strings, and are symmetric, so in the following there is no distinction on which string is the argument name and which string is the parameter name.

这些启发式规则是通过比较字符串对来实现的，具有对称性，因此在以下内容中不区分哪个是实参名，哪个是形参名。

### Equality

The most trivial heuristic, which compares the two strings for case-insensitive equality.

最简单的启发式规则，比较两个字符串是否相等（不区分大小写）。

### Abbreviation {#abbreviation_heuristic}

Common abbreviations can be specified which will deem the strings similar if the abbreviated and the abbreviation stand together. For example, if src is registered as an abbreviation for source, then the following code example will be warned about.

可以指定常见缩写，如果缩写和完整形式出现在一起，则认为字符串相似。例如，如果将 src 注册为 source 的缩写，则以下代码将触发警告。

```c++
void foo(int source, int x);

foo(b, src);
```

The abbreviations to recognise can be configured with the Abbreviations<opt_Abbreviations> check option. This heuristic is case-insensitive.

可通过 Abbreviations<opt_Abbreviations> 检查选项配置识别的缩写。此规则不区分大小写。

### Prefix

The prefix heuristic reports if one of the strings is a sufficiently long prefix of the other string, e.g. target to targetPtr. The similarity percentage is the length ratio of the prefix to the longer string, in the previous example, it would be 6 / 9 = 66.66...%.

前缀启发式规则会在一个字符串是另一个字符串的足够长的前缀时触发，例如 target 与 targetPtr。相似度为前缀长度与较长字符串长度之比，例如上述例子中为 6 / 9 = 66.66...%。

This heuristic can be configured with bounds<opt_Bounds>. The default bounds are: below 25% dissimilar and above 30% similar. This heuristic is case-insensitive.

此规则可通过 bounds<opt_Bounds> 配置。默认阈值为：小于 25% 为不相似，大于 30% 为相似。此规则不区分大小写。

### Suffix

Analogous to the Prefix heuristic. In the case of oldValue and value compared, the similarity percentage is 8 / 5 = 62.5%.

与前缀规则类似。比较 oldValue 和 value 时，相似度为 8 / 5 = 62.5%。

This heuristic can be configured with bounds<opt_Bounds>. The default bounds are: below 25% dissimilar and above 30% similar. This heuristic is case-insensitive.

此规则可通过 bounds<opt_Bounds> 配置。默认阈值为：小于 25% 为不相似，大于 30% 为相似。此规则不区分大小写。

### Substring

The substring heuristic combines the prefix and the suffix heuristic, and tries to find the longest common substring in the two strings provided. The similarity percentage is the ratio of the found longest common substring against the longer of the two input strings. For example, given val and rvalue, the similarity is 3 / 6 = 50%. If no characters are common in the two string, 0%.

子串启发式规则结合了前缀和后缀规则，尝试找出两个字符串中的最长公共子串。相似度为最长公共子串长度与较长字符串长度之比。例如 val 与 rvalue 的相似度为 3 / 6 = 50%。若无公共字符，则为 0%。

This heuristic can be configured with bounds<opt_Bounds>. The default bounds are: below 40% dissimilar and above 50% similar. This heuristic is case-insensitive.

此规则可通过 bounds<opt_Bounds> 配置。默认阈值为：小于 40% 为不相似，大于 50% 为相似。此规则不区分大小写。

### Levenshtein distance (as Levenshtein)

The Levenshtein distance describes how many single-character changes (additions, changes, or removals) must be applied to transform one string into another.

Levenshtein 距离描述将一个字符串转换为另一个字符串所需的最少单字符操作数（添加、修改或删除）。

The Levenshtein distance is translated into a similarity percentage by dividing it with the length of the longer string, and taking its complement with regards to 100%. For example, given something and anything, the distance is 4 edits, and the similarity percentage is 100% - 4 / 9 = 55.55...%.

Levenshtein 距离通过将编辑距离除以较长字符串长度，并用 100% 减去该值来转换为相似度。例如 something 与 anything 的距离为 4，相似度为 100% - 4 / 9 = 55.55...%。

This heuristic can be configured with bounds<opt_Bounds>. The default bounds are: below 50% dissimilar and above 66% similar. This heuristic is case-sensitive.

此规则可通过 bounds<opt_Bounds> 配置。默认阈值为：小于 50% 为不相似，大于 66% 为相似。此规则区分大小写。

### Jaro–Winkler distance (as JaroWinkler)

The Jaro–Winkler distance is an edit distance like the Levenshtein distance. It is calculated from the amount of common characters that are sufficiently close to each other in position, and to-be-changed characters. The original definition of Jaro has been extended by Winkler to weigh prefix similarities more. The similarity percentage is expressed as an average of the common and non-common characters against the length of both strings.

Jaro–Winkler 距离是一种类似于 Levenshtein 距离的编辑距离。它基于字符在位置上足够接近的公共字符数和需更改字符数计算。Winkler 在 Jaro 的基础上增加了对前缀相似度的权重。相似度为公共字符与总长度的加权平均。

This heuristic can be configured with bounds<opt_Bounds>. The default bounds are: below 75% dissimilar and above 85% similar. This heuristic is case-insensitive.

此规则可通过 bounds<opt_Bounds> 配置。默认阈值为：小于 75% 为不相似，大于 85% 为相似。此规则不区分大小写。

### Sørensen–Dice coefficient (as Dice)

The Sørensen–Dice coefficient was originally defined to measure the similarity of two sets. Formally, the coefficient is calculated by dividing 2 × #(intersection) with #(set1) + #(set2), where #() is the cardinality function of sets. This metric is applied to strings by creating bigrams (substring sequences of length 2) of the two strings and using the set of bigrams for the two strings as the two sets.

Sørensen–Dice 系数最初用于衡量两个集合的相似度。其计算方式为 2 × #(交集) / (#(集合1) + #(集合2))，其中 #() 表示集合的基数。应用于字符串时，将字符串分解为 bigram（二字符子串）集合，并以此计算相似度。

This heuristic can be configured with bounds<opt_Bounds>. The default bounds are: below 60% dissimilar and above 70% similar. This heuristic is case-insensitive.

此规则可通过 bounds<opt_Bounds> 配置。默认阈值为：小于 60% 为不相似，大于 70% 为相似。此规则不区分大小写。

## Options

::: option
MinimumIdentifierNameLength

Sets the minimum required length the argument and parameter names need to have. Names shorter than this length will be ignored. Defaults to 3.

设置实参名和形参名的最小长度。短于该长度的名称将被忽略。默认值为 3。
:::

::: {#opt_Abbreviations}
::: option
Abbreviations

For the Abbreviation heuristic (see here), this option configures the abbreviations in the "abbreviation=abbreviated_value" format. The option is a string, with each value joined by ";".

用于 Abbreviation 启发式规则（见上文），此选项以 "缩写=完整形式" 格式配置缩写。多个值用 ";" 分隔。

By default, the following abbreviations are set:

默认配置如下缩写：

- addr=address
- arr=array
- attr=attribute
- buf=buffer
- cl=client
- cnt=count
- col=column
- cpy=copy
- dest=destination
- dist=distance
- dst=distance
- elem=element
- hght=height
- i=index
- idx=index
- len=length
- ln=line
- lst=list
- nr=number
- num=number
- pos=position
- ptr=pointer
- ref=reference
- src=source
- srv=server
- stmt=statement
- str=string
- val=value
- var=variable
- vec=vector
- wdth=width
  :::
  :::

The configuration options for each implemented heuristic (see above) is constructed dynamically. In the following, <HeuristicName> refers to one of the keys from the heuristics implemented.

每个已实现启发式规则的配置选项（见上文）是动态构建的。以下 <HeuristicName> 表示已实现规则的名称。

::: option
<HeuristicName>

True or False, whether a particular heuristic, such as Equality or Levenshtein is enabled.

设置某个启发式规则（如 Equality 或 Levenshtein）是否启用，取值为 True 或 False。

Defaults to True for every heuristic.

所有规则默认启用（True）。
:::

::: {#opt_Bounds}
::: option
<HeuristicName>DissimilarBelow, <HeuristicName>SimilarAbove

A value between 0 and 100, expressing a percentage. The bounds set what percentage of similarity the heuristic must deduce for the two identifiers to be considered similar or dissimilar by the check.

取值为 0 到 100 的百分比。该阈值用于判断两个标识符是否相似或不相似。

Given arguments arg1 and arg2 passed to param1 and param2, respectively, the bounds check is performed in the following way: If the similarity of the currently passed argument order (arg1 to param1) is below the DissimilarBelow threshold, and the similarity of the suggested swapped order (arg1 to param2) is above the SimilarAbove threshold, the swap is reported.

例如，若 arg1 和 arg2 分别传给 param1 和 param2，当 arg1 与 param1 的相似度低于 DissimilarBelow 阈值，且 arg1 与 param2 的相似度高于 SimilarAbove 阈值时，将报告参数顺序可能错误。

For the defaults of each heuristic, see above.

各规则的默认值见上文。
:::
:::

## Name synthesis

When comparing the argument names and parameter names, the following logic is used to gather the names for comparison:

在比较实参名和形参名时，使用以下逻辑收集名称以供比较：

Parameter names are the identifiers as written in the source code.

形参名为源代码中定义的标识符。

Argument names are:

实参名为：

- If a variable is passed, the variable's name.
- 如果传递的是变量，则使用变量名。
- If a subsequent function call's return value is used as argument, the called function's name.
- 如果传递的是函数调用的返回值，则使用被调用函数的名称。
- Otherwise, empty string.
- 否则，使用空字符串。

Empty argument or parameter names are ignored by the heuristics.

启发式规则会忽略空的实参名或形参名。
