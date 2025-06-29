# objc-nsdate-formatter

When `NSDateFormatter` is used to convert an `NSDate` type to a `String`
type, the user can specify a custom format string. Certain format
specifiers are undesirable despite being legal. See
<http://www.unicode.org/reports/tr35/tr35-dates.html#Date_Format_Patterns>
for all legal date patterns.

This checker reports as warnings the following string patterns in a date
format specifier:

1.  yyyy + ww : Calendar year specified with week of a week year (unless
    YYYY is also specified).
    - | **Example 1:** Input Date: [29 December 2014]{.title-ref} ;
      Format String: [yyyy-ww]{.title-ref};
      | Output string: [2014-01]{.title-ref} (Wrong because it's not
      the first week of 2014)

    - | **Example 2:** Input Date: [29 December 2014]{.title-ref} ;
      Format String: [dd-MM-yyyy (ww-YYYY)]{.title-ref};
      | Output string: [29-12-2014 (01-2015)]{.title-ref} (This is
      correct)

2.  F without ee/EE : Numeric day of week in a month without actual day.
    - | **Example:** Input Date: [29 December 2014]{.title-ref} ;
      Format String: [F-MM]{.title-ref};
      | Output string: [5-12]{.title-ref} (Wrong because it reads as
      _5th \_\_\_ of Dec_ in English)
3.  F without MM : Numeric day of week in a month without month.
    - | **Example:** Input Date: [29 December 2014]{.title-ref} ;
      Format String: [F-EE]{.title-ref}
      | Output string: [5-Mon]{.title-ref} (Wrong because it reads as
      \_5th Mon of \_\_\_\_ in English)
4.  WW without MM : Week of the month without the month.
    - | **Example:** Input Date: [29 December 2014]{.title-ref} ;
      Format String: [WW-yyyy]{.title-ref}
      | Output string: [05-2014]{.title-ref} (Wrong because it reads
      as \_5th Week of \_\_\_\_ in English)
5.  YYYY + QQ : Week year specified with quarter of normal year (unless
    yyyy is also specified).
    - | **Example 1:** Input Date: [29 December 2014]{.title-ref} ;
      Format String: [YYYY-QQ]{.title-ref}
      | Output string: [2015-04]{.title-ref} (Wrong because it's not
      the 4th quarter of 2015)

    - | **Example 2:** Input Date: [29 December 2014]{.title-ref} ;
      Format String: [ww-YYYY (QQ-yyyy)]{.title-ref}
      | Output string: [01-2015 (04-2014)]{.title-ref} (This is
      correct)

6.  YYYY + MM : Week year specified with Month of a calendar year
    (unless yyyy is also specified).
    - | **Example 1:** Input Date: [29 December 2014]{.title-ref} ;
      Format String: [YYYY-MM]{.title-ref}
      | Output string: [2015-12]{.title-ref} (Wrong because it's not
      the 12th month of 2015)

    - | **Example 2:** Input Date: [29 December 2014]{.title-ref} ;
      Format String: [ww-YYYY (MM-yyyy)]{.title-ref}
      | Output string: [01-2015 (12-2014)]{.title-ref} (This is
      correct)

7.  YYYY + DD : Week year with day of a calendar year (unless yyyy is
    also specified).
    - | **Example 1:** Input Date: [29 December 2014]{.title-ref} ;
      Format String: [YYYY-DD]{.title-ref}
      | Output string: [2015-363]{.title-ref} (Wrong because it's not
      the 363rd day of 2015)

    - | **Example 2:** Input Date: [29 December 2014]{.title-ref} ;
      Format String: [ww-YYYY (DD-yyyy)]{.title-ref}
      | Output string: [01-2015 (363-2014)]{.title-ref} (This is
      correct)

8.  YYYY + WW : Week year with week of a calendar year (unless yyyy is
    also specified).
    - | **Example 1:** Input Date: [29 December 2014]{.title-ref} ;
      Format String: [YYYY-WW]{.title-ref}
      | Output string: [2015-05]{.title-ref} (Wrong because it's not
      the 5th week of 2015)

    - | **Example 2:** Input Date: [29 December 2014]{.title-ref} ;
      Format String: [ww-YYYY (WW-MM-yyyy)]{.title-ref}
      | Output string: [01-2015 (05-12-2014)]{.title-ref} (This is
      correct)

9.  YYYY + F : Week year with day of week in a calendar month (unless
    yyyy is also specified).
    - | **Example 1:** Input Date: [29 December 2014]{.title-ref} ;
      Format String: [YYYY-ww-F-EE]{.title-ref}
      | Output string: [2015-01-5-Mon]{.title-ref} (Wrong because it's
      not the 5th Monday of January in 2015)

    - | **Example 2:** Input Date: [29 December 2014]{.title-ref} ;
      Format String: [ww-YYYY (F-EE-MM-yyyy)]{.title-ref}
      | Output string: [01-2015 (5-Mon-12-2014)]{.title-ref} (This is
      correct)
