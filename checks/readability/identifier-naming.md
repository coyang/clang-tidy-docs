# readability-identifier-naming

Checks for identifiers naming style mismatch.

This check will try to enforce coding guidelines on the identifiers
naming. It supports one of the following casing types and tries to
convert from one to another if a mismatch is detected

Casing types include:

> - `lower_case`
> - `UPPER_CASE`
> - `camelBack`
> - `CamelCase`
> - `camel_Snake_Back`
> - `Camel_Snake_Case`
> - `aNy_CasE`
> - `Leading_upper_snake_case`

It also supports a fixed prefix and suffix that will be prepended or
appended to the identifiers, regardless of the casing.

Many configuration options are available, in order to be able to create
different rules for different kinds of identifiers. In general, the
rules are falling back to a more generic rule if the specific case is
not configured.

The naming of virtual methods is reported where they occur in the base
class, but not where they are overridden, as it can\'t be fixed locally
there. This also applies for pseudo-override patterns like CRTP.

`Leading_upper_snake_case` is a naming convention where the first word
is capitalized followed by lower case word(s) separated by underscore(s)
\'\_\'. Examples include: [Cap_snake_case]{.title-ref},
[Cobra_case]{.title-ref}, [Foo_bar_baz]{.title-ref}, and
[Master_copy_8gb]{.title-ref}.

Hungarian notation can be customized using different _HungarianPrefix_
settings. The options and their corresponding values are:

> - `Off` - the default setting
> - `On` - example: `int iVariable`
> - `LowerCase` - example: `int i_Variable`
> - `CamelCase` - example: `int IVariable`

## Options

The following options are described below:

