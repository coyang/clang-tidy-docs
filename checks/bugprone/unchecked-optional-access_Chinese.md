# bugprone-unchecked-optional-access

_Note_: This check uses a flow-sensitive static analysis to produce its results. Therefore, it may be more resource intensive (RAM, CPU) than the average clang-tidy check.

注意：此检查使用流敏感的静态分析来生成结果。因此，它可能比普通的 clang-tidy 检查更消耗资源（内存、CPU）。

This check identifies unsafe accesses to values contained in std::optional<T>, absl::optional<T>, base::Optional<T>, folly::Optional<T>, bsl::optional, or BloombergLP::bdlb::NullableValue objects. Below we will refer to all these types collectively as optional<T>.

此检查用于识别对 std::optional<T>、absl::optional<T>、base::Optional<T>、folly::Optional<T>、bsl::optional 或 BloombergLP::bdlb::NullableValue 对象中值的不安全访问。下文中我们统称这些类型为 optional<T>。

An access to the value of an optional<T> occurs when one of its value, operator\*, or operator-> member functions is invoked. To align with common misconceptions, the check considers these member functions as equivalent, even though there are subtle differences related to exceptions versus undefined behavior. See Additional notes, below, for more information on this topic.

当调用 optional<T> 的 value、operator\* 或 operator-> 成员函数之一时，就发生了对其值的访问。为了符合常见的误解，该检查将这些成员函数视为等价，尽管它们在异常处理与未定义行为方面存在细微差别。有关此主题的更多信息，请参见下文的“附加说明”。

An access to the value of an optional<T> is considered safe if and only if code in the local scope (for example, a function body) ensures that the optional<T> has a value in all possible execution paths that can reach the access. That should happen either through an explicit check, using the optional<T>::has_value member function, or by constructing the optional<T> in a way that shows that it unambiguously holds a value (e.g using std::make_optional which always returns a populated std::optional<T>).

只有当局部作用域（例如函数体）中的代码确保在所有可能到达该访问点的执行路径中 optional<T> 都具有值时，对 optional<T> 值的访问才被视为安全。这应通过显式检查（使用 optional<T>::has_value 成员函数）或以明确持有值的方式构造 optional<T>（例如使用 std::make_optional，它总是返回一个已填充的 std::optional<T>）来实现。

Below we list some examples, starting with unsafe optional access patterns, followed by safe access patterns.

下面列出了一些示例，首先是对 optional 的不安全访问模式，然后是安全访问模式。

## Unsafe access patterns

## 不安全的访问模式

### Access the value without checking if it exists

### 未检查是否存在即访问值

The check flags accesses to the value that are not locally guarded by existence check:

该检查会标记那些未在本地通过存在性检查保护的值访问：

```c++
void f(std::optional<int> opt) {
  use(*opt); // unsafe: it is unclear whether `opt` has a value.
             // 不安全：不清楚 `opt` 是否有值。
}
```

### Access the value in the wrong branch

### 在错误的分支中访问值

The check is aware of the state of an optional object in different branches of the code. For example:

该检查能够识别代码中不同分支中 optional 对象的状态。例如：

```c++
void f(std::optional<int> opt) {
  if (opt.has_value()) {
  } else {
    use(opt.value()); // unsafe: it is clear that `opt` does *not* have a value.
                      // 不安全：明确知道 `opt` 没有值。
  }
}
```

### Assume a function result to be stable

### 假设函数结果是稳定的

The check is aware that function results might not be stable. That is, consecutive calls to the same function might return different values. For example:

该检查知道函数结果可能不稳定。也就是说，对同一函数的连续调用可能返回不同的值。例如：

```c++
void f(Foo foo) {
  if (foo.take().has_value()) {
    use(*foo.take()); // unsafe: it is unclear whether `foo.take()` has a value.
                      // 不安全：不清楚 `foo.take()` 是否有值。
  }
}
```

#### Exception: accessor methods

#### 例外：访问器方法

