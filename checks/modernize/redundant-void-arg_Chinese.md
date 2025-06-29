# modernize-redundant-void-arg

Find and remove redundant void argument lists.

查找并移除冗余的 void 参数列表。

Examples:

示例：

:  
 Initial code Code with applied fixes  
 初始代码 应用修复后的代码

---

int f(void);  
int f();

int f(void);  
int f();

int (*f(void))(void);  
int (*f())();

int (*f(void))(void);  
int (*f())();

typedef int (*f_t(void))(void);  
typedef int (*f_t())();

typedef int (*f_t(void))(void);  
typedef int (*f_t())();

void (C::*p)(void);  
void (C::*p)();

void (C::*p)(void);  
void (C::*p)();

C::C(void) {}  
C::C() {}

C::C(void) {}  
C::C() {}

C::~C(void) {}  
C::~C() {}

C::~C(void) {}  
C::~C() {}