> - `AbstractClassCase`{.interpreted-text role="option"},
>   `AbstractClassPrefix`{.interpreted-text role="option"},
>   `AbstractClassSuffix`{.interpreted-text role="option"},
>   `AbstractClassIgnoredRegexp`{.interpreted-text role="option"},
>   `AbstractClassHungarianPrefix`{.interpreted-text role="option"}
> - `AggressiveDependentMemberLookup`{.interpreted-text role="option"}
> - `CheckAnonFieldInParent`{.interpreted-text role="option"}
> - `ClassCase`{.interpreted-text role="option"},
>   `ClassPrefix`{.interpreted-text role="option"},
>   `ClassSuffix`{.interpreted-text role="option"},
>   `ClassIgnoredRegexp`{.interpreted-text role="option"},
>   `ClassHungarianPrefix`{.interpreted-text role="option"}
> - `ClassConstantCase`{.interpreted-text role="option"},
>   `ClassConstantPrefix`{.interpreted-text role="option"},
>   `ClassConstantSuffix`{.interpreted-text role="option"},
>   `ClassConstantIgnoredRegexp`{.interpreted-text role="option"},
>   `ClassConstantHungarianPrefix`{.interpreted-text role="option"}
> - `ClassMemberCase`{.interpreted-text role="option"},
>   `ClassMemberPrefix`{.interpreted-text role="option"},
>   `ClassMemberSuffix`{.interpreted-text role="option"},
>   `ClassMemberIgnoredRegexp`{.interpreted-text role="option"},
>   `ClassMemberHungarianPrefix`{.interpreted-text role="option"}
> - `ClassMethodCase`{.interpreted-text role="option"},
>   `ClassMethodPrefix`{.interpreted-text role="option"},
>   `ClassMethodSuffix`{.interpreted-text role="option"},
>   `ClassMethodIgnoredRegexp`{.interpreted-text role="option"}
> - `ConceptCase`{.interpreted-text role="option"},
>   `ConceptPrefix`{.interpreted-text role="option"},
>   `ConceptSuffix`{.interpreted-text role="option"},
>   `ConceptIgnoredRegexp`{.interpreted-text role="option"}
> - `ConstantCase`{.interpreted-text role="option"},
>   `ConstantPrefix`{.interpreted-text role="option"},
>   `ConstantSuffix`{.interpreted-text role="option"},
>   `ConstantIgnoredRegexp`{.interpreted-text role="option"},
>   `ConstantHungarianPrefix`{.interpreted-text role="option"}
> - `ConstantMemberCase`{.interpreted-text role="option"},
>   `ConstantMemberPrefix`{.interpreted-text role="option"},
>   `ConstantMemberSuffix`{.interpreted-text role="option"},
>   `ConstantMemberIgnoredRegexp`{.interpreted-text role="option"},
>   `ConstantMemberHungarianPrefix`{.interpreted-text role="option"}
> - `ConstantParameterCase`{.interpreted-text role="option"},
>   `ConstantParameterPrefix`{.interpreted-text role="option"},
>   `ConstantParameterSuffix`{.interpreted-text role="option"},
>   `ConstantParameterIgnoredRegexp`{.interpreted-text role="option"},
>   `ConstantParameterHungarianPrefix`{.interpreted-text
>   role="option"}
> - `ConstantPointerParameterCase`{.interpreted-text role="option"},
>   `ConstantPointerParameterPrefix`{.interpreted-text role="option"},
>   `ConstantPointerParameterSuffix`{.interpreted-text role="option"},
>   `ConstantPointerParameterIgnoredRegexp`{.interpreted-text
>   role="option"},
>   `ConstantPointerParameterHungarianPrefix`{.interpreted-text
>   role="option"}
> - `ConstexprFunctionCase`{.interpreted-text role="option"},
>   `ConstexprFunctionPrefix`{.interpreted-text role="option"},
>   `ConstexprFunctionSuffix`{.interpreted-text role="option"},
>   `ConstexprFunctionIgnoredRegexp`{.interpreted-text role="option"}
> - `ConstexprMethodCase`{.interpreted-text role="option"},
>   `ConstexprMethodPrefix`{.interpreted-text role="option"},
>   `ConstexprMethodSuffix`{.interpreted-text role="option"},
>   `ConstexprMethodIgnoredRegexp`{.interpreted-text role="option"}
> - `ConstexprVariableCase`{.interpreted-text role="option"},
>   `ConstexprVariablePrefix`{.interpreted-text role="option"},
>   `ConstexprVariableSuffix`{.interpreted-text role="option"},
>   `ConstexprVariableIgnoredRegexp`{.interpreted-text role="option"},
>   `ConstexprVariableHungarianPrefix`{.interpreted-text
>   role="option"}
> - `EnumCase`{.interpreted-text role="option"},
>   `EnumPrefix`{.interpreted-text role="option"},
>   `EnumSuffix`{.interpreted-text role="option"},
>   `EnumIgnoredRegexp`{.interpreted-text role="option"}
> - `EnumConstantCase`{.interpreted-text role="option"},
>   `EnumConstantPrefix`{.interpreted-text role="option"},
>   `EnumConstantSuffix`{.interpreted-text role="option"},
>   `EnumConstantIgnoredRegexp`{.interpreted-text role="option"},
>   `EnumConstantHungarianPrefix`{.interpreted-text role="option"}
> - `FunctionCase`{.interpreted-text role="option"},
>   `FunctionPrefix`{.interpreted-text role="option"},
>   `FunctionSuffix`{.interpreted-text role="option"},
>   `FunctionIgnoredRegexp`{.interpreted-text role="option"}
> - `GetConfigPerFile`{.interpreted-text role="option"}
> - `GlobalConstantCase`{.interpreted-text role="option"},
>   `GlobalConstantPrefix`{.interpreted-text role="option"},
>   `GlobalConstantSuffix`{.interpreted-text role="option"},
>   `GlobalConstantIgnoredRegexp`{.interpreted-text role="option"},
>   `GlobalConstantHungarianPrefix`{.interpreted-text role="option"}
> - `GlobalConstantPointerCase`{.interpreted-text role="option"},
>   `GlobalConstantPointerPrefix`{.interpreted-text role="option"},
>   `GlobalConstantPointerSuffix`{.interpreted-text role="option"},
>   `GlobalConstantPointerIgnoredRegexp`{.interpreted-text
>   role="option"},
>   `GlobalConstantPointerHungarianPrefix`{.interpreted-text
>   role="option"}
> - `GlobalFunctionCase`{.interpreted-text role="option"},
>   `GlobalFunctionPrefix`{.interpreted-text role="option"},
>   `GlobalFunctionSuffix`{.interpreted-text role="option"},
>   `GlobalFunctionIgnoredRegexp`{.interpreted-text role="option"}
> - `GlobalPointerCase`{.interpreted-text role="option"},
>   `GlobalPointerPrefix`{.interpreted-text role="option"},
>   `GlobalPointerSuffix`{.interpreted-text role="option"},
>   `GlobalPointerIgnoredRegexp`{.interpreted-text role="option"},
>   `GlobalPointerHungarianPrefix`{.interpreted-text role="option"}
> - `GlobalVariableCase`{.interpreted-text role="option"},
>   `GlobalVariablePrefix`{.interpreted-text role="option"},
>   `GlobalVariableSuffix`{.interpreted-text role="option"},
>   `GlobalVariableIgnoredRegexp`{.interpreted-text role="option"},
>   `GlobalVariableHungarianPrefix`{.interpreted-text role="option"}
> - `IgnoreMainLikeFunctions`{.interpreted-text role="option"}
> - `InlineNamespaceCase`{.interpreted-text role="option"},
>   `InlineNamespacePrefix`{.interpreted-text role="option"},
>   `InlineNamespaceSuffix`{.interpreted-text role="option"},
>   `InlineNamespaceIgnoredRegexp`{.interpreted-text role="option"}
> - `LocalConstantCase`{.interpreted-text role="option"},
>   `LocalConstantPrefix`{.interpreted-text role="option"},
>   `LocalConstantSuffix`{.interpreted-text role="option"},
>   `LocalConstantIgnoredRegexp`{.interpreted-text role="option"},
>   `LocalConstantHungarianPrefix`{.interpreted-text role="option"}
> - `LocalConstantPointerCase`{.interpreted-text role="option"},
>   `LocalConstantPointerPrefix`{.interpreted-text role="option"},
>   `LocalConstantPointerSuffix`{.interpreted-text role="option"},
>   `LocalConstantPointerIgnoredRegexp`{.interpreted-text
>   role="option"},
>   `LocalConstantPointerHungarianPrefix`{.interpreted-text
>   role="option"}
> - `LocalPointerCase`{.interpreted-text role="option"},
>   `LocalPointerPrefix`{.interpreted-text role="option"},
>   `LocalPointerSuffix`{.interpreted-text role="option"},
>   `LocalPointerIgnoredRegexp`{.interpreted-text role="option"},
>   `LocalPointerHungarianPrefix`{.interpreted-text role="option"}
> - `LocalVariableCase`{.interpreted-text role="option"},
>   `LocalVariablePrefix`{.interpreted-text role="option"},
>   `LocalVariableSuffix`{.interpreted-text role="option"},
>   `LocalVariableIgnoredRegexp`{.interpreted-text role="option"},
>   `LocalVariableHungarianPrefix`{.interpreted-text role="option"}
> - `MacroDefinitionCase`{.interpreted-text role="option"},
>   `MacroDefinitionPrefix`{.interpreted-text role="option"},
>   `MacroDefinitionSuffix`{.interpreted-text role="option"},
>   `MacroDefinitionIgnoredRegexp`{.interpreted-text role="option"}
> - `MemberCase`{.interpreted-text role="option"},
>   `MemberPrefix`{.interpreted-text role="option"},
>   `MemberSuffix`{.interpreted-text role="option"},
>   `MemberIgnoredRegexp`{.interpreted-text role="option"},
>   `MemberHungarianPrefix`{.interpreted-text role="option"}
> - `MethodCase`{.interpreted-text role="option"},
>   `MethodPrefix`{.interpreted-text role="option"},
>   `MethodSuffix`{.interpreted-text role="option"},
>   `MethodIgnoredRegexp`{.interpreted-text role="option"}
> - `NamespaceCase`{.interpreted-text role="option"},
>   `NamespacePrefix`{.interpreted-text role="option"},
>   `NamespaceSuffix`{.interpreted-text role="option"},
>   `NamespaceIgnoredRegexp`{.interpreted-text role="option"}
> - `ParameterCase`{.interpreted-text role="option"},
>   `ParameterPrefix`{.interpreted-text role="option"},
>   `ParameterSuffix`{.interpreted-text role="option"},
>   `ParameterIgnoredRegexp`{.interpreted-text role="option"},
>   `ParameterHungarianPrefix`{.interpreted-text role="option"}
> - `ParameterPackCase`{.interpreted-text role="option"},
>   `ParameterPackPrefix`{.interpreted-text role="option"},
>   `ParameterPackSuffix`{.interpreted-text role="option"},
>   `ParameterPackIgnoredRegexp`{.interpreted-text role="option"}
> - `PointerParameterCase`{.interpreted-text role="option"},
>   `PointerParameterPrefix`{.interpreted-text role="option"},
>   `PointerParameterSuffix`{.interpreted-text role="option"},
>   `PointerParameterIgnoredRegexp`{.interpreted-text role="option"},
>   `PointerParameterHungarianPrefix`{.interpreted-text role="option"}
> - `PrivateMemberCase`{.interpreted-text role="option"},
>   `PrivateMemberPrefix`{.interpreted-text role="option"},
>   `PrivateMemberSuffix`{.interpreted-text role="option"},
>   `PrivateMemberIgnoredRegexp`{.interpreted-text role="option"},
>   `PrivateMemberHungarianPrefix`{.interpreted-text role="option"}
> - `PrivateMethodCase`{.interpreted-text role="option"},
>   `PrivateMethodPrefix`{.interpreted-text role="option"},
>   `PrivateMethodSuffix`{.interpreted-text role="option"},
>   `PrivateMethodIgnoredRegexp`{.interpreted-text role="option"}
> - `ProtectedMemberCase`{.interpreted-text role="option"},
>   `ProtectedMemberPrefix`{.interpreted-text role="option"},
>   `ProtectedMemberSuffix`{.interpreted-text role="option"},
>   `ProtectedMemberIgnoredRegexp`{.interpreted-text role="option"},
>   `ProtectedMemberHungarianPrefix`{.interpreted-text role="option"}
> - `ProtectedMethodCase`{.interpreted-text role="option"},
>   `ProtectedMethodPrefix`{.interpreted-text role="option"},
>   `ProtectedMethodSuffix`{.interpreted-text role="option"},
>   `ProtectedMethodIgnoredRegexp`{.interpreted-text role="option"}
> - `PublicMemberCase`{.interpreted-text role="option"},
>   `PublicMemberPrefix`{.interpreted-text role="option"},
>   `PublicMemberSuffix`{.interpreted-text role="option"},
>   `PublicMemberIgnoredRegexp`{.interpreted-text role="option"},
>   `PublicMemberHungarianPrefix`{.interpreted-text role="option"}
> - `PublicMethodCase`{.interpreted-text role="option"},
>   `PublicMethodPrefix`{.interpreted-text role="option"},
>   `PublicMethodSuffix`{.interpreted-text role="option"},
>   `PublicMethodIgnoredRegexp`{.interpreted-text role="option"}
> - `ScopedEnumConstantCase`{.interpreted-text role="option"},
>   `ScopedEnumConstantPrefix`{.interpreted-text role="option"},
>   `ScopedEnumConstantSuffix`{.interpreted-text role="option"},
>   `ScopedEnumConstantIgnoredRegexp`{.interpreted-text role="option"}
> - `StaticConstantCase`{.interpreted-text role="option"},
>   `StaticConstantPrefix`{.interpreted-text role="option"},
>   `StaticConstantSuffix`{.interpreted-text role="option"},
>   `StaticConstantIgnoredRegexp`{.interpreted-text role="option"},
>   `StaticConstantHungarianPrefix`{.interpreted-text role="option"}
> - `StaticVariableCase`{.interpreted-text role="option"},
>   `StaticVariablePrefix`{.interpreted-text role="option"},
>   `StaticVariableSuffix`{.interpreted-text role="option"},
>   `StaticVariableIgnoredRegexp`{.interpreted-text role="option"},
>   `StaticVariableHungarianPrefix`{.interpreted-text role="option"}
> - `StructCase`{.interpreted-text role="option"},
>   `StructPrefix`{.interpreted-text role="option"},
>   `StructSuffix`{.interpreted-text role="option"},
>   `StructIgnoredRegexp`{.interpreted-text role="option"}
> - `TemplateParameterCase`{.interpreted-text role="option"},
>   `TemplateParameterPrefix`{.interpreted-text role="option"},
>   `TemplateParameterSuffix`{.interpreted-text role="option"},
>   `TemplateParameterIgnoredRegexp`{.interpreted-text role="option"}
> - `TemplateTemplateParameterCase`{.interpreted-text role="option"},
>   `TemplateTemplateParameterPrefix`{.interpreted-text
>   role="option"},
>   `TemplateTemplateParameterSuffix`{.interpreted-text
>   role="option"},
>   `TemplateTemplateParameterIgnoredRegexp`{.interpreted-text
>   role="option"}
> - `TypeAliasCase`{.interpreted-text role="option"},
>   `TypeAliasPrefix`{.interpreted-text role="option"},
>   `TypeAliasSuffix`{.interpreted-text role="option"},
>   `TypeAliasIgnoredRegexp`{.interpreted-text role="option"}
> - `TypedefCase`{.interpreted-text role="option"},
>   `TypedefPrefix`{.interpreted-text role="option"},
>   `TypedefSuffix`{.interpreted-text role="option"},
>   `TypedefIgnoredRegexp`{.interpreted-text role="option"}
> - `TypeTemplateParameterCase`{.interpreted-text role="option"},
>   `TypeTemplateParameterPrefix`{.interpreted-text role="option"},
>   `TypeTemplateParameterSuffix`{.interpreted-text role="option"},
>   `TypeTemplateParameterIgnoredRegexp`{.interpreted-text
>   role="option"}
> - `UnionCase`{.interpreted-text role="option"},
>   `UnionPrefix`{.interpreted-text role="option"},
>   `UnionSuffix`{.interpreted-text role="option"},
>   `UnionIgnoredRegexp`{.interpreted-text role="option"}
> - `ValueTemplateParameterCase`{.interpreted-text role="option"},
>   `ValueTemplateParameterPrefix`{.interpreted-text role="option"},
>   `ValueTemplateParameterSuffix`{.interpreted-text role="option"},
>   `ValueTemplateParameterIgnoredRegexp`{.interpreted-text
>   role="option"}
> - `VariableCase`{.interpreted-text role="option"},
>   `VariablePrefix`{.interpreted-text role="option"},
>   `VariableSuffix`{.interpreted-text role="option"},
>   `VariableIgnoredRegexp`{.interpreted-text role="option"},
>   `VariableHungarianPrefix`{.interpreted-text role="option"}
> - `VirtualMethodCase`{.interpreted-text role="option"},
>   `VirtualMethodPrefix`{.interpreted-text role="option"},
>   `VirtualMethodSuffix`{.interpreted-text role="option"},
>   `VirtualMethodIgnoredRegexp`{.interpreted-text role="option"}

::: option
AbstractClassCase

When defined, the check will ensure abstract class names conform to the
selected casing.
:::

::: option
AbstractClassPrefix

When defined, the check will ensure abstract class names will add the
prefixed with the given value (regardless of casing).
:::

::: option
AbstractClassIgnoredRegexp

Identifier naming checks won\'t be enforced for abstract class names
matching this regular expression.
:::

::: option
AbstractClassSuffix

When defined, the check will ensure abstract class names will add the
suffix with the given value (regardless of casing).
:::

::: option
AbstractClassHungarianPrefix

When enabled, the check ensures that the declared identifier will have a
Hungarian notation prefix based on the declared type.
:::

For example using values of:

> - AbstractClassCase of `lower_case`
> - AbstractClassPrefix of `pre_`
> - AbstractClassSuffix of `_post`
> - AbstractClassHungarianPrefix of `On`

Identifies and/or transforms abstract class names as follows:

Before:

```c++
class ABSTRACT_CLASS {
public:
  ABSTRACT_CLASS();
};
```

After:

```c++
class pre_abstract_class_post {
public:
  pre_abstract_class_post();
};
```

::: option
AggressiveDependentMemberLookup

When set to [true]{.title-ref} the check will look in dependent base
classes for dependent member references that need changing. This can
lead to errors with template specializations so the default value is
[false]{.title-ref}.
:::

For example using values of:

> - ClassMemberCase of `lower_case`

Before:

```c++
template <typename T>
struct Base {
  T BadNamedMember;
};

template <typename T>
struct Derived : Base<T> {
  void reset() {
    this->BadNamedMember = 0;
  }
};
```

