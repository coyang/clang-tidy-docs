# bugprone-inaccurate-erase

Checks for inaccurate use of the erase() method.

检查对 erase() 方法的不准确使用。

Algorithms like remove() do not actually remove any element from the container but return an iterator to the first redundant element at the end of the container. These redundant elements must be removed using the erase() method. This check warns when not all of the elements will be removed due to using an inappropriate overload.

像 remove() 这样的算法实际上并不会从容器中移除任何元素，而是返回一个指向容器末尾第一个冗余元素的迭代器。这些冗余元素必须使用 erase() 方法来移除。此检查会在由于使用了不合适的重载版本而导致并未移除所有元素时发出警告。

For example, the following code erases only one element:

例如，以下代码只会移除一个元素：

```c++
std::vector<int> xs;
...
xs.erase(std::remove(xs.begin(), xs.end(), 10));
```

Call the two-argument overload of erase() to remove the subrange:

调用带有两个参数的 erase() 重载版本以移除子区间：

```c++
std::vector<int> xs;
...
xs.erase(std::remove(xs.begin(), xs.end(), 10), xs.end());
```
