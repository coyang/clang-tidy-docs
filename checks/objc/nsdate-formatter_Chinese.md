# objc-nsdate-formatter

When NSDateFormatter is used to convert an NSDate type to a String type, the user can specify a custom format string. Certain format specifiers are undesirable despite being legal. See http://www.unicode.org/reports/tr35/tr35-dates.html#Date_Format_Patterns for all legal date patterns.

当使用 NSDateFormatter 将 NSDate 类型转换为 String 类型时，用户可以指定自定义的格式字符串。尽管某些格式说明符是合法的，但它们并不推荐使用。请参阅 http://www.unicode.org/reports/tr35/tr35-dates.html#Date_Format_Patterns 获取所有合法的日期格式模式。

This checker reports as warnings the following string patterns in a date format specifier:

该检查器会对以下日期格式说明符中的字符串模式发出警告：

1. yyyy + ww : Calendar year specified with week of a week year (unless YYYY is also specified).

   yyyy + ww：使用日历年与周年中的周组合（除非同时指定 YYYY）。
   - Example 1: Input Date: 29 December 2014 ; Format String: yyyy-ww;
     Output string: 2014-01 (Wrong because it's not the first week of 2014)

     示例 1：输入日期：2014 年 12 月 29 日；格式字符串：yyyy-ww；
     输出字符串：2014-01（错误，因为这不是 2014 年的第一周）

   - Example 2: Input Date: 29 December 2014 ; Format String: dd-MM-yyyy (ww-YYYY);
     Output string: 29-12-2014 (01-2015) (This is correct)

     示例 2：输入日期：2014 年 12 月 29 日；格式字符串：dd-MM-yyyy (ww-YYYY)；
     输出字符串：29-12-2014 (01-2015)（这是正确的）

2. F without ee/EE : Numeric day of week in a month without actual day.

   F 未与 ee/EE 一起使用：表示一个月中某个星期几的序数，但没有具体的星期几。
   - Example: Input Date: 29 December 2014 ; Format String: F-MM;
     Output string: 5-12 (Wrong because it reads as 5th \_\_\_ of Dec in English)

     示例：输入日期：2014 年 12 月 29 日；格式字符串：F-MM；
     输出字符串：5-12（错误，因为英文中理解为“12 月的第 5 个 \_\_\_”）

3. F without MM : Numeric day of week in a month without month.

   F 未与 MM 一起使用：表示一个月中某个星期几的序数，但没有指定月份。
   - Example: Input Date: 29 December 2014 ; Format String: F-EE
     Output string: 5-Mon (Wrong because it reads as 5th Mon of \_\_\_ in English)

     示例：输入日期：2014 年 12 月 29 日；格式字符串：F-EE；
     输出字符串：5-Mon（错误，因为英文中理解为“\_\_\_ 月的第 5 个星期一”）

4. WW without MM : Week of the month without the month.

   WW 未与 MM 一起使用：表示某个月的第几周，但没有指定月份。
   - Example: Input Date: 29 December 2014 ; Format String: WW-yyyy
     Output string: 05-2014 (Wrong because it reads as 5th Week of \_\_\_ in English)

     示例：输入日期：2014 年 12 月 29 日；格式字符串：WW-yyyy；
     输出字符串：05-2014（错误，因为英文中理解为“\_\_\_ 月的第 5 周”）

5. YYYY + QQ : Week year specified with quarter of normal year (unless yyyy is also specified).

   YYYY + QQ：使用周年与普通年份的季度组合（除非同时指定 yyyy）。
   - Example 1: Input Date: 29 December 2014 ; Format String: YYYY-QQ
     Output string: 2015-04 (Wrong because it's not the 4th quarter of 2015)

     示例 1：输入日期：2014 年 12 月 29 日；格式字符串：YYYY-QQ；
     输出字符串：2015-04（错误，因为这不是 2015 年的第 4 季度）

   - Example 2: Input Date: 29 December 2014 ; Format String: ww-YYYY (QQ-yyyy)
     Output string: 01-2015 (04-2014) (This is correct)

     示例 2：输入日期：2014 年 12 月 29 日；格式字符串：ww-YYYY (QQ-yyyy)；
     输出字符串：01-2015 (04-2014)（这是正确的）

6. YYYY + MM : Week year specified with Month of a calendar year (unless yyyy is also specified).

   YYYY + MM：使用周年与日历年的月份组合（除非同时指定 yyyy）。
   - Example 1: Input Date: 29 December 2014 ; Format String: YYYY-MM
     Output string: 2015-12 (Wrong because it's not the 12th month of 2015)

     示例 1：输入日期：2014 年 12 月 29 日；格式字符串：YYYY-MM；
     输出字符串：2015-12（错误，因为这不是 2015 年的第 12 个月）

   - Example 2: Input Date: 29 December 2014 ; Format String: ww-YYYY (MM-yyyy)
     Output string: 01-2015 (12-2014) (This is correct)

     示例 2：输入日期：2014 年 12 月 29 日；格式字符串：ww-YYYY (MM-yyyy)；
     输出字符串：01-2015 (12-2014)（这是正确的）

7. YYYY + DD : Week year with day of a calendar year (unless yyyy is also specified).

   YYYY + DD：使用周年与日历年的第几天组合（除非同时指定 yyyy）。
   - Example 1: Input Date: 29 December 2014 ; Format String: YYYY-DD
     Output string: 2015-363 (Wrong because it's not the 363rd day of 2015)

     示例 1：输入日期：2014 年 12 月 29 日；格式字符串：YYYY-DD；
     输出字符串：2015-363（错误，因为这不是 2015 年的第 363 天）

   - Example 2: Input Date: 29 December 2014 ; Format String: ww-YYYY (DD-yyyy)
     Output string: 01-2015 (363-2014) (This is correct)

     示例 2：输入日期：2014 年 12 月 29 日；格式字符串：ww-YYYY (DD-yyyy)；
     输出字符串：01-2015 (363-2014)（这是正确的）

8. YYYY + WW : Week year with week of a calendar year (unless yyyy is also specified).

   YYYY + WW：使用周年与日历年的第几周组合（除非同时指定 yyyy）。
   - Example 1: Input Date: 29 December 2014 ; Format String: YYYY-WW
     Output string: 2015-05 (Wrong because it's not the 5th week of 2015)

     示例 1：输入日期：2014 年 12 月 29 日；格式字符串：YYYY-WW；
     输出字符串：2015-05（错误，因为这不是 2015 年的第 5 周）

   - Example 2: Input Date: 29 December 2014 ; Format String: ww-YYYY (WW-MM-yyyy)
     Output string: 01-2015 (05-12-2014) (This is correct)

     示例 2：输入日期：2014 年 12 月 29 日；格式字符串：ww-YYYY (WW-MM-yyyy)；
     输出字符串：01-2015 (05-12-2014)（这是正确的）

9. YYYY + F : Week year with day of week in a calendar month (unless yyyy is also specified).

   YYYY + F：使用周年与日历月中某个星期几的序数组合（除非同时指定 yyyy）。
   - Example 1: Input Date: 29 December 2014 ; Format String: YYYY-ww-F-EE
     Output string: 2015-01-5-Mon (Wrong because it's not the 5th Monday of January in 2015)

     示例 1：输入日期：2014 年 12 月 29 日；格式字符串：YYYY-ww-F-EE；
     输出字符串：2015-01-5-Mon（错误，因为这不是 2015 年 1 月的第 5 个星期一）

   - Example 2: Input Date: 29 December 2014 ; Format String: ww-YYYY (F-EE-MM-yyyy)
     Output string: 01-2015 (5-Mon-12-2014) (This is correct)

     示例 2：输入日期：2014 年 12 月 29 日；格式字符串：ww-YYYY (F-EE-MM-yyyy)；
     输出字符串：01-2015 (5-Mon-12-2014)（这是正确的）
