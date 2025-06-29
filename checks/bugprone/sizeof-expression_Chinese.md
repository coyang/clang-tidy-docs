# bugprone-sizeof-expression

The check finds usages of sizeof expressions which are most likely errors.

该检查会发现可能是错误的 sizeof 表达式用法。

The sizeof operator yields the size (in bytes) of its operand, which may be an expression or the parenthesized name of a type. Misuse of this operator may be leading to errors and possible software vulnerabilities.

sizeof 运算符返回其操作数的大小（以字节为单位），操作数可以是一个表达式或一个带括号的类型名。误用该运算符可能导致错误甚至软件漏洞。

## Suspicious usage of 'sizeof(K)'

对 'sizeof(K)' 的可疑用法

A common mistake is to query the sizeof of an integer literal. This is equivalent to query the size of its type (probably int). The intent of the programmer was probably to simply get the integer and not its size.

一个常见错误是对整数字面量使用 sizeof，这等价于查询其类型（可能是 int）的大小。程序员的本意可能只是想获取该整数，而不是它的大小。

```c++
#define BUFLEN 42
char buf[BUFLEN];
memset(buf, 0, sizeof(BUFLEN));  // sizeof(42) ==> sizeof(int)
```

## Suspicious usage of 'sizeof(expr)'

对 'sizeof(expr)' 的可疑用法

In cases, where there is an enum or integer to represent a type, a common mistake is to query the sizeof on the integer or enum that represents the type that should be used by sizeof. This results in the size of the integer and not of the type the integer represents:

在某些情况下，使用枚举或整数来表示一个类型时，一个常见错误是对该整数或枚举使用 sizeof，而不是对其所代表的类型使用 sizeof。这样会得到整数的大小，而不是其代表的类型的大小：

```c++
enum data_type {
  FLOAT_TYPE,
  DOUBLE_TYPE
};

struct data {
  data_type type;
  void* buffer;
  data_type get_type() {
    return type;
  }
};

void f(data d, int numElements) {
  // should be sizeof(float) or sizeof(double), depending on d.get_type()
  int numBytes = numElements * sizeof(d.get_type());
  ...
}
```

## Suspicious usage of 'sizeof(this)'

对 'sizeof(this)' 的可疑用法

The this keyword is evaluated to a pointer to an object of a given type. The expression sizeof(this) is returning the size of a pointer. The programmer most likely wanted the size of the object and not the size of the pointer.

this 关键字表示指向当前对象的指针。表达式 sizeof(this) 返回的是指针的大小。程序员很可能是想获取对象的大小，而不是指针的大小。

```c++
class Point {
  [...]
  size_t size() { return sizeof(this); }  // should probably be sizeof(*this)
  [...]
};
```

## Suspicious usage of 'sizeof(char\*)'

对 'sizeof(char\*)' 的可疑用法

There is a subtle difference between declaring a string literal with char* A = "" and char A[] = "". The first case has the type char* instead of the aggregate type char[]. Using sizeof on an object declared with char\* type is returning the size of a pointer instead of the number of characters (bytes) in the string literal.

使用 char* A = "" 和 char A[] = "" 声明字符串字面量之间存在细微差别。前者的类型是 char*，而不是聚合类型 char[]。对 char\* 类型的对象使用 sizeof 返回的是指针的大小，而不是字符串字面量中的字符（字节）数量。

```c++
const char* kMessage = "Hello World!";      // const char kMessage[] = "...";
void getMessage(char* buf) {
  memcpy(buf, kMessage, sizeof(kMessage));  // sizeof(char*)
}
```

## Suspicious usage of 'sizeof(A\*)'

对 'sizeof(A\*)' 的可疑用法

A common mistake is to compute the size of a pointer instead of its pointee. These cases may occur because of explicit cast or implicit conversion.

一个常见错误是计算指针的大小，而不是其指向对象的大小。这种情况可能由于显式类型转换或隐式转换导致。

