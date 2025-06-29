# readability-avoid-unconditional-preprocessor-if

Finds code blocks that are constantly enabled or disabled in  
preprocessor directives by analyzing #if conditions, such as #if 0  
and #if 1, etc.

通过分析 #if 条件（例如 #if 0 和 #if 1 等），查找在预处理指令中始终启用或禁用的代码块。

```c++
#if 0
    // some disabled code
    // 一些被禁用的代码
#endif

#if 1
   // some enabled code that can be disabled manually
   // 一些可以手动禁用的启用代码
#endif
```

Unconditional preprocessor directives, such as #if 0 for disabled code  
and #if 1 for enabled code, can lead to dead code and always enabled  
code, respectively. Dead code can make understanding the codebase more  
difficult, hinder readability, and may be a sign of unfinished  
functionality or abandoned features. This can cause maintenance issues,  
confusion for future developers, and potential compilation problems.

无条件的预处理指令（如用于禁用代码的 #if 0 和用于启用代码的 #if 1）可能分别导致死代码和始终启用的代码。死代码会使理解代码库变得更加困难，降低可读性，并可能是未完成功能或被废弃特性的迹象。这可能会引发维护问题，使未来的开发人员感到困惑，并可能导致编译问题。

As a solution for both cases, consider using preprocessor macros or  
defines, like #ifdef DEBUGGING_ENABLED, to control code enabling or  
disabling. This approach provides better coordination and flexibility  
when working with different parts of the codebase. Alternatively, you  
can comment out the entire code using /\* \*/ block comments and add a  
hint, such as @todo, to indicate future actions.

针对这两种情况的解决方案，可以考虑使用预处理宏或定义（如 #ifdef DEBUGGING_ENABLED）来控制代码的启用或禁用。这种方法在处理代码库的不同部分时提供了更好的协调性和灵活性。或者，也可以使用 /\* \*/ 块注释将整段代码注释掉，并添加提示（例如 @todo）以指示未来的操作。