After if AggressiveDependentMemberLookup is \`false\`:

```c++
template <typename T>
struct Base {
  T bad_named_member;
};

template <typename T>
struct Derived : Base<T> {
  void reset() {
    this->BadNamedMember = 0;
  }
};
```

After if AggressiveDependentMemberLookup is \`true\`:

```c++
template <typename T>
struct Base {
  T bad_named_member;
};

template <typename T>
struct Derived : Base<T> {
  void reset() {
    this->bad_named_member = 0;
  }
};
```

::: option
CheckAnonFieldInParent

When set to [true]{.title-ref}, fields in anonymous records (i.e.
anonymous unions and structs) will be treated as names in the enclosing
scope rather than public members of the anonymous record for the purpose
of name checking.
:::

For example:

```c++
class Foo {
private:
  union {
    int iv_;
    float fv_;
  };
};
```

If `CheckAnonFieldInParent`{.interpreted-text role="option"} is
[false]{.title-ref}, you may get warnings that `iv_` and `fv_` are not
coherent to public member names, because `iv_` and `fv_` are public
members of the anonymous union. When
`CheckAnonFieldInParent`{.interpreted-text role="option"} is
[true]{.title-ref}, `iv_` and `fv_` will be treated as private data
members of `Foo` for the purpose of name checking and thus no warnings
will be emitted.

::: option
ClassCase

When defined, the check will ensure class names conform to the selected
casing.
:::

::: option
ClassPrefix

When defined, the check will ensure class names will add the prefixed
with the given value (regardless of casing).
:::

::: option
ClassIgnoredRegexp

Identifier naming checks won\'t be enforced for class names matching
this regular expression.
:::

::: option
ClassSuffix

When defined, the check will ensure class names will add the suffix with
the given value (regardless of casing).
:::

::: option
ClassHungarianPrefix

When enabled, the check ensures that the declared identifier will have a
Hungarian notation prefix based on the declared type.
:::

For example using values of:

> - ClassCase of `lower_case`
> - ClassPrefix of `pre_`
> - ClassSuffix of `_post`
> - ClassHungarianPrefix of `On`

Identifies and/or transforms class names as follows:

Before:

```c++
class FOO {
public:
  FOO();
  ~FOO();
};
```

After:

```c++
class pre_foo_post {
public:
  pre_foo_post();
  ~pre_foo_post();
};
```

::: option
ClassConstantCase

When defined, the check will ensure class constant names conform to the
selected casing.
:::

::: option
ClassConstantPrefix

When defined, the check will ensure class constant names will add the
prefixed with the given value (regardless of casing).
:::

::: option
ClassConstantIgnoredRegexp

Identifier naming checks won\'t be enforced for class constant names
matching this regular expression.
:::

::: option
ClassConstantSuffix

When defined, the check will ensure class constant names will add the
suffix with the given value (regardless of casing).
:::

::: option
ClassConstantHungarianPrefix

When enabled, the check ensures that the declared identifier will have a
Hungarian notation prefix based on the declared type.
:::

For example using values of:

> - ClassConstantCase of `lower_case`
> - ClassConstantPrefix of `pre_`
> - ClassConstantSuffix of `_post`
> - ClassConstantHungarianPrefix of `On`

Identifies and/or transforms class constant names as follows:

Before:

```c++
class FOO {
public:
  static const int CLASS_CONSTANT;
};
```

After:

```c++
class FOO {
public:
  static const int pre_class_constant_post;
};
```

::: option
ClassMemberCase

When defined, the check will ensure class member names conform to the
selected casing.
:::

::: option
ClassMemberPrefix

When defined, the check will ensure class member names will add the
prefixed with the given value (regardless of casing).
:::

::: option
ClassMemberIgnoredRegexp

Identifier naming checks won\'t be enforced for class member names
matching this regular expression.
:::

::: option
ClassMemberSuffix

When defined, the check will ensure class member names will add the
suffix with the given value (regardless of casing).
:::

::: option
ClassMemberHungarianPrefix

When enabled, the check ensures that the declared identifier will have a
Hungarian notation prefix based on the declared type.
:::

For example using values of:

> - ClassMemberCase of `lower_case`
> - ClassMemberPrefix of `pre_`
> - ClassMemberSuffix of `_post`
> - ClassMemberHungarianPrefix of `On`

Identifies and/or transforms class member names as follows:

Before:

```c++
class FOO {
public:
  static int CLASS_CONSTANT;
};
```

After:

```c++
class FOO {
public:
  static int pre_class_constant_post;
};
```

::: option
ClassMethodCase

When defined, the check will ensure class method names conform to the
selected casing.
:::

::: option
ClassMethodPrefix

When defined, the check will ensure class method names will add the
prefixed with the given value (regardless of casing).
:::

::: option
ClassMethodIgnoredRegexp

Identifier naming checks won\'t be enforced for class method names
matching this regular expression.
:::

::: option
ClassMethodSuffix

When defined, the check will ensure class method names will add the
suffix with the given value (regardless of casing).
:::

For example using values of:

> - ClassMethodCase of `lower_case`
> - ClassMethodPrefix of `pre_`
> - ClassMethodSuffix of `_post`

Identifies and/or transforms class method names as follows:

Before:

```c++
class FOO {
public:
  int CLASS_MEMBER();
};
```

After:

```c++
class FOO {
public:
  int pre_class_member_post();
};
```

::: option
ConceptCase

When defined, the check will ensure concept names conform to the
selected casing.
:::

::: option
ConceptPrefix

When defined, the check will ensure concept names will add the prefixed
with the given value (regardless of casing).
:::

::: option
ConceptIgnoredRegexp

Identifier naming checks won\'t be enforced for concept names matching
this regular expression.
:::

::: option
ConceptSuffix

When defined, the check will ensure concept names will add the suffix
with the given value (regardless of casing).
:::

For example using values of:

> - ConceptCase of `CamelCase`
> - ConceptPrefix of `Pre`
> - ConceptSuffix of `Post`

Identifies and/or transforms concept names as follows:

Before:

```c++
template<typename T> concept my_concept = requires (T t) { {t++}; };
```

After:

```c++
template<typename T> concept PreMyConceptPost = requires (T t) { {t++}; };
```

::: option
ConstantCase

When defined, the check will ensure constant names conform to the
selected casing.
:::

::: option
ConstantPrefix

When defined, the check will ensure constant names will add the prefixed
with the given value (regardless of casing).
:::

::: option
ConstantIgnoredRegexp

Identifier naming checks won\'t be enforced for constant names matching
this regular expression.
:::

::: option
ConstantSuffix

When defined, the check will ensure constant names will add the suffix
with the given value (regardless of casing).
:::

::: option
ConstantHungarianPrefix

When enabled, the check ensures that the declared identifier will have a
Hungarian notation prefix based on the declared type.
:::

For example using values of:

> - ConstantCase of `lower_case`
> - ConstantPrefix of `pre_`
> - ConstantSuffix of `_post`
> - ConstantHungarianPrefix of `On`

Identifies and/or transforms constant names as follows:

Before:

```c++
void function() { unsigned const MyConst_array[] = {1, 2, 3}; }
```

After:

```c++
void function() { unsigned const pre_myconst_array_post[] = {1, 2, 3}; }
```

::: option
ConstantMemberCase

When defined, the check will ensure constant member names conform to the
selected casing.
:::

::: option
ConstantMemberPrefix

When defined, the check will ensure constant member names will add the
prefixed with the given value (regardless of casing).
:::

::: option
ConstantMemberIgnoredRegexp

Identifier naming checks won\'t be enforced for constant member names
matching this regular expression.
:::

::: option
ConstantMemberSuffix

When defined, the check will ensure constant member names will add the
suffix with the given value (regardless of casing).
:::

::: option
ConstantMemberHungarianPrefix

When enabled, the check ensures that the declared identifier will have a
Hungarian notation prefix based on the declared type.
:::

For example using values of:

> - ConstantMemberCase of `lower_case`
> - ConstantMemberPrefix of `pre_`
> - ConstantMemberSuffix of `_post`
> - ConstantMemberHungarianPrefix of `On`

Identifies and/or transforms constant member names as follows:

Before:

```c++
class Foo {
  char const MY_ConstMember_string[4] = "123";
}
```

After:

```c++
class Foo {
  char const pre_my_constmember_string_post[4] = "123";
}
```

::: option
ConstantParameterCase

When defined, the check will ensure constant parameter names conform to
the selected casing.
:::

::: option
ConstantParameterPrefix

When defined, the check will ensure constant parameter names will add
the prefixed with the given value (regardless of casing).
:::

::: option
ConstantParameterIgnoredRegexp

Identifier naming checks won\'t be enforced for constant parameter names
matching this regular expression.
:::

::: option
ConstantParameterSuffix

When defined, the check will ensure constant parameter names will add
the suffix with the given value (regardless of casing).
:::

::: option
ConstantParameterHungarianPrefix

When enabled, the check ensures that the declared identifier will have a
Hungarian notation prefix based on the declared type.
:::

For example using values of:

> - ConstantParameterCase of `lower_case`
> - ConstantParameterPrefix of `pre_`
> - ConstantParameterSuffix of `_post`
> - ConstantParameterHungarianPrefix of `On`

Identifies and/or transforms constant parameter names as follows:

Before:

```c++
void GLOBAL_FUNCTION(int PARAMETER_1, int const CONST_parameter);
```

After:

```c++
void GLOBAL_FUNCTION(int PARAMETER_1, int const pre_const_parameter_post);
```

::: option
ConstantPointerParameterCase

When defined, the check will ensure constant pointer parameter names
conform to the selected casing.
:::

::: option
ConstantPointerParameterPrefix

When defined, the check will ensure constant pointer parameter names
will add the prefixed with the given value (regardless of casing).
:::

::: option
ConstantPointerParameterIgnoredRegexp

Identifier naming checks won\'t be enforced for constant pointer
parameter names matching this regular expression.
:::

::: option
ConstantPointerParameterSuffix

When defined, the check will ensure constant pointer parameter names
will add the suffix with the given value (regardless of casing).
:::

::: option
ConstantPointerParameterHungarianPrefix

When enabled, the check ensures that the declared identifier will have a
Hungarian notation prefix based on the declared type.
:::

For example using values of:

> - ConstantPointerParameterCase of `lower_case`
> - ConstantPointerParameterPrefix of `pre_`
> - ConstantPointerParameterSuffix of `_post`
> - ConstantPointerParameterHungarianPrefix of `On`

Identifies and/or transforms constant pointer parameter names as
follows:

Before:

```c++
void GLOBAL_FUNCTION(int const *CONST_parameter);
```

After:

```c++
void GLOBAL_FUNCTION(int const *pre_const_parameter_post);
```

::: option
ConstexprFunctionCase

When defined, the check will ensure constexpr function names conform to
the selected casing.
:::

::: option
ConstexprFunctionPrefix

When defined, the check will ensure constexpr function names will add
the prefixed with the given value (regardless of casing).
:::

::: option
ConstexprFunctionIgnoredRegexp

Identifier naming checks won\'t be enforced for constexpr function names
matching this regular expression.
:::

::: option
ConstexprFunctionSuffix

When defined, the check will ensure constexpr function names will add
the suffix with the given value (regardless of casing).
:::

For example using values of:

> - ConstexprFunctionCase of `lower_case`
> - ConstexprFunctionPrefix of `pre_`
> - ConstexprFunctionSuffix of `_post`

Identifies and/or transforms constexpr function names as follows:

Before:

```c++
constexpr int CE_function() { return 3; }
```

After:

```c++
constexpr int pre_ce_function_post() { return 3; }
```

::: option
ConstexprMethodCase

When defined, the check will ensure constexpr method names conform to
the selected casing.
:::

::: option
ConstexprMethodPrefix

When defined, the check will ensure constexpr method names will add the
prefixed with the given value (regardless of casing).
:::

::: option
ConstexprMethodIgnoredRegexp

Identifier naming checks won\'t be enforced for constexpr method names
matching this regular expression.
:::

::: option
ConstexprMethodSuffix

When defined, the check will ensure constexpr method names will add the
suffix with the given value (regardless of casing).
:::

For example using values of:

> - ConstexprMethodCase of `lower_case`
> - ConstexprMethodPrefix of `pre_`
> - ConstexprMethodSuffix of `_post`

Identifies and/or transforms constexpr method names as follows:

Before:

```c++
class Foo {
public:
  constexpr int CST_expr_Method() { return 2; }
}
```

After:

```c++
class Foo {
public:
  constexpr int pre_cst_expr_method_post() { return 2; }
}
```

::: option
ConstexprVariableCase

When defined, the check will ensure constexpr variable names conform to
the selected casing.
:::

::: option
ConstexprVariablePrefix

When defined, the check will ensure constexpr variable names will add
the prefixed with the given value (regardless of casing).
:::

::: option
ConstexprVariableIgnoredRegexp

Identifier naming checks won\'t be enforced for constexpr variable names
matching this regular expression.
:::

::: option
ConstexprVariableSuffix

When defined, the check will ensure constexpr variable names will add
the suffix with the given value (regardless of casing).
:::

::: option
ConstexprVariableHungarianPrefix

When enabled, the check ensures that the declared identifier will have a
Hungarian notation prefix based on the declared type.
:::

For example using values of:

> - ConstexprVariableCase of `lower_case`
> - ConstexprVariablePrefix of `pre_`
> - ConstexprVariableSuffix of `_post`
> - ConstexprVariableHungarianPrefix of `On`

Identifies and/or transforms constexpr variable names as follows:

Before:

```c++
constexpr int ConstExpr_variable = MyConstant;
```

After:

```c++
constexpr int pre_constexpr_variable_post = MyConstant;
```

::: option
EnumCase

When defined, the check will ensure enumeration names conform to the
selected casing.
:::

::: option
EnumPrefix

When defined, the check will ensure enumeration names will add the
prefixed with the given value (regardless of casing).
:::

::: option
EnumIgnoredRegexp

Identifier naming checks won\'t be enforced for enumeration names
matching this regular expression.
:::

::: option
EnumSuffix

When defined, the check will ensure enumeration names will add the
suffix with the given value (regardless of casing).
:::

For example using values of:

> - EnumCase of `lower_case`
> - EnumPrefix of `pre_`
> - EnumSuffix of `_post`

Identifies and/or transforms enumeration names as follows:

Before:

```c++
enum FOO { One, Two, Three };
```

After:

```c++
enum pre_foo_post { One, Two, Three };
```

::: option
EnumConstantCase

When defined, the check will ensure enumeration constant names conform
to the selected casing.
:::

::: option
EnumConstantPrefix

When defined, the check will ensure enumeration constant names will add
the prefixed with the given value (regardless of casing).
:::

::: option
EnumConstantIgnoredRegexp

Identifier naming checks won\'t be enforced for enumeration constant
names matching this regular expression.
:::

::: option
EnumConstantSuffix

When defined, the check will ensure enumeration constant names will add
the suffix with the given value (regardless of casing).
:::

::: option
EnumConstantHungarianPrefix

When enabled, the check ensures that the declared identifier will have a
Hungarian notation prefix based on the declared type.
:::

For example using values of:

> - EnumConstantCase of `lower_case`
> - EnumConstantPrefix of `pre_`
> - EnumConstantSuffix of `_post`
> - EnumConstantHungarianPrefix of `On`

Identifies and/or transforms enumeration constant names as follows:

Before:

```c++
enum FOO { One, Two, Three };
```

After:

```c++
enum FOO { pre_One_post, pre_Two_post, pre_Three_post };
```

::: option
FunctionCase

When defined, the check will ensure function names conform to the
selected casing.
:::

::: option
FunctionPrefix

When defined, the check will ensure function names will add the prefixed
with the given value (regardless of casing).
:::

::: option
FunctionIgnoredRegexp

Identifier naming checks won\'t be enforced for function names matching
this regular expression.
:::

::: option
FunctionSuffix

When defined, the check will ensure function names will add the suffix
with the given value (regardless of casing).
:::

For example using values of:

> - FunctionCase of `lower_case`
> - FunctionPrefix of `pre_`
> - FunctionSuffix of `_post`

Identifies and/or transforms function names as follows:

Before:

```c++
char MY_Function_string();
```

After:

```c++
char pre_my_function_string_post();
```

::: option
GetConfigPerFile

When [true]{.title-ref} the check will look for the configuration for
where an identifier is declared. Useful for when included header files
use a different style. Default value is [true]{.title-ref}.
:::

::: option
GlobalConstantCase

When defined, the check will ensure global constant names conform to the
selected casing.
:::

::: option
GlobalConstantPrefix

When defined, the check will ensure global constant names will add the
prefixed with the given value (regardless of casing).
:::

::: option
GlobalConstantIgnoredRegexp

Identifier naming checks won\'t be enforced for global constant names
matching this regular expression.
:::

::: option
GlobalConstantSuffix

When defined, the check will ensure global constant names will add the
suffix with the given value (regardless of casing).
:::

::: option
GlobalConstantHungarianPrefix

When enabled, the check ensures that the declared identifier will have a
Hungarian notation prefix based on the declared type.
:::

For example using values of:

> - GlobalConstantCase of `lower_case`
> - GlobalConstantPrefix of `pre_`
> - GlobalConstantSuffix of `_post`
> - GlobalConstantHungarianPrefix of `On`

Identifies and/or transforms global constant names as follows:

Before:

```c++
unsigned const MyConstGlobal_array[] = {1, 2, 3};
```

After:

```c++
unsigned const pre_myconstglobal_array_post[] = {1, 2, 3};
```

::: option
GlobalConstantPointerCase

When defined, the check will ensure global constant pointer names
conform to the selected casing.
:::

::: option
GlobalConstantPointerPrefix

When defined, the check will ensure global constant pointer names will
add the prefixed with the given value (regardless of casing).
:::

::: option
GlobalConstantPointerIgnoredRegexp

Identifier naming checks won\'t be enforced for global constant pointer
names matching this regular expression.
:::

::: option
GlobalConstantPointerSuffix

When defined, the check will ensure global constant pointer names will
add the suffix with the given value (regardless of casing).
:::

::: option
GlobalConstantPointerHungarianPrefix

When enabled, the check ensures that the declared identifier will have a
Hungarian notation prefix based on the declared type.
:::

For example using values of:

> - GlobalConstantPointerCase of `lower_case`
> - GlobalConstantPointerPrefix of `pre_`
> - GlobalConstantPointerSuffix of `_post`
> - GlobalConstantPointerHungarianPrefix of `On`

Identifies and/or transforms global constant pointer names as follows:

Before:

```c++
int *const MyConstantGlobalPointer = nullptr;
```

After:

```c++
int *const pre_myconstantglobalpointer_post = nullptr;
```

::: option
GlobalFunctionCase

When defined, the check will ensure global function names conform to the
selected casing.
:::

::: option
GlobalFunctionPrefix

When defined, the check will ensure global function names will add the
prefixed with the given value (regardless of casing).
:::

::: option
GlobalFunctionIgnoredRegexp

Identifier naming checks won\'t be enforced for global function names
matching this regular expression.
:::

::: option
GlobalFunctionSuffix

When defined, the check will ensure global function names will add the
suffix with the given value (regardless of casing).
:::

For example using values of:

> - GlobalFunctionCase of `lower_case`
> - GlobalFunctionPrefix of `pre_`
> - GlobalFunctionSuffix of `_post`

Identifies and/or transforms global function names as follows:

Before:

```c++
void GLOBAL_FUNCTION(int PARAMETER_1, int const CONST_parameter);
```

After:

```c++
void pre_global_function_post(int PARAMETER_1, int const CONST_parameter);
```

::: option
GlobalPointerCase

When defined, the check will ensure global pointer names conform to the
selected casing.
:::

::: option
GlobalPointerPrefix

When defined, the check will ensure global pointer names will add the
prefixed with the given value (regardless of casing).
:::

::: option
GlobalPointerIgnoredRegexp

Identifier naming checks won\'t be enforced for global pointer names
matching this regular expression.
:::

::: option
GlobalPointerSuffix

When defined, the check will ensure global pointer names will add the
suffix with the given value (regardless of casing).
:::

::: option
GlobalPointerHungarianPrefix

When enabled, the check ensures that the declared identifier will have a
Hungarian notation prefix based on the declared type.
:::

For example using values of:

> - GlobalPointerCase of `lower_case`
> - GlobalPointerPrefix of `pre_`
> - GlobalPointerSuffix of `_post`
> - GlobalPointerHungarianPrefix of `On`

Identifies and/or transforms global pointer names as follows:

Before:

```c++
int *GLOBAL3;
```

After:

```c++
int *pre_global3_post;
```

::: option
GlobalVariableCase

When defined, the check will ensure global variable names conform to the
selected casing.
:::

::: option
GlobalVariablePrefix

When defined, the check will ensure global variable names will add the
prefixed with the given value (regardless of casing).
:::

::: option
GlobalVariableIgnoredRegexp

Identifier naming checks won\'t be enforced for global variable names
matching this regular expression.
:::

::: option
GlobalVariableSuffix

When defined, the check will ensure global variable names will add the
suffix with the given value (regardless of casing).
:::

::: option
GlobalVariableHungarianPrefix

When enabled, the check ensures that the declared identifier will have a
Hungarian notation prefix based on the declared type.
:::

For example using values of:

> - GlobalVariableCase of `lower_case`
> - GlobalVariablePrefix of `pre_`
> - GlobalVariableSuffix of `_post`
> - GlobalVariableHungarianPrefix of `On`

Identifies and/or transforms global variable names as follows:

Before:

```c++
int GLOBAL3;
```

After:

```c++
int pre_global3_post;
```

::: option
IgnoreMainLikeFunctions

When set to [true]{.title-ref} functions that have a similar signature
to `main` or `wmain` won\'t enforce checks on the names of their
parameters. Default value is [false]{.title-ref}.
:::

::: option
InlineNamespaceCase

When defined, the check will ensure inline namespaces names conform to
the selected casing.
:::

::: option
InlineNamespacePrefix

When defined, the check will ensure inline namespaces names will add the
prefixed with the given value (regardless of casing).
:::

::: option
InlineNamespaceIgnoredRegexp

Identifier naming checks won\'t be enforced for inline namespaces names
matching this regular expression.
:::

::: option
InlineNamespaceSuffix

When defined, the check will ensure inline namespaces names will add the
suffix with the given value (regardless of casing).
:::

For example using values of:

> - InlineNamespaceCase of `lower_case`
> - InlineNamespacePrefix of `pre_`
> - InlineNamespaceSuffix of `_post`

Identifies and/or transforms inline namespaces names as follows:

Before:

```c++
namespace FOO_NS {
inline namespace InlineNamespace {
...
}
} // namespace FOO_NS
```

After:

```c++
namespace FOO_NS {
inline namespace pre_inlinenamespace_post {
...
}
} // namespace FOO_NS
```

::: option
LocalConstantCase

When defined, the check will ensure local constant names conform to the
selected casing.
:::

::: option
LocalConstantPrefix

When defined, the check will ensure local constant names will add the
prefixed with the given value (regardless of casing).
:::

::: option
LocalConstantIgnoredRegexp

Identifier naming checks won\'t be enforced for local constant names
matching this regular expression.
:::

::: option
LocalConstantSuffix

When defined, the check will ensure local constant names will add the
suffix with the given value (regardless of casing).
:::

::: option
LocalConstantHungarianPrefix

When enabled, the check ensures that the declared identifier will have a
Hungarian notation prefix based on the declared type.
:::

For example using values of:

> - LocalConstantCase of `lower_case`
> - LocalConstantPrefix of `pre_`
> - LocalConstantSuffix of `_post`
> - LocalConstantHungarianPrefix of `On`

Identifies and/or transforms local constant names as follows:

Before:

```c++
void foo() { int const local_Constant = 3; }
```

After:

```c++
void foo() { int const pre_local_constant_post = 3; }
```

::: option
LocalConstantPointerCase

When defined, the check will ensure local constant pointer names conform
to the selected casing.
:::

::: option
LocalConstantPointerPrefix

When defined, the check will ensure local constant pointer names will
add the prefixed with the given value (regardless of casing).
:::

::: option
LocalConstantPointerIgnoredRegexp

Identifier naming checks won\'t be enforced for local constant pointer
names matching this regular expression.
:::

::: option
LocalConstantPointerSuffix

When defined, the check will ensure local constant pointer names will
add the suffix with the given value (regardless of casing).
:::

::: option
LocalConstantPointerHungarianPrefix

When enabled, the check ensures that the declared identifier will have a
Hungarian notation prefix based on the declared type.
:::

For example using values of:

> - LocalConstantPointerCase of `lower_case`
> - LocalConstantPointerPrefix of `pre_`
> - LocalConstantPointerSuffix of `_post`
> - LocalConstantPointerHungarianPrefix of `On`

Identifies and/or transforms local constant pointer names as follows:

Before:

```c++
void foo() { int const *local_Constant = 3; }
```

After:

```c++
void foo() { int const *pre_local_constant_post = 3; }
```

::: option
LocalPointerCase

When defined, the check will ensure local pointer names conform to the
selected casing.
:::

::: option
LocalPointerPrefix

When defined, the check will ensure local pointer names will add the
prefixed with the given value (regardless of casing).
:::

::: option
LocalPointerIgnoredRegexp

Identifier naming checks won\'t be enforced for local pointer names
matching this regular expression.
:::

::: option
LocalPointerSuffix

When defined, the check will ensure local pointer names will add the
suffix with the given value (regardless of casing).
:::

::: option
LocalPointerHungarianPrefix

When enabled, the check ensures that the declared identifier will have a
Hungarian notation prefix based on the declared type.
:::

For example using values of:

> - LocalPointerCase of `lower_case`
> - LocalPointerPrefix of `pre_`
> - LocalPointerSuffix of `_post`
> - LocalPointerHungarianPrefix of `On`

Identifies and/or transforms local pointer names as follows:

Before:

```c++
void foo() { int *local_Constant; }
```

After:

```c++
void foo() { int *pre_local_constant_post; }
```

::: option
LocalVariableCase

When defined, the check will ensure local variable names conform to the
selected casing.
:::

::: option
LocalVariablePrefix

When defined, the check will ensure local variable names will add the
prefixed with the given value (regardless of casing).
:::

::: option
LocalVariableIgnoredRegexp

Identifier naming checks won\'t be enforced for local variable names
matching this regular expression.
:::

For example using values of:

> - LocalVariableCase of `CamelCase`
> - LocalVariableIgnoredRegexp of `\w{1,2}`

Will exclude variables with a length less than or equal to 2 from the
camel case check applied to other variables.

::: option
LocalVariableSuffix

When defined, the check will ensure local variable names will add the
suffix with the given value (regardless of casing).
:::

::: option
LocalVariableHungarianPrefix

When enabled, the check ensures that the declared identifier will have a
Hungarian notation prefix based on the declared type.
:::

For example using values of:

> - LocalVariableCase of `lower_case`
> - LocalVariablePrefix of `pre_`
> - LocalVariableSuffix of `_post`
> - LocalVariableHungarianPrefix of `On`

Identifies and/or transforms local variable names as follows:

Before:

```c++
void foo() { int local_Constant; }
```

After:

```c++
void foo() { int pre_local_constant_post; }
```

::: option
MacroDefinitionCase

When defined, the check will ensure macro definitions conform to the
selected casing.
:::

::: option
MacroDefinitionPrefix

When defined, the check will ensure macro definitions will add the
prefixed with the given value (regardless of casing).
:::

::: option
MacroDefinitionIgnoredRegexp

Identifier naming checks won\'t be enforced for macro definitions
matching this regular expression.
:::

::: option
MacroDefinitionSuffix

When defined, the check will ensure macro definitions will add the
suffix with the given value (regardless of casing).
:::

For example using values of:

> - MacroDefinitionCase of `lower_case`
> - MacroDefinitionPrefix of `pre_`
> - MacroDefinitionSuffix of `_post`

Identifies and/or transforms macro definitions as follows:

Before:

```c
#define MY_MacroDefinition
```

After:

```c
#define pre_my_macro_definition_post
```

Note: This will not warn on builtin macros or macros defined on the
command line using the `-D` flag.

::: option
MemberCase

When defined, the check will ensure member names conform to the selected
casing.
:::

::: option
MemberPrefix

When defined, the check will ensure member names will add the prefixed
with the given value (regardless of casing).
:::

::: option
MemberIgnoredRegexp

Identifier naming checks won\'t be enforced for member names matching
this regular expression.
:::

::: option
MemberSuffix

When defined, the check will ensure member names will add the suffix
with the given value (regardless of casing).
:::

::: option
MemberHungarianPrefix

When enabled, the check ensures that the declared identifier will have a
Hungarian notation prefix based on the declared type.
:::

For example using values of:

> - MemberCase of `lower_case`
> - MemberPrefix of `pre_`
> - MemberSuffix of `_post`
> - MemberHungarianPrefix of `On`

Identifies and/or transforms member names as follows:

Before:

```c++
class Foo {
  char MY_ConstMember_string[4];
}
```

After:

```c++
class Foo {
  char pre_my_constmember_string_post[4];
}
```

::: option
MethodCase

When defined, the check will ensure method names conform to the selected
casing.
:::

::: option
MethodPrefix

When defined, the check will ensure method names will add the prefixed
with the given value (regardless of casing).
:::

::: option
MethodIgnoredRegexp

Identifier naming checks won\'t be enforced for method names matching
this regular expression.
:::

::: option
MethodSuffix

When defined, the check will ensure method names will add the suffix
with the given value (regardless of casing).
:::

For example using values of:

> - MethodCase of `lower_case`
> - MethodPrefix of `pre_`
> - MethodSuffix of `_post`

Identifies and/or transforms method names as follows:

Before:

```c++
class Foo {
  char MY_Method_string();
}
```

After:

```c++
class Foo {
  char pre_my_method_string_post();
}
```

::: option
NamespaceCase

When defined, the check will ensure namespace names conform to the
selected casing.
:::

::: option
NamespacePrefix

When defined, the check will ensure namespace names will add the
prefixed with the given value (regardless of casing).
:::

::: option
NamespaceIgnoredRegexp

Identifier naming checks won\'t be enforced for namespace names matching
this regular expression.
:::

::: option
NamespaceSuffix

When defined, the check will ensure namespace names will add the suffix
with the given value (regardless of casing).
:::

For example using values of:

> - NamespaceCase of `lower_case`
> - NamespacePrefix of `pre_`
> - NamespaceSuffix of `_post`

Identifies and/or transforms namespace names as follows:

Before:

```c++
namespace FOO_NS {
...
}
```

After:

```c++
namespace pre_foo_ns_post {
...
}
```

::: option
ParameterCase

When defined, the check will ensure parameter names conform to the
selected casing.
:::

::: option
ParameterPrefix

When defined, the check will ensure parameter names will add the
prefixed with the given value (regardless of casing).
:::

::: option
ParameterIgnoredRegexp

Identifier naming checks won\'t be enforced for parameter names matching
this regular expression.
:::

::: option
ParameterSuffix

When defined, the check will ensure parameter names will add the suffix
with the given value (regardless of casing).
:::

::: option
ParameterHungarianPrefix

When enabled, the check ensures that the declared identifier will have a
Hungarian notation prefix based on the declared type.
:::

For example using values of:

> - ParameterCase of `lower_case`
> - ParameterPrefix of `pre_`
> - ParameterSuffix of `_post`
> - ParameterHungarianPrefix of `On`

Identifies and/or transforms parameter names as follows:

Before:

```c++
void GLOBAL_FUNCTION(int PARAMETER_1, int const CONST_parameter);
```

After:

```c++
void GLOBAL_FUNCTION(int pre_parameter_post, int const CONST_parameter);
```

::: option
ParameterPackCase

When defined, the check will ensure parameter pack names conform to the
selected casing.
:::

::: option
ParameterPackPrefix

When defined, the check will ensure parameter pack names will add the
prefixed with the given value (regardless of casing).
:::

::: option
ParameterPackIgnoredRegexp

Identifier naming checks won\'t be enforced for parameter pack names
matching this regular expression.
:::

::: option
ParameterPackSuffix

When defined, the check will ensure parameter pack names will add the
suffix with the given value (regardless of casing).
:::

For example using values of:

> - ParameterPackCase of `lower_case`
> - ParameterPackPrefix of `pre_`
> - ParameterPackSuffix of `_post`

Identifies and/or transforms parameter pack names as follows:

Before:

```c++
template <typename... TYPE_parameters> {
  void FUNCTION(int... TYPE_parameters);
}
```

After:

```c++
template <typename... TYPE_parameters> {
  void FUNCTION(int... pre_type_parameters_post);
}
```

::: option
PointerParameterCase

When defined, the check will ensure pointer parameter names conform to
the selected casing.
:::

::: option
PointerParameterPrefix

When defined, the check will ensure pointer parameter names will add the
prefixed with the given value (regardless of casing).
:::

::: option
PointerParameterIgnoredRegexp

Identifier naming checks won\'t be enforced for pointer parameter names
matching this regular expression.
:::

::: option
PointerParameterSuffix

When defined, the check will ensure pointer parameter names will add the
suffix with the given value (regardless of casing).
:::

::: option
PointerParameterHungarianPrefix

When enabled, the check ensures that the declared identifier will have a
Hungarian notation prefix based on the declared type.
:::

For example using values of:

> - PointerParameterCase of `lower_case`
> - PointerParameterPrefix of `pre_`
> - PointerParameterSuffix of `_post`
> - PointerParameterHungarianPrefix of `On`

Identifies and/or transforms pointer parameter names as follows:

Before:

```c++
void FUNCTION(int *PARAMETER);
```

After:

```c++
void FUNCTION(int *pre_parameter_post);
```

::: option
PrivateMemberCase

When defined, the check will ensure private member names conform to the
selected casing.
:::

::: option
PrivateMemberPrefix

When defined, the check will ensure private member names will add the
prefixed with the given value (regardless of casing).
:::

::: option
PrivateMemberIgnoredRegexp

Identifier naming checks won\'t be enforced for private member names
matching this regular expression.
:::

::: option
PrivateMemberSuffix

When defined, the check will ensure private member names will add the
suffix with the given value (regardless of casing).
:::

::: option
PrivateMemberHungarianPrefix

When enabled, the check ensures that the declared identifier will have a
Hungarian notation prefix based on the declared type.
:::

For example using values of:

> - PrivateMemberCase of `lower_case`
> - PrivateMemberPrefix of `pre_`
> - PrivateMemberSuffix of `_post`
> - PrivateMemberHungarianPrefix of `On`

Identifies and/or transforms private member names as follows:

Before:

```c++
class Foo {
private:
  int Member_Variable;
}
```

After:

```c++
class Foo {
private:
  int pre_member_variable_post;
}
```

::: option
PrivateMethodCase

When defined, the check will ensure private method names conform to the
selected casing.
:::

::: option
PrivateMethodPrefix

When defined, the check will ensure private method names will add the
prefixed with the given value (regardless of casing).
:::

::: option
PrivateMethodIgnoredRegexp

Identifier naming checks won\'t be enforced for private method names
matching this regular expression.
:::

::: option
PrivateMethodSuffix

When defined, the check will ensure private method names will add the
suffix with the given value (regardless of casing).
:::

For example using values of:

> - PrivateMethodCase of `lower_case`
> - PrivateMethodPrefix of `pre_`
> - PrivateMethodSuffix of `_post`

Identifies and/or transforms private method names as follows:

Before:

```c++
class Foo {
private:
  int Member_Method();
}
```

After:

```c++
class Foo {
private:
  int pre_member_method_post();
}
```

::: option
ProtectedMemberCase

When defined, the check will ensure protected member names conform to
the selected casing.
:::

::: option
ProtectedMemberPrefix

When defined, the check will ensure protected member names will add the
prefixed with the given value (regardless of casing).
:::

::: option
ProtectedMemberIgnoredRegexp

Identifier naming checks won\'t be enforced for protected member names
matching this regular expression.
:::

::: option
ProtectedMemberSuffix

When defined, the check will ensure protected member names will add the
suffix with the given value (regardless of casing).
:::

::: option
ProtectedMemberHungarianPrefix

When enabled, the check ensures that the declared identifier will have a
Hungarian notation prefix based on the declared type.
:::

For example using values of:

> - ProtectedMemberCase of `lower_case`
> - ProtectedMemberPrefix of `pre_`
> - ProtectedMemberSuffix of `_post`
> - ProtectedMemberHungarianPrefix of `On`

Identifies and/or transforms protected member names as follows:

Before:

```c++
class Foo {
protected:
  int Member_Variable;
}
```

After:

```c++
class Foo {
protected:
  int pre_member_variable_post;
}
```

::: option
ProtectedMethodCase

When defined, the check will ensure protected method names conform to
the selected casing.
:::

::: option
ProtectedMethodPrefix

When defined, the check will ensure protected method names will add the
prefixed with the given value (regardless of casing).
:::

::: option
ProtectedMethodIgnoredRegexp

Identifier naming checks won\'t be enforced for protected method names
matching this regular expression.
:::

::: option
ProtectedMethodSuffix

When defined, the check will ensure protected method names will add the
suffix with the given value (regardless of casing).
:::

For example using values of:

> - ProtectedMethodCase of `lower_case`
> - ProtectedMethodPrefix of `pre_`
> - ProtectedMethodSuffix of `_post`

Identifies and/or transforms protect method names as follows:

Before:

```c++
class Foo {
protected:
  int Member_Method();
}
```

After:

```c++
class Foo {
protected:
  int pre_member_method_post();
}
```

::: option
PublicMemberCase

When defined, the check will ensure public member names conform to the
selected casing.
:::

::: option
PublicMemberPrefix

When defined, the check will ensure public member names will add the
prefixed with the given value (regardless of casing).
:::

::: option
PublicMemberIgnoredRegexp

Identifier naming checks won\'t be enforced for public member names
matching this regular expression.
:::

::: option
PublicMemberSuffix

When defined, the check will ensure public member names will add the
suffix with the given value (regardless of casing).
:::

::: option
PublicMemberHungarianPrefix

When enabled, the check ensures that the declared identifier will have a
Hungarian notation prefix based on the declared type.
:::

For example using values of:

> - PublicMemberCase of `lower_case`
> - PublicMemberPrefix of `pre_`
> - PublicMemberSuffix of `_post`
> - PublicMemberHungarianPrefix of `On`

Identifies and/or transforms public member names as follows:

Before:

```c++
class Foo {
public:
  int Member_Variable;
}
```

After:

```c++
class Foo {
public:
  int pre_member_variable_post;
}
```

::: option
PublicMethodCase

When defined, the check will ensure public method names conform to the
selected casing.
:::

::: option
PublicMethodPrefix

When defined, the check will ensure public method names will add the
prefixed with the given value (regardless of casing).
:::

::: option
PublicMethodIgnoredRegexp

Identifier naming checks won\'t be enforced for public method names
matching this regular expression.
:::

::: option
PublicMethodSuffix

When defined, the check will ensure public method names will add the
suffix with the given value (regardless of casing).
:::

For example using values of:

> - PublicMethodCase of `lower_case`
> - PublicMethodPrefix of `pre_`
> - PublicMethodSuffix of `_post`

Identifies and/or transforms public method names as follows:

Before:

```c++
class Foo {
public:
  int Member_Method();
}
```

After:

```c++
class Foo {
public:
  int pre_member_method_post();
}
```

::: option
ScopedEnumConstantCase

When defined, the check will ensure scoped enum constant names conform
to the selected casing.
:::

::: option
ScopedEnumConstantPrefix

When defined, the check will ensure scoped enum constant names will add
the prefixed with the given value (regardless of casing).
:::

::: option
ScopedEnumConstantIgnoredRegexp

Identifier naming checks won\'t be enforced for scoped enum constant
names matching this regular expression.
:::

::: option
ScopedEnumConstantSuffix

When defined, the check will ensure scoped enum constant names will add
the suffix with the given value (regardless of casing).
:::

::: option
ScopedEnumConstantHungarianPrefix

When enabled, the check ensures that the declared identifier will have a
Hungarian notation prefix based on the declared type.
:::

For example using values of:

> - ScopedEnumConstantCase of `lower_case`
> - ScopedEnumConstantPrefix of `pre_`
> - ScopedEnumConstantSuffix of `_post`
> - ScopedEnumConstantHungarianPrefix of `On`

Identifies and/or transforms enumeration constant names as follows:

Before:

```c++
enum class FOO { One, Two, Three };
```

After:

```c++
enum class FOO { pre_One_post, pre_Two_post, pre_Three_post };
```

::: option
StaticConstantCase

When defined, the check will ensure static constant names conform to the
selected casing.
:::

::: option
StaticConstantPrefix

When defined, the check will ensure static constant names will add the
prefixed with the given value (regardless of casing).
:::

::: option
StaticConstantIgnoredRegexp

Identifier naming checks won\'t be enforced for static constant names
matching this regular expression.
:::

::: option
StaticConstantSuffix

When defined, the check will ensure static constant names will add the
suffix with the given value (regardless of casing).
:::

::: option
StaticConstantHungarianPrefix

When enabled, the check ensures that the declared identifier will have a
Hungarian notation prefix based on the declared type.
:::

For example using values of:

> - StaticConstantCase of `lower_case`
> - StaticConstantPrefix of `pre_`
> - StaticConstantSuffix of `_post`
> - StaticConstantHungarianPrefix of `On`

Identifies and/or transforms static constant names as follows:

Before:

```c++
static unsigned const MyConstStatic_array[] = {1, 2, 3};
```

After:

```c++
static unsigned const pre_myconststatic_array_post[] = {1, 2, 3};
```

::: option
StaticVariableCase

When defined, the check will ensure static variable names conform to the
selected casing.
:::

::: option
StaticVariablePrefix

When defined, the check will ensure static variable names will add the
prefixed with the given value (regardless of casing).
:::

::: option
StaticVariableIgnoredRegexp

Identifier naming checks won\'t be enforced for static variable names
matching this regular expression.
:::

::: option
StaticVariableSuffix

When defined, the check will ensure static variable names will add the
suffix with the given value (regardless of casing).
:::

::: option
StaticVariableHungarianPrefix

When enabled, the check ensures that the declared identifier will have a
Hungarian notation prefix based on the declared type.
:::

For example using values of:

> - StaticVariableCase of `lower_case`
> - StaticVariablePrefix of `pre_`
> - StaticVariableSuffix of `_post`
> - StaticVariableHungarianPrefix of `On`

Identifies and/or transforms static variable names as follows:

Before:

```c++
static unsigned MyStatic_array[] = {1, 2, 3};
```

After:

```c++
static unsigned pre_mystatic_array_post[] = {1, 2, 3};
```

::: option
StructCase

When defined, the check will ensure struct names conform to the selected
casing.
:::

::: option
StructPrefix

When defined, the check will ensure struct names will add the prefixed
with the given value (regardless of casing).
:::

::: option
StructIgnoredRegexp

Identifier naming checks won\'t be enforced for struct names matching
this regular expression.
:::

::: option
StructSuffix

When defined, the check will ensure struct names will add the suffix
with the given value (regardless of casing).
:::

For example using values of:

> - StructCase of `lower_case`
> - StructPrefix of `pre_`
> - StructSuffix of `_post`

Identifies and/or transforms struct names as follows:

Before:

```c++
struct FOO {
  FOO();
  ~FOO();
};
```

After:

```c++
struct pre_foo_post {
  pre_foo_post();
  ~pre_foo_post();
};
```

::: option
TemplateParameterCase

When defined, the check will ensure template parameter names conform to
the selected casing.
:::

::: option
TemplateParameterPrefix

When defined, the check will ensure template parameter names will add
the prefixed with the given value (regardless of casing).
:::

::: option
TemplateParameterIgnoredRegexp

Identifier naming checks won\'t be enforced for template parameter names
matching this regular expression.
:::

::: option
TemplateParameterSuffix

When defined, the check will ensure template parameter names will add
the suffix with the given value (regardless of casing).
:::

For example using values of:

> - TemplateParameterCase of `lower_case`
> - TemplateParameterPrefix of `pre_`
> - TemplateParameterSuffix of `_post`

Identifies and/or transforms template parameter names as follows:

Before:

```c++
template <typename T> class Foo {};
```

After:

```c++
template <typename pre_t_post> class Foo {};
```

::: option
TemplateTemplateParameterCase

When defined, the check will ensure template template parameter names
conform to the selected casing.
:::

::: option
TemplateTemplateParameterPrefix

When defined, the check will ensure template template parameter names
will add the prefixed with the given value (regardless of casing).
:::

::: option
TemplateTemplateParameterIgnoredRegexp

Identifier naming checks won\'t be enforced for template template
parameter names matching this regular expression.
:::

::: option
TemplateTemplateParameterSuffix

When defined, the check will ensure template template parameter names
will add the suffix with the given value (regardless of casing).
:::

For example using values of:

> - TemplateTemplateParameterCase of `lower_case`
> - TemplateTemplateParameterPrefix of `pre_`
> - TemplateTemplateParameterSuffix of `_post`

Identifies and/or transforms template template parameter names as
follows:

Before:

```c++
template <template <typename> class TPL_parameter, int COUNT_params,
          typename... TYPE_parameters>
