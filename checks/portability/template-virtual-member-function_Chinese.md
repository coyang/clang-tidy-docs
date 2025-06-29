# portability-template-virtual-member-function

Finds cases when an uninstantiated virtual member function in a template  
class causes cross-compiler incompatibility.

查找模板类中未实例化的虚函数导致跨编译器不兼容的情况。

Upon instantiating a template class, non-virtual member functions don't  
have to be instantiated unless they are used. Virtual member function  
instantiation on the other hand is unspecified and depends on the  
implementation of the compiler.

在实例化模板类时，非虚成员函数只有在被使用时才需要被实例化。而虚成员函数的实例化行为则未被明确规定，取决于编译器的具体实现。

In the following snippets the virtual member function is not  
instantiated by GCC and Clang, but it is instantiated by MSVC, so while  
the snippet is accepted by the former compilers, it is rejected by the  
latter.

在以下代码片段中，GCC 和 Clang 不会实例化虚成员函数，但 MSVC 会实例化。因此，该代码可以被前两者接受，但会被 MSVC 拒绝。

```c++
template<typename T>
struct CrossPlatformError {
    virtual ~CrossPlatformError() = default;

    static void used() {}

    virtual void unused() {
        T MSVCError = this;
    };
};

int main() {
    CrossPlatformError<int>::used();
    return 0;
}
```

Cross-platform projects that need to support MSVC on Windows might see  
compiler errors because certain virtual member functions are  
instantiated, which are not instantiated by other compilers on other  
platforms. This check highlights such virtual member functions.

需要支持 Windows 上 MSVC 的跨平台项目可能会遇到编译错误，因为某些虚成员函数会被实例化，而这些函数在其他平台的编译器中不会被实例化。此检查项会标出这些虚成员函数。
