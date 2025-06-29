# bugprone-assignment-in-if-condition

Finds assignments within conditions of [if]{.title-ref} statements. Such
assignments are bug-prone because they may have been intended as
equality tests.

This check finds all assignments within [if]{.title-ref} conditions,
including ones that are not flagged by [-Wparentheses]{.title-ref} due
to an extra set of parentheses, and including assignments that call an
overloaded [operator=()]{.title-ref}. The identified assignments violate
[BARR group \"Rule
8.2.c\"](https://barrgroup.com/embedded-systems/books/embedded-c-coding-standard/statement-rules/if-else-statements).

```c++
int f = 3;
if(f = 4) { // This is identified by both `Wparentheses` and this check - should it have been: `if (f == 4)` ?
  f = f + 1;
}

if((f == 5) || (f = 6)) { // the assignment here `(f = 6)` is identified by this check, but not by `-Wparentheses`. Should it have been `(f == 6)` ?
  f = f + 2;
}
```