```

After:

```c++
template <template <typename> class pre_tpl_parameter_post, int COUNT_params,
          typename... TYPE_parameters>
```

::: option
TypeAliasCase

When defined, the check will ensure type alias names conform to the
selected casing.
:::

::: option
TypeAliasPrefix

When defined, the check will ensure type alias names will add the
prefixed with the given value (regardless of casing).
:::

::: option
TypeAliasIgnoredRegexp

Identifier naming checks won\'t be enforced for type alias names
matching this regular expression.
:::

::: option
TypeAliasSuffix

When defined, the check will ensure type alias names will add the suffix
with the given value (regardless of casing).
:::

For example using values of:

> - TypeAliasCase of `lower_case`
> - TypeAliasPrefix of `pre_`
> - TypeAliasSuffix of `_post`

Identifies and/or transforms type alias names as follows:

Before:

```c++
using MY_STRUCT_TYPE = my_structure;
```

After:

```c++
using pre_my_struct_type_post = my_structure;
```

::: option
TypedefCase

When defined, the check will ensure typedef names conform to the
selected casing.
:::

::: option
TypedefPrefix

When defined, the check will ensure typedef names will add the prefixed
with the given value (regardless of casing).
:::

::: option
TypedefIgnoredRegexp

Identifier naming checks won\'t be enforced for typedef names matching
this regular expression.
:::

::: option
TypedefSuffix

When defined, the check will ensure typedef names will add the suffix
with the given value (regardless of casing).
:::

For example using values of:

> - TypedefCase of `lower_case`
> - TypedefPrefix of `pre_`
> - TypedefSuffix of `_post`

Identifies and/or transforms typedef names as follows:

Before:

```c++
typedef int MYINT;
```

After:

```c++
typedef int pre_myint_post;
```

::: option
TypeTemplateParameterCase

When defined, the check will ensure type template parameter names
conform to the selected casing.
:::

::: option
TypeTemplateParameterPrefix

When defined, the check will ensure type template parameter names will
add the prefixed with the given value (regardless of casing).
:::

::: option
TypeTemplateParameterIgnoredRegexp

Identifier naming checks won\'t be enforced for type template names
matching this regular expression.
:::

::: option
TypeTemplateParameterSuffix

When defined, the check will ensure type template parameter names will
add the suffix with the given value (regardless of casing).
:::

For example using values of:

> - TypeTemplateParameterCase of `lower_case`
> - TypeTemplateParameterPrefix of `pre_`
> - TypeTemplateParameterSuffix of `_post`

Identifies and/or transforms type template parameter names as follows:

Before:

```c++
template <template <typename> class TPL_parameter, int COUNT_params,
          typename... TYPE_parameters>
