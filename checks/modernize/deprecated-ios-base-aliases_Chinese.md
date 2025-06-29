# modernize-deprecated-ios-base-aliases

Detects usage of the deprecated member types of std::ios_base and  
replaces those that have a non-deprecated equivalent.

检测对 std::ios_base 中已弃用成员类型的使用，  
并将其替换为未弃用的等效类型。

Deprecated member type Replacement  
已弃用的成员类型 替代类型

---

std::ios_base::io_state → std::ios_base::iostate  
std::ios_base::io_state → std::ios_base::iostate

std::ios_base::open_mode → std::ios_base::openmode  
std::ios_base::open_mode → std::ios_base::openmode

std::ios_base::seek_dir → std::ios_base::seekdir  
std::ios_base::seek_dir → std::ios_base::seekdir

std::ios_base::streamoff 和 std::ios_base::streampos 没有直接的已弃用别名，  
因此未在此列出。
