以下是翻译后的 markdown 文件内容，保留了中英文双语，中文在英文下方，并以一个空行分隔，代码块格式保持不变：

# misc-no-recursion

Finds strongly connected functions (by analyzing the call graph for SCC's (Strongly Connected Components) that are loops), diagnoses each function in the cycle, and displays one example of a possible call graph loop (recursion).

查找强连接函数（通过分析调用图中形成循环的强连接组件 SCC），诊断循环中的每个函数，并显示一个可能的调用图循环（递归）示例。

References:

参考资料：

- CERT C++ Coding Standard rule [DCL56-CPP. Avoid cycles during initialization of static objects](https://wiki.sei.cmu.edu/confluence/display/cplusplus/DCL56-CPP.+Avoid+cycles+during+initialization+of+static+objects).

  CERT C++ 编码标准规则 [DCL56-CPP. 避免在静态对象初始化期间出现循环](https://wiki.sei.cmu.edu/confluence/display/cplusplus/DCL56-CPP.+Avoid+cycles+during+initialization+of+static+objects)。

- JPL Institutional Coding Standard for the C Programming Language (JPL DOCID D-60411) rule [2.4 Do not use direct or indirect recursion]{.title-ref}.

  JPL C 语言机构编码标准（JPL DOCID D-60411）规则 [2.4 不要使用直接或间接递归]{.title-ref}。

- OpenCL Specification, Version 1.2 rule [6.9 Restrictions: i. Recursion is not supported.](https://www.khronos.org/registry/OpenCL/specs/opencl-1.2.pdf).

  OpenCL 规范，版本 1.2，规则 [6.9 限制：i. 不支持递归](https://www.khronos.org/registry/OpenCL/specs/opencl-1.2.pdf)。

Limitations:

限制：

- The check does not handle calls done through function pointers

  此检查无法处理通过函数指针进行的调用

- The check does not handle C++ destructors

  此检查无法处理 C++ 析构函数
