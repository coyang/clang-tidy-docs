# objc-assert-equals

Finds improper usages of [XCTAssertEqual]{.title-ref} and  
[XCTAssertNotEqual]{.title-ref} and replaces them with  
[XCTAssertEqualObjects]{.title-ref} or  
[XCTAssertNotEqualObjects]{.title-ref}.

查找不正确使用 [XCTAssertEqual]{.title-ref} 和  
[XCTAssertNotEqual]{.title-ref} 的情况，并将其替换为  
[XCTAssertEqualObjects]{.title-ref} 或  
[XCTAssertNotEqualObjects]{.title-ref}。

This makes tests less fragile, as many improperly rely on pointer  
equality for strings that have equal values. This assumption is not  
guaranteed by the language.

这样可以使测试更健壮，因为许多测试错误地依赖于指针相等来判断值相等的字符串。  
这种假设在语言中并不被保证。
