# google-build-explicit-make-pair

Check that make_pair's template arguments are deduced.

检查 make_pair 的模板参数是否由编译器自动推导。

G++ 4.6 in C++11 mode fails badly if make_pair's template arguments are specified explicitly, and such use isn't intended in any case.

在 C++11 模式下，如果显式指定 make_pair 的模板参数，G++ 4.6 会严重出错，而且这种用法本身也不是预期的。

Corresponding cpplint.py check name:  
[build/explicit_make_pair]{.title-ref}.

对应的 cpplint.py 检查名称：  
[build/explicit_make_pair]{.title-ref}。
