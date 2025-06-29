以下是翻译后的 markdown 文件内容，保留了中英文双语，中文位于英文下方，并以一个空行分隔，同时保留了代码块格式：

# readability-convert-member-functions-to-static

Finds non-static member functions that can be made static because the functions don't use this.

查找可以转换为 static 的非静态成员函数，因为这些函数没有使用 this 指针。

After applying modifications as suggested by the check, running the check again might find more opportunities to mark member functions static.

按照检查建议进行修改后，再次运行该检查可能会发现更多可以标记为 static 的成员函数。

After making a member function static, you might want to run the check readability-static-accessed-through-instance <../readability/static-accessed-through-instance>{.interpreted-text role="doc"} to replace calls like Instance.method() by Class::method().

将成员函数设为 static 后，您可能希望运行检查 readability-static-accessed-through-instance <../readability/static-accessed-through-instance>{.interpreted-text role="doc"}，以将类似 Instance.method() 的调用替换为 Class::method()。
