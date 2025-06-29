# bugprone-incorrect-roundings

Checks the usage of patterns known to produce incorrect rounding.  
检查已知会导致错误舍入的代码模式的使用。

Programmers often use:  
程序员通常会使用：

    (int)(double_expression + 0.5)

to round the double expression to an integer. The problem with this:  
将 double 表达式四舍五入为整数。这样做的问题在于：

1.  It is unnecessarily slow.  
    它的执行速度不必要地慢。

2.  It is incorrect. The number 0.499999975 (smallest representable  
    float number below 0.5) rounds to 1.0. Even worse behavior for  
    negative numbers where both -0.5f and -1.4f both round to 0.0.  
    它是错误的。数值 0.499999975（小于 0.5 的最小可表示 float 数）会被舍入为 1.0。对于负数的行为更糟，例如 -0.5f 和 -1.4f 都会被舍入为 0.0。