```c++
int A[10];
memset(A, 0, sizeof(A + 0));

struct Point point;
memset(point, 0, sizeof(&point));
```

## Suspicious usage of 'sizeof(...)/sizeof(...)'

对 'sizeof(...)/sizeof(...)' 的可疑用法

Dividing sizeof expressions is typically used to retrieve the number of elements of an aggregate. This check warns on incompatible or suspicious cases.

使用 sizeof 表达式相除通常用于获取聚合类型的元素数量。该检查会对不兼容或可疑的情况发出警告。

In the following example, the entity has 10-bytes and is incompatible with the type int which has 4 bytes.

在以下示例中，实体大小为 10 字节，而 int 类型为 4 字节，两者不兼容。

```c++
char buf[] = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };  // sizeof(buf) => 10
void getMessage(char* dst) {
  memcpy(dst, buf, sizeof(buf) / sizeof(int));  // sizeof(int) => 4  [incompatible sizes]
}
```

In the following example, the expression sizeof(Values) is returning the size of char*. One can easily be fooled by its declaration, but in parameter declaration the size '10' is ignored and the function is receiving a char*.

在以下示例中，表达式 sizeof(Values) 返回的是 char* 的大小。虽然声明中看起来像是数组，但在函数参数中，大小 '10' 被忽略，函数实际接收到的是 char*。

```c++
char OrderedValues[10] = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
return CompareArray(char Values[10]) {
  return memcmp(OrderedValues, Values, sizeof(Values)) == 0;  // sizeof(Values) ==> sizeof(char*) [implicit cast to char*]
}
```

## Suspicious 'sizeof' by 'sizeof' expression

对 'sizeof' \* 'sizeof' 表达式的可疑用法

Multiplying sizeof expressions typically makes no sense and is probably a logic error. In the following example, the programmer used \* instead of /.

将 sizeof 表达式相乘通常没有意义，很可能是逻辑错误。在以下示例中，程序员使用了 \* 而不是 /。

```c++
const char kMessage[] = "Hello World!";
void getMessage(char* buf) {
  memcpy(buf, kMessage, sizeof(kMessage) * sizeof(char));  //  sizeof(kMessage) / sizeof(char)
}
```

This check may trigger on code using the arraysize macro. The following code is working correctly but should be simplified by using only the sizeof operator.

该检查可能会在使用 arraysize 宏的代码中触发。以下代码虽然功能正确，但应简化为仅使用 sizeof 运算符。

```c++
extern Object objects[100];
void InitializeObjects() {
  memset(objects, 0, arraysize(objects) * sizeof(Object));  // sizeof(objects)
}
```

## Suspicious usage of 'sizeof(sizeof(...))'

对 'sizeof(sizeof(...))' 的可疑用法

Getting the sizeof of a sizeof makes no sense and is typically an error hidden through macros.

对 sizeof 的结果再次使用 sizeof 没有意义，通常是通过宏隐藏的错误。

```c++
#define INT_SZ sizeof(int)
int buf[] = { 42 };
void getInt(int* dst) {
  memcpy(dst, buf, sizeof(INT_SZ));  // sizeof(sizeof(int)) is suspicious.
}
```

## Suspicious usages of 'sizeof(...)' in pointer arithmetic

在指针运算中对 'sizeof(...)' 的可疑用法

Arithmetic operators on pointers automatically scale the result with the size of the pointed typed. Further use of sizeof around pointer arithmetic will typically result in an unintended result.

指针上的算术运算会自动按所指类型的大小进行缩放。在指针运算中再使用 sizeof 通常会导致非预期的结果。

### Scaling the result of pointer difference

对指针差值进行缩放

Subtracting two pointers results in an integer expression (of type ptrdiff_t) which expresses the distance between the two pointed objects in "number of objects between". A common mistake is to think that the result is "number of bytes between", and scale the difference with sizeof, such as P1 - P2 == N \* sizeof(T) (instead of P1 - P2 == N) or (P1 - P2) / sizeof(T) instead of P1 - P2.