```

After:

```c++
template <template <typename> class TPL_parameter, int COUNT_params,
          typename... pre_type_parameters_post>
```

::: option
UnionCase

When defined, the check will ensure union names conform to the selected
casing.
:::

::: option
UnionPrefix

When defined, the check will ensure union names will add the prefixed
with the given value (regardless of casing).
:::

::: option
UnionIgnoredRegexp

Identifier naming checks won\'t be enforced for union names matching
this regular expression.
:::

::: option
UnionSuffix

When defined, the check will ensure union names will add the suffix with
the given value (regardless of casing).
:::

For example using values of:

> - UnionCase of `lower_case`
> - UnionPrefix of `pre_`
> - UnionSuffix of `_post`

Identifies and/or transforms union names as follows:

Before:

```c++
union FOO {
  int a;
  char b;
};
```

After:

```c++
union pre_foo_post {
  int a;
  char b;
};
```

::: option
ValueTemplateParameterCase

When defined, the check will ensure value template parameter names
conform to the selected casing.
:::

::: option
ValueTemplateParameterPrefix

When defined, the check will ensure value template parameter names will
add the prefixed with the given value (regardless of casing).
:::

::: option
ValueTemplateParameterIgnoredRegexp

Identifier naming checks won\'t be enforced for value template parameter
names matching this regular expression.
:::

::: option
ValueTemplateParameterSuffix

When defined, the check will ensure value template parameter names will
add the suffix with the given value (regardless of casing).
:::

For example using values of:

> - ValueTemplateParameterCase of `lower_case`
> - ValueTemplateParameterPrefix of `pre_`
> - ValueTemplateParameterSuffix of `_post`

Identifies and/or transforms value template parameter names as follows:

Before:

```c++
template <template <typename> class TPL_parameter, int COUNT_params,
          typename... TYPE_parameters>
