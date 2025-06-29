# objc-missing-hash

Finds Objective-C implementations that implement -isEqual: without also appropriately implementing -hash.  
查找实现了 -isEqual: 但未适当实现 -hash 的 Objective-C 实现。

Apple documentation highlights that objects that are equal must have the same hash value:  
Apple 的文档强调，相等的对象必须具有相同的哈希值：  
<https://developer.apple.com/documentation/objectivec/1418956-nsobject/1418795-isequal?language=objc>

Note that the check only verifies the presence of -hash in scenarios where its omission could result in unexpected behavior. The verification of the implementation of -hash is the responsibility of the developer, e.g., through the addition of unit tests to verify the implementation.  
请注意，该检查仅在省略 -hash 可能导致意外行为的情况下验证其是否存在。-hash 的具体实现验证由开发者负责，例如通过添加单元测试来验证实现。