The check assumes accessor methods of a class are stable, with a heuristic to determine which methods are accessors. Specifically, parameter-free const methods and smart pointer-like APIs (non const overloads of \* when there is a parallel const overload) are treated as accessors. Note that this is not guaranteed to be safe -- but, it is widely used (safely) in practice. Calls to non const methods are assumed to modify the state of the object and affect the stability of earlier accessor calls.

该检查假设类的访问器方法是稳定的，并使用启发式方法来判断哪些方法是访问器。具体来说，无参数的 const 方法和类似智能指针的 API（当存在对应的 const 重载时的非 const 的 \* 重载）被视为访问器。请注意，这并不能保证绝对安全——但在实践中被广泛（且安全地）使用。调用非 const 方法被认为会修改对象状态，并影响先前访问器调用的稳定性。

### Rely on invariants of uncommon APIs

### 依赖不常见 API 的不变量

The check is unaware of invariants of uncommon APIs. For example:

该检查无法识别不常见 API 的不变量。例如：

```c++
void f(Foo foo) {
  if (foo.HasProperty("bar")) {
    use(*foo.GetProperty("bar")); // unsafe: it is unclear whether `foo.GetProperty("bar")` has a value.
                                  // 不安全：不清楚 `foo.GetProperty("bar")` 是否有值。
  }
}
```

### Check if a value exists, then pass the optional to another function

### 检查值是否存在后将 optional 传递给另一个函数

The check relies on local reasoning. The check and value access must both happen in the same function. An access is considered unsafe even if the caller of the function performing the access ensures that the optional has a value. For example:

该检查依赖于局部推理。检查和访问值必须发生在同一个函数中。即使调用该函数的代码已确保 optional 有值，该访问仍被视为不安全。例如：

```c++
void g(std::optional<int> opt) {
  use(*opt); // unsafe: it is unclear whether `opt` has a value.
             // 不安全：不清楚 `opt` 是否有值。
}

void f(std::optional<int> opt) {
  if (opt.has_value()) {
    g(opt);
  }
}
```

## Safe access patterns

## 安全的访问模式

### Check if a value exists, then access the value

### 检查值是否存在后再访问值

The check recognizes all straightforward ways for checking if a value exists and accessing the value contained in an optional object. For example:

该检查可以识别所有直接的方式来检查值是否存在并访问 optional 对象中的值。例如：

```c++
void f(std::optional<int> opt) {
  if (opt.has_value()) {
    use(*opt);
  }
}
```

### Check if a value exists, then access the value from a copy

### 检查值是否存在后从副本访问值

The criteria that the check uses is semantic, not syntactic. It recognizes when a copy of the optional object being accessed is known to have a value. For example:

该检查依据语义而非语法来判断。它能识别出被访问的 optional 对象副本是否已知具有值。例如：

```c++
void f(std::optional<int> opt1) {
  if (opt1.has_value()) {
    std::optional<int> opt2 = opt1;
    use(*opt2);
  }
}
```

### Ensure that a value exists using common macros

### 使用常见宏确保值存在

The check is aware of common macros like CHECK and DCHECK. Those can be used to ensure that an optional object has a value. For example:

该检查识别常见的宏，如 CHECK 和 DCHECK。这些宏可用于确保 optional 对象具有值。例如：

```c++
void f(std::optional<int> opt) {
  DCHECK(opt.has_value());
  use(*opt);
}
```

### Ensure that a value exists, then access the value in a correlated branch

### 确保值存在后，在相关分支中访问值

The check is aware of correlated branches in the code and can figure out when an optional object is ensured to have a value on all execution paths that lead to an access. For example:

该检查能够识别代码中的相关分支，并判断在所有通往访问点的执行路径中 optional 对象是否已被确保具有值。例如：

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

## 稳定函数结果

Function results are not assumed to be stable across calls, except for const accessor methods. For more complex accessors (non-const, or depend on multiple params) it is best to store the result of the function call in a local variable and use that variable to access the value. For example:

函数结果在多次调用中不被假定为稳定，除了 const 访问器方法。对于更复杂的访问器（非 const，或依赖多个参数），最好将函数调用结果存储在局部变量中，并使用该变量访问值。例如：

