# cert-err60-cpp

This check flags all throw expressions where the exception object is not nothrow copy constructible.

此检查会标记所有异常对象不可无抛出复制构造的 throw 表达式。

This check corresponds to the CERT C++ Coding Standard rule [ERR60-CPP. Exception objects must be nothrow copy constructible](https://www.securecoding.cert.org/confluence/display/cplusplus/ERR60-CPP.+Exception+objects+must+be+nothrow+copy+constructible).

此检查对应于 CERT C++ 编码标准规则 [ERR60-CPP：异常对象必须是无抛出复制构造的](https://www.securecoding.cert.org/confluence/display/cplusplus/ERR60-CPP.+Exception+objects+must+be+nothrow+copy+constructible)。
