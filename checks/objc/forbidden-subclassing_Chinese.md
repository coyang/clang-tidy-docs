以下是翻译后的 markdown 文件内容，保留了中英文双语，中文在英文下方，并以一个空行分隔，同时保留了代码块格式：

# objc-forbidden-subclassing

Finds Objective-C classes which are subclasses of classes which are not
designed to be subclassed.

查找那些继承自不支持被继承的类的 Objective-C 子类。

By default, includes a list of Objective-C classes which are publicly
documented as not supporting subclassing.

默认情况下，包含一组被官方文档明确标注为不支持继承的 Objective-C 类。

::: note

Instead of using this check, for code under your control, you should add
**attribute**((objc_subclassing_restricted)) before your @interface
declarations to ensure the compiler prevents others from subclassing
your Objective-C classes. See
https://clang.llvm.org/docs/AttributeReference.html#objc-subclassing-restricted

对于您自己控制的代码，与其使用此检查，不如在 @interface 声明前添加 **attribute**((objc_subclassing_restricted))，以确保编译器阻止其他人继承您的 Objective-C 类。请参阅：https://clang.llvm.org/docs/AttributeReference.html#objc-subclassing-restricted

:::

## Options

## 选项

::: option
ForbiddenSuperClassNames

Semicolon-separated list of names of Objective-C classes which do not
support subclassing.

不支持继承的 Objective-C 类名列表，使用分号分隔。

Defaults to
[ABNewPersonViewController;ABPeoplePickerNavigationController;ABPersonViewController;ABUnknownPersonViewController;NSHashTable;NSMapTable;NSPointerArray;NSPointerFunctions;NSTimer;UIActionSheet;UIAlertView;UIImagePickerController;UITextInputMode;UIWebView]{.title-ref}.

默认值为：
[ABNewPersonViewController;ABPeoplePickerNavigationController;ABPersonViewController;ABUnknownPersonViewController;NSHashTable;NSMapTable;NSPointerArray;NSPointerFunctions;NSTimer;UIActionSheet;UIAlertView;UIImagePickerController;UITextInputMode;UIWebView]{.title-ref}。

:::