```

After:

```c++
template <template <typename> class TPL_parameter, int pre_count_params_post,
          typename... TYPE_parameters>
```

::: option
VariableCase

When defined, the check will ensure variable names conform to the
selected casing.
:::

::: option
VariablePrefix

When defined, the check will ensure variable names will add the prefixed
with the given value (regardless of casing).
:::

::: option
VariableIgnoredRegexp

Identifier naming checks won\'t be enforced for variable names matching
this regular expression.
:::

::: option
VariableSuffix

When defined, the check will ensure variable names will add the suffix
with the given value (regardless of casing).
:::

::: option
VariableHungarianPrefix

When enabled, the check ensures that the declared identifier will have a
Hungarian notation prefix based on the declared type.
:::

For example using values of:

> - VariableCase of `lower_case`
> - VariablePrefix of `pre_`
> - VariableSuffix of `_post`
> - VariableHungarianPrefix of `On`

Identifies and/or transforms variable names as follows:

Before:

```c++
unsigned MyVariable;
```

After:

```c++
unsigned pre_myvariable_post;
```

::: option
VirtualMethodCase

When defined, the check will ensure virtual method names conform to the
selected casing.
:::

::: option
VirtualMethodPrefix

When defined, the check will ensure virtual method names will add the
prefixed with the given value (regardless of casing).
:::

::: option
VirtualMethodIgnoredRegexp

Identifier naming checks won\'t be enforced for virtual method names
matching this regular expression.
:::

::: option
VirtualMethodSuffix

When defined, the check will ensure virtual method names will add the
suffix with the given value (regardless of casing).
:::

For example using values of:

> - VirtualMethodCase of `lower_case`
> - VirtualMethodPrefix of `pre_`
> - VirtualMethodSuffix of `_post`

Identifies and/or transforms virtual method names as follows:

Before:

```c++
class Foo {
public:
  virtual int MemberFunction();
}
```

After:

```c++
class Foo {
public:
  virtual int pre_member_function_post();
}
```

## The default mapping table of Hungarian Notation

In Hungarian notation, a variable name starts with a group of lower-case
letters which are mnemonics for the type or purpose of that variable,
followed by whatever name the programmer has chosen; this last part is
sometimes distinguished as the given name. The first character of the
given name can be capitalized to separate it from the type indicators
(see also CamelCase). Otherwise the case of this character denotes
scope.

The following table is the default mapping table of Hungarian Notation
which maps Decl to its prefix string. You can also have your own style
in config file.

+----------+----------+----------+----------+----------+----------+
| P | | | | M | |
| rimitive | | | | icrosoft | |
| Type | | | | Type | |
+==========+==========+==========+==========+==========+==========+
| > Type | Prefix | Type | Prefix | Type | Prefix |
+----------+----------+----------+----------+----------+----------+
| = | ====== | ====== | ====== | ====== | ====== |
| ======== | ======== | ======== | ======== | ======== | ======== |
| ======== | | ======== | | | |
+----------+----------+----------+----------+----------+----------+
| int8*t | i8 | signed | si | BOOL | b |
| | | int | | | |
+----------+----------+----------+----------+----------+----------+
| int16_t | i16 | signed | ss | BOOLEAN | b |
| | | short | | | |
+----------+----------+----------+----------+----------+----------+
| int32_t | i32 | signed | ssi | BYTE | by |
| | | short | | | |
| | | int | | | |
+----------+----------+----------+----------+----------+----------+
| int64_t | i64 | signed | slli | CHAR | c |
| | | long | | | |
| | | long int | | | |
+----------+----------+----------+----------+----------+----------+
| uint8_t | u8 | signed | sll | UCHAR | uc |
| | | long | | | |
| | | long | | | |
+----------+----------+----------+----------+----------+----------+
| uint16_t | u16 | signed | sli | SHORT | s |
| | | long int | | | |
+----------+----------+----------+----------+----------+----------+
| uint32_t | u32 | signed | sl | USHORT | us |
| | | long | | | |
+----------+----------+----------+----------+----------+----------+
| uint64_t | u64 | signed | s | WORD | w |
+----------+----------+----------+----------+----------+----------+
| char8_t | c8 | unsigned | ulli | DWORD | dw |
| | | long | | | |
| | | long int | | | |
+----------+----------+----------+----------+----------+----------+
| char16_t | c16 | unsigned | ull | DWORD32 | dw32 |
| | | long | | | |
| | | long | | | |
+----------+----------+----------+----------+----------+----------+
| char32_t | c32 | unsigned | uli | DWORD64 | dw64 |
| | | long int | | | |
+----------+----------+----------+----------+----------+----------+
| float | f | unsigned | ul | LONG | l |
| | | long | | | |
+----------+----------+----------+----------+----------+----------+
| double | d | unsigned | usi | ULONG | ul |
| | | short | | | |
| | | int | | | |
+----------+----------+----------+----------+----------+----------+
| char | c | unsigned | us | ULONG32 | ul32 |
| | | short | | | |
+----------+----------+----------+----------+----------+----------+
| bool | b | unsigned | ui | ULONG64 | ul64 |
| | | int | | | |
+----------+----------+----------+----------+----------+----------+
| \_Bool | b | unsigned | uc | U | ull |
| | | char | | LONGLONG | |
+----------+----------+----------+----------+----------+----------+
| int | i | unsigned | u | HANDLE | h |
+----------+----------+----------+----------+----------+----------+
| size_t | n | long | lli | INT | i |
| | | long int | | | |
+----------+----------+----------+----------+----------+----------+
| short | s | long | ld | INT8 | i8 |
| | | double | | | |
+----------+----------+----------+----------+----------+----------+
| signed | i | long | ll | INT16 | i16 |
| | | long | | | |
+----------+----------+----------+----------+----------+----------+
| unsigned | u | long int | li | INT32 | i32 |
+----------+----------+----------+----------+----------+----------+
| long | l | long | l | INT64 | i64 |
+----------+----------+----------+----------+----------+----------+
| long | ll | p | p | UINT | ui |
| long | | trdiff_t | | | |
+----------+----------+----------+----------+----------+----------+
| unsigned | ul ld p | void | \_none* | UINT8 | u8 u16 |
| long | wc si s | | | UINT16 | u32 u64 |
| long | | | | UINT32 | p |
| double | | | | UINT64 | |
| p | | | | PVOID | |
| trdiff_t | | | | | |
| wchar_t | | | | | |
| short | | | | | |
| int | | | | | |
| short | | | | | |
+----------+----------+----------+----------+----------+----------+

**There are more trivial options for Hungarian Notation:**

**HungarianNotation.General.**\*

: Options are not belonging to any specific Decl.

**HungarianNotation.CString.**\*

: Options for NULL-terminated string.

**HungarianNotation.DerivedType.**\*

: Options for derived types.

**HungarianNotation.PrimitiveType.**\*

: Options for primitive types.

**HungarianNotation.UserDefinedType.**\*

: Options for user-defined types.

## Options for Hungarian Notation

- `HungarianNotation.General.TreatStructAsClass`{.interpreted-text
  role="option"}
- `HungarianNotation.DerivedType.Array`{.interpreted-text
  role="option"}
- `HungarianNotation.DerivedType.Pointer`{.interpreted-text
  role="option"}
- `HungarianNotation.DerivedType.FunctionPointer`{.interpreted-text
  role="option"}
- `HungarianNotation.CString.CharPointer`{.interpreted-text
  role="option"}
- `HungarianNotation.CString.CharArray`{.interpreted-text
  role="option"}
- `HungarianNotation.CString.WideCharPointer`{.interpreted-text
  role="option"}
- `HungarianNotation.CString.WideCharArray`{.interpreted-text
  role="option"}
- `HungarianNotation.PrimitiveType.*`{.interpreted-text role="option"}
- `HungarianNotation.UserDefinedType.*`{.interpreted-text
  role="option"}

::: option
HungarianNotation.General.TreatStructAsClass

When defined, the check will treat naming of struct as a class. The
default value is [false]{.title-ref}.
:::

::: option
HungarianNotation.DerivedType.Array

When defined, the check will ensure variable name will add the prefix
with the given string. The default prefix is [a]{.title-ref}.
:::

::: option
HungarianNotation.DerivedType.Pointer

When defined, the check will ensure variable name will add the prefix
with the given string. The default prefix is [p]{.title-ref}.
:::

::: option
HungarianNotation.DerivedType.FunctionPointer

When defined, the check will ensure variable name will add the prefix
with the given string. The default prefix is [fn]{.title-ref}.
:::

Before:

```c++
// Array
int DataArray[2] = {0};

