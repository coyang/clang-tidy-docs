# readability-container-contains

Finds usages of container.count() and container.find() == container.end() which should be replaced by a call to the container.contains() method.

查找使用 container.count() 和 container.find() == container.end() 的情况，这些用法应替换为调用 container.contains() 方法。

Whether an element is contained inside a container should be checked with contains instead of count/find because contains conveys the intent more clearly. Furthermore, for containers which permit multiple entries per key (multimap, multiset, ...), contains is more efficient than count because count has to do unnecessary additional work.

判断一个元素是否存在于容器中，应使用 contains 而不是 count/find，因为 contains 更清晰地表达了意图。此外，对于允许每个键有多个条目的容器（如 multimap、multiset 等），contains 比 count 更高效，因为 count 需要执行额外的不必要操作。

Examples:

示例：

Initial expression  
原始表达式

Result  
结果

---

myMap.find(x) == myMap.end()  
!myMap.contains(x)

myMap.find(x) != myMap.end()  
myMap.contains(x)

if (myMap.count(x))  
if (myMap.contains(x))

bool exists = myMap.count(x)  
bool exists = myMap.contains(x)

bool exists = myMap.count(x) > 0  
bool exists = myMap.contains(x)

bool exists = myMap.count(x) >= 1  
bool exists = myMap.contains(x)

bool missing = myMap.count(x) == 0  
bool missing = !myMap.contains(x)

This check will apply to any class that has a contains method, notably including std::set, std::unordered_set, std::map, and std::unordered_map as of C++20, and std::string and std::string_view as of C++23.

此检查适用于任何具有 contains 方法的类，特别包括 C++20 中的 std::set、std::unordered_set、std::map 和 std::unordered_map，以及 C++23 中的 std::string 和 std::string_view。
