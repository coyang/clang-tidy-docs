# objc-assert-equals

Finds improper usages of [XCTAssertEqual]{.title-ref} and
[XCTAssertNotEqual]{.title-ref} and replaces them with
[XCTAssertEqualObjects]{.title-ref} or
[XCTAssertNotEqualObjects]{.title-ref}.

This makes tests less fragile, as many improperly rely on pointer
equality for strings that have equal values. This assumption is not
guaranteed by the language.