// Pointer
void *DataBuffer = NULL;

// FunctionPointer
typedef void (*FUNC_PTR)();
FUNC_PTR FuncPtr = NULL;
```

After:

```c++
// Array
int aDataArray[2] = {0};

// Pointer
void *pDataBuffer = NULL;

// FunctionPointer
typedef void (*FUNC_PTR)();
FUNC_PTR fnFuncPtr = NULL;
```

::: option
HungarianNotation.CString.CharPointer

When defined, the check will ensure variable name will add the prefix
with the given string. The default prefix is [sz]{.title-ref}.
:::

::: option
HungarianNotation.CString.CharArray

When defined, the check will ensure variable name will add the prefix
with the given string. The default prefix is [sz]{.title-ref}.
:::

::: option
HungarianNotation.CString.WideCharPointer

When defined, the check will ensure variable name will add the prefix
with the given string. The default prefix is [wsz]{.title-ref}.
:::

::: option
HungarianNotation.CString.WideCharArray

When defined, the check will ensure variable name will add the prefix
with the given string. The default prefix is [wsz]{.title-ref}.
:::

Before:

```c++
// CharPointer
const char *NamePtr = "Name";

// CharArray
const char NameArray[] = "Name";

// WideCharPointer
const wchar_t *WideNamePtr = L"Name";