```c++
void f(Foo foo) {
  if (const auto& foo_opt = foo.take(); foo_opt.has_value()) {
    use(*foo_opt);
  }
}
```

## Do not rely on uncommon-API invariants

## 不要依赖不常见 API 的不变量

When uncommon APIs guarantee that an optional has contents, do not rely on it --instead, check explicitly that the optional object has a value. For example:

当不常见的 API 保证 optional 有内容时，不要依赖它——应显式检查 optional 对象是否有值。例如：

```c++
void f(Foo foo) {
  if (const auto& property = foo.GetProperty("bar")) {
    use(*property);
  }
}
```

instead of the HasProperty, GetProperty pairing we saw above.

而不是使用上面提到的 HasProperty 和 GetProperty 搭配方式。

## Do not rely on caller-performed checks

## 不要依赖调用者执行的检查

If you know that all of a function's callers have checked that an optional argument has a value, either change the function to take the value directly or check the optional again in the local scope of the callee. For example:

如果你知道所有调用该函数的地方都检查了 optional 参数是否有值，要么将函数改为直接接收值，要么在被调用函数的局部作用域中再次检查。例如：

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

以及

```c++
struct S {
  std::optional<int> opt;
  int x;
};

void g(const S &s) {
  if (s.opt.has_value() && s.x > 10) {
    use(*s.opt);
  }
}

void f(S s) {
  if (s.opt.has_value()) {
    g(s);
  }
}
```

## Additional notes

## 附加说明

### Aliases created via using declarations

### 通过 using 声明创建的别名

The check is aware of aliases of optional types that are created via using declarations. For example:

该检查能够识别通过 using 声明创建的 optional 类型别名。例如：

```c++
using OptionalInt = std::optional<int>;

void f(OptionalInt opt) {
  use(opt.value()); // unsafe: it is unclear whether `opt` has a value.
                    // 不安全：不清楚 `opt` 是否有值。
}
```

### Lambdas

### Lambda 表达式

The check does not currently report unsafe optional accesses in lambdas. A future version will expand the scope to lambdas, following the rules outlined above. It is best to follow the same principles when using optionals in lambdas.

当前该检查不会报告 lambda 表达式中的不安全 optional 访问。未来版本将扩展到 lambda，遵循上述规则。在 lambda 中使用 optional 时，最好遵循相同的原则。

### Access with operator\*() vs. value()

### 使用 operator\*() 与 value() 的区别

Given that value() has well-defined behavior (either throwing an exception or terminating the program), why treat it the same as operator*() which causes undefined behavior (UB)? That is, why is it considered unsafe to access an optional with value(), if it's not provably populated with a value? For that matter, why is CHECK() followed by operator*() any better than value(), given that they are semantically equivalent (on configurations that disable exceptions)?

既然 value() 有明确定义的行为（抛出异常或终止程序），为什么要将其与可能导致未定义行为（UB）的 operator*() 等同对待？换句话说，如果 optional 没有被明确证明具有值，为什么使用 value() 访问它也被认为是不安全的？再者，在禁用异常的配置中，CHECK() 后接 operator*() 与 value() 在语义上是等价的，为什么前者更好？

The answer is that we assume most users do not realize the difference between value() and operator*(). Shifting to operator*() and some form of explicit value-presence check or explicit program termination has two advantages:

答案是我们假设大多数用户并不了解 value() 与 operator*() 的区别。转而使用 operator*() 并配合显式的值存在性检查或显式的程序终止，有两个优势：

- Readability. The check, and any potential side effects like program shutdown, are very clear in the code. Separating access from checks can actually make the checks more obvious.

- 可读性。检查以及程序终止等潜在副作用在代码中非常清晰。将访问与检查分离实际上可以使检查更明显。

- Performance. A single check can cover many or even all accesses within scope. This gives the user the best of both worlds -- the safety of a dynamic check, but without incurring redundant costs.

- 性能。一次检查可以涵盖作用域内的多个甚至所有访问。这为用户提供了两全其美的方案——动态检查的安全性，同时避免了冗余开销。