两个指针相减会得到一个整数表达式（类型为 ptrdiff_t），表示两个对象之间的“对象数量”。一个常见错误是误以为结果是“字节数”，并用 sizeof 对差值进行缩放，如 P1 - P2 == N \* sizeof(T)（应为 P1 - P2 == N）或 (P1 - P2) / sizeof(T)（应为 P1 - P2）。

```c++
void splitFour(const Obj* Objs, size_t N, Obj Delimiter) {
  const Obj *P = Objs;
  while (P < Objs + N) {
    if (*P == Delimiter) {
      break;
    }
  }

  if (P - Objs != 4 * sizeof(Obj)) { // Expecting a distance multiplied by sizeof is suspicious.
    error();
  }
}
```

```c++
void iterateIfEvenLength(int *Begin, int *End) {
  auto N = (Begin - End) / sizeof(int); // Dividing by sizeof() is suspicious.
  if (N % 2)
    return;

  // ...
}
```

### Stepping a pointer with a scaled integer

使用缩放后的整数进行指针步进

Conversely, when performing pointer arithmetics to add or subtract from a pointer, the arithmetic operator implicitly scales the value actually added to the pointer with the size of the pointee, as Ptr + N expects N to be "number of objects to step", and not "number of bytes to step".

相反地，当对指针进行加减运算时，算术运算符会隐式地将加到指针上的值按所指对象的大小进行缩放。Ptr + N 期望 N 是“要步进的对象数量”，而不是“要步进的字节数”。

Seeing the calculation of a pointer where sizeof appears is suspicious, and the result is typically unintended, often out of bounds. Ptr + sizeof(T) will offset the pointer by sizeof(T) elements, effectively exponentiating the scaling factor to the power of 2.

在指针计算中出现 sizeof 是可疑的，结果通常不是预期的，甚至可能越界。Ptr + sizeof(T) 会将指针偏移 sizeof(T) 个元素，相当于将缩放因子平方。

Similarly, multiplying or dividing a numeric value with the sizeof of an element or the whole buffer is suspicious, because the dimensional connection between the numeric value and the actual sizeof result can not always be deduced. While scaling an integer up (multiplying) with sizeof is likely always an issue, a scaling down (division) is not always inherently dangerous, in case the developer is aware that the division happens between an appropriate number of bytes and a sizeof value. Turning WarnOnOffsetDividedBySizeOf off will restrict the warnings to the multiplication case.

类似地，将数值与元素或整个缓冲区的 sizeof 相乘或相除也是可疑的，因为数值与 sizeof 结果之间的维度关系并不总是明确。虽然用 sizeof 乘以整数几乎总是有问题，但如果开发者清楚地知道除法是在字节数与 sizeof 值之间进行的，除法并不总是危险的。关闭 WarnOnOffsetDividedBySizeOf 选项将仅对乘法情况发出警告。

This case also checks suspicious alignof and offsetof usages in pointer arithmetic, as both return the "size" in bytes and not elements, potentially resulting in doubly-scaled offsets.

该检查还会检测在指针运算中对 alignof 和 offsetof 的可疑用法，因为它们返回的是字节大小而不是元素数量，可能导致偏移量被重复缩放。

```c++
void printEveryEvenIndexElement(int *Array, size_t N) {
  int *P = Array;
  while (P <= Array + N * sizeof(int)) { // Suspicious pointer arithmetic using sizeof()!
    printf("%d ", *P);

    P += 2 * sizeof(int); // Suspicious pointer arithmetic using sizeof()!
  }
}
```

```c++
struct Message { /* ... */; char Flags[8]; };
void clearFlags(Message *Array, size_t N) {
  const Message *End = Array + N;
  while (Array < End) {
    memset(Array + offsetof(Message, Flags), // Suspicious pointer arithmetic using offsetof()!
           0, sizeof(Message::Flags));
    ++Array;
  }
}
```

