# bugprone-unused-return-value

Warns on unused function return values. The checked functions can be configured.

对未使用的函数返回值发出警告。可配置要检查的函数。

Operator overloading with assignment semantics are ignored.

具有赋值语义的运算符重载将被忽略。

## Options

## 选项

::: option
CheckedFunctions

Semicolon-separated list of functions to check. This parameter supports regexp. The function is checked if the name and scope matches, with any arguments. By default the following functions are checked:
^::std::async$, ^::std::launder$, ^::std::remove$, ^::std::remove_if$, ^::std::unique$, ^::std::unique_ptr::release$, ^::std::basic_string::empty$, ^::std::vector::empty$, ^::std::back_inserter$, ^::std::distance$, ^::std::find$, ^::std::find_if$, ^::std::inserter$, ^::std::lower_bound$, ^::std::make_pair$, ^::std::map::count$, ^::std::map::find$, ^::std::map::lower_bound$, ^::std::multimap::equal_range$, ^::std::multimap::upper_bound$, ^::std::set::count$, ^::std::set::find$, ^::std::setfill$, ^::std::setprecision$, ^::std::setw$, ^::std::upper_bound$, ^::std::vector::at$, ^::bsearch$, ^::ferror$, ^::feof$, ^::isalnum$, ^::isalpha$, ^::isblank$, ^::iscntrl$, ^::isdigit$, ^::isgraph$, ^::islower$, ^::isprint$, ^::ispunct$, ^::isspace$, ^::isupper$, ^::iswalnum$, ^::iswprint$, ^::iswspace$, ^::isxdigit$, ^::memchr$, ^::memcmp$, ^::strcmp$, ^::strcoll$, ^::strncmp$, ^::strpbrk$, ^::strrchr$, ^::strspn$, ^::strstr$, ^::wcscmp$, ^::access$, ^::bind$, ^::connect$, ^::difftime$, ^::dlsym$, ^::fnmatch$, ^::getaddrinfo$, ^::getopt$, ^::htonl$, ^::htons$, ^::iconv_open$, ^::inet_addr$, isascii$, isatty$, ^::mmap$, ^::newlocale$, ^::openat$, ^::pathconf$, ^::pthread_equal$, ^::pthread_getspecific$, ^::pthread_mutex_trylock$, ^::readdir$, ^::readlink$, ^::recvmsg$, ^::regexec$, ^::scandir$, ^::semget$, ^::setjmp$, ^::shm_open$, ^::shmget$, ^::sigismember$, ^::strcasecmp$, ^::strsignal$, ^::ttyname$

用分号分隔的要检查的函数列表。该参数支持正则表达式。若函数名称和作用域匹配（参数任意），则会被检查。默认检查以下函数：
^::std::async$, ^::std::launder$, ^::std::remove$, ^::std::remove_if$, ^::std::unique$, ^::std::unique_ptr::release$, ^::std::basic_string::empty$, ^::std::vector::empty$, ^::std::back_inserter$, ^::std::distance$, ^::std::find$, ^::std::find_if$, ^::std::inserter$, ^::std::lower_bound$, ^::std::make_pair$, ^::std::map::count$, ^::std::map::find$, ^::std::map::lower_bound$, ^::std::multimap::equal_range$, ^::std::multimap::upper_bound$, ^::std::set::count$, ^::std::set::find$, ^::std::setfill$, ^::std::setprecision$, ^::std::setw$, ^::std::upper_bound$, ^::std::vector::at$, ^::bsearch$, ^::ferror$, ^::feof$, ^::isalnum$, ^::isalpha$, ^::isblank$, ^::iscntrl$, ^::isdigit$, ^::isgraph$, ^::islower$, ^::isprint$, ^::ispunct$, ^::isspace$, ^::isupper$, ^::iswalnum$, ^::iswprint$, ^::iswspace$, ^::isxdigit$, ^::memchr$, ^::memcmp$, ^::strcmp$, ^::strcoll$, ^::strncmp$, ^::strpbrk$, ^::strrchr$, ^::strspn$, ^::strstr$, ^::wcscmp$, ^::access$, ^::bind$, ^::connect$, ^::difftime$, ^::dlsym$, ^::fnmatch$, ^::getaddrinfo$, ^::getopt$, ^::htonl$, ^::htons$, ^::iconv_open$, ^::inet_addr$, isascii$, isatty$, ^::mmap$, ^::newlocale$, ^::openat$, ^::pathconf$, ^::pthread_equal$, ^::pthread_getspecific$, ^::pthread_mutex_trylock$, ^::readdir$, ^::readlink$, ^::recvmsg$, ^::regexec$, ^::scandir$, ^::semget$, ^::setjmp$, ^::shm_open$, ^::shmget$, ^::sigismember$, ^::strcasecmp$, ^::strsignal$, ^::ttyname$

- std::async() — Not using the return value makes the call synchronous.
- std::async() — 未使用返回值会使调用变为同步执行。

- std::launder() — Not using the return value usually means that the function interface was misunderstood by the programmer. Only the returned pointer is "laundered", not the argument.
- std::launder() — 未使用返回值通常意味着程序员误解了函数接口。只有返回的指针被“清洗”，而不是传入的参数。

- std::remove(), std::remove_if(), and std::unique() — The returned iterator indicates the boundary between elements to keep and elements to be removed. Not using the return value means that the information about which elements to remove is lost.
- std::remove()、std::remove_if() 和 std::unique() — 返回的迭代器表示要保留和要移除元素之间的边界。未使用返回值意味着丢失了哪些元素应被移除的信息。

- std::unique_ptr::release() — Not using the return value can lead to resource leaks if the same pointer isn't stored anywhere else. Often, ignoring the release() return value indicates that the programmer confused the function with reset().
- std::unique_ptr::release() — 如果返回的指针未被存储在其他地方，未使用返回值可能导致资源泄漏。通常，忽略 release() 的返回值表明程序员将其与 reset() 混淆了。

- std::basic_string::empty() and std::vector::empty() — Not using the return value often indicates that the programmer confused the function with clear().
- std::basic_string::empty() 和 std::vector::empty() — 未使用返回值通常表明程序员将其与 clear() 混淆了。

:::

::: option
CheckedReturnTypes

Semicolon-separated list of function return types to check. By default the following function return types are checked:
^::std::error_code$, ^::std::error_condition$, ^::std::errc$, ^::std::expected$, ^::boost::system::error_code$

用分号分隔的要检查的函数返回类型列表。默认检查以下返回类型：
^::std::error_code$、^::std::error_condition$、^::std::errc$、^::std::expected$、^::boost::system::error_code$

:::

::: option
AllowCastToVoid

Controls whether casting return values to void is permitted. Default: false.

控制是否允许将返回值强制转换为 void。默认值：false。

:::

cert-err33-c <../cert/err33-c> is an alias of this check that checks a fixed and large set of standard library functions.

cert-err33-c <../cert/err33-c> 是此检查的别名，用于检查一组固定且数量众多的标准库函数。
