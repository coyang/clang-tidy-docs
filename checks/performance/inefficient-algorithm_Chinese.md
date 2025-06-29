# performance-inefficient-algorithm

Warns on inefficient use of STL algorithms on associative containers.

警告在关联容器上低效地使用 STL 算法。

Associative containers implement some of the algorithms as methods which should be preferred to the algorithms in the algorithm header. The methods can take advantage of the order of the elements.

关联容器将某些算法实现为成员方法，这些方法应优先于 <algorithm> 头文件中的通用算法使用。这些成员方法能够利用元素的有序性提高效率。

```c++
std::set<int> s;
auto it = std::find(s.begin(), s.end(), 43);

// becomes
// 替换为

auto it = s.find(43);
```

```c++
std::set<int> s;
auto c = std::count(s.begin(), s.end(), 43);

// becomes
// 替换为

auto c = s.count(43);
```