For this checked bogus pattern, cert-arr39-c redirects here as an alias of this check.

对于这种被检查的错误模式，cert-arr39-c 会作为该检查的别名重定向到此处。

This check corresponds to the CERT C Coding Standard rule ARR39-C. Do not add or subtract a scaled integer to a pointer.

该检查对应 CERT C 编码标准规则 ARR39-C：不要将缩放后的整数加到指针上或从指针中减去。

#### Limitations

限制

Cases where the pointee type has a size of 1 byte (such as, and most importantly, char) are excluded.

指向类型大小为 1 字节的情况（如 char，最常见）将被排除在检查之外。

## Options

选项

::: option
WarnOnSizeOfConstant

When true, the check will warn on an expression like sizeof(CONSTANT). Default is true.

当为 true 时，该检查会对类似 sizeof(CONSTANT) 的表达式发出警告。默认值为 true。
:::

::: option
WarnOnSizeOfIntegerExpression

When true, the check will warn on an expression like sizeof(expr) where the expression results in an integer. Default is false.

当为 true 时，该检查会对表达式为整数的 sizeof(expr) 发出警告。默认值为 false。
:::

::: option
WarnOnSizeOfThis

When true, the check will warn on an expression like sizeof(this). Default is true.

当为 true 时，该检查会对类似 sizeof(this) 的表达式发出警告。默认值为 true。
:::

::: option
WarnOnSizeOfCompareToConstant

When true, the check will warn on an expression like sizeof(expr) <= k for a suspicious constant k while k is 0 or greater than 0x8000. Default is true.

当为 true 时，该检查会对类似 sizeof(expr) <= k 的表达式发出警告，若常量 k 为 0 或大于 0x8000。默认值为 true。
:::

::: option
WarnOnSizeOfPointerToAggregate

When true, the check will warn when the argument of sizeof is either a pointer-to-aggregate type, an expression returning a pointer-to-aggregate value or an expression that returns a pointer from an array-to-pointer conversion (that may be implicit or explicit, for example array + 2 or (int \*)array). Default is true.

当为 true 时，该检查会在 sizeof 的参数是聚合类型指针、返回聚合类型指针的表达式，或从数组到指针的转换表达式（如 array + 2 或 (int \*)array）时发出警告。默认值为 true。
:::

::: option
WarnOnSizeOfPointer

When true, the check will report all expressions where the argument of sizeof is an expression that produces a pointer (except for a few idiomatic expressions that are probably intentional and correct). This detects occurrences of CWE 467. Default is false.

当为 true 时，该检查会报告所有 sizeof 的参数为指针表达式的情况（除了一些可能是有意且正确的惯用表达式）。这可用于检测 CWE 467。默认值为 false。
:::

::: option
WarnOnOffsetDividedBySizeOf

When true, the check will warn on pointer arithmetic where the element count is obtained from a division with sizeof(...), e.g., Ptr + Bytes / sizeof(\*T). Default is true.

当为 true 时，该检查会对通过 sizeof(...) 除法获得元素数量的指针运算发出警告，例如 Ptr + Bytes / sizeof(\*T)。默认值为 true。
:::

::: option
WarnOnSizeOfInLoopTermination

When true, the check will warn about incorrect use of sizeof expression in loop termination condition. The warning triggers if the sizeof expression appears to be incorrectly used to determine the number of array/buffer elements. e.g, long arr[10]; for(int i = 0; i < sizeof(arr); i++) { ... }. Default is true.

当为 true 时，该检查会对在循环终止条件中错误使用 sizeof 表达式的情况发出警告。如果 sizeof 表达式被错误地用于确定数组/缓冲区元素数量，则会触发警告。例如：long arr[10]; for(int i = 0; i < sizeof(arr); i++) { ... }。默认值为 true。
:::