// WideCharArray
const wchar_t WideNameArray[] = L"Name";
```

After:

```c++
// CharPointer
const char *szNamePtr = "Name";

// CharArray
const char szNameArray[] = "Name";

// WideCharPointer
const wchar_t *wszWideNamePtr = L"Name";

// WideCharArray
const wchar_t wszWideNameArray[] = L"Name";
```

::: option
HungarianNotation.PrimitiveType.\*

When defined, the check will ensure variable name of involved primitive
types will add the prefix with the given string. The default prefixes
are defined in the default mapping table.
:::

::: option
HungarianNotation.UserDefinedType.\*

When defined, the check will ensure variable name of involved primitive
types will add the prefix with the given string. The default prefixes
are defined in the default mapping table.
:::

Before:

```c++
int8_t   ValueI8      = 0;
int16_t  ValueI16     = 0;
int32_t  ValueI32     = 0;
int64_t  ValueI64     = 0;
uint8_t  ValueU8      = 0;
uint16_t ValueU16     = 0;
uint32_t ValueU32     = 0;
uint64_t ValueU64     = 0;
float    ValueFloat   = 0.0;
double   ValueDouble  = 0.0;
ULONG    ValueUlong   = 0;
DWORD    ValueDword   = 0;
```

After:

```c++
int8_t   i8ValueI8    = 0;
int16_t  i16ValueI16  = 0;
int32_t  i32ValueI32  = 0;
int64_t  i64ValueI64  = 0;
uint8_t  u8ValueU8    = 0;
uint16_t u16ValueU16  = 0;
uint32_t u32ValueU32  = 0;
uint64_t u64ValueU64  = 0;
float    fValueFloat  = 0.0;
double   dValueDouble = 0.0;
ULONG    ulValueUlong = 0;
DWORD    dwValueDword = 0;
```
