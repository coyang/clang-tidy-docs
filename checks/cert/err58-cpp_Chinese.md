# cert-err58-cpp

This check flags all static or thread_local variable declarations where the initializer for the object may throw an exception.

该检查会标记所有 static 或 thread_local 变量声明，其中对象的初始化器可能会抛出异常。

This check corresponds to the CERT C++ Coding Standard rule ERR58-CPP. Handle all exceptions thrown before main() begins executing.

该检查对应于 CERT C++ 编码标准规则 ERR58-CPP：处理所有在 main() 开始执行之前抛出的异常。

https://www.securecoding.cert.org/confluence/display/cplusplus/ERR58-CPP.+Handle+all+exceptions+thrown+before+main%28%29+begins+executing

https://www.securecoding.cert.org/confluence/display/cplusplus/ERR58-CPP.+Handle+all+exceptions+thrown+before+main%28%29+begins+executing
