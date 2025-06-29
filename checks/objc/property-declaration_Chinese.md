# objc-property-declaration

Finds property declarations in Objective-C files that do not follow the pattern of property names in Apple's programming guide. The property name should be in the format of Lower Camel Case.

查找 Objective-C 文件中不符合 Apple 编程指南中属性命名模式的属性声明。属性名称应采用小驼峰命名法（Lower Camel Case）的格式。

For code:

对于如下代码：

```objc
@property(nonatomic, assign) int LowerCamelCase;
```

The fix will be:

修正后的代码为：

```objc
@property(nonatomic, assign) int lowerCamelCase;
```

The check will only fix 'CamelCase' to 'camelCase'. In some other cases we will only provide warning messages since the property name could be complicated. Users will need to come up with a proper name by their own.

该检查仅会将 'CamelCase' 修正为 'camelCase'。在其他一些情况下，由于属性名称可能较为复杂，我们只会提供警告信息。用户需要自行想出一个合适的名称。

This check also accepts special acronyms as prefixes or suffixes. Such prefixes or suffixes will suppress the Lower Camel Case check according to the guide:  
<https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingBasics.html#//apple_ref/doc/uid/20001281-1002931-BBCFHEAB>

该检查还接受特殊缩略词作为前缀或后缀。根据指南中的说明，此类前缀或后缀将跳过小驼峰命名法的检查：  
<https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingBasics.html#//apple_ref/doc/uid/20001281-1002931-BBCFHEAB>

For a full list of well-known acronyms:  
<https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/APIAbbreviations.html#//apple_ref/doc/uid/20001285-BCIHCGAE>

完整的常用缩略词列表请参见：  
<https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/APIAbbreviations.html#//apple_ref/doc/uid/20001285-BCIHCGAE>

The corresponding style rule:  
<https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-1001757>

相关的命名风格规则：  
<https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-1001757>

The check will also accept property declared in category with a prefix of lowercase letters followed by a '\_' to avoid naming conflict. For example:

该检查也接受在分类（category）中声明的属性，其名称以小写字母加下划线（'\_'）作为前缀，以避免命名冲突。例如：

```objc
@property(nonatomic, assign) int abc_lowerCamelCase;
```

The corresponding style rule:  
<https://developer.apple.com/library/content/qa/qa1908/_index.html>

相关的命名风格规则：  
<https://developer.apple.com/library/content/qa/qa1908/_index.html>
