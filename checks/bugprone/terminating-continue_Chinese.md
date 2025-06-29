# bugprone-terminating-continue

Detects do while loops with a condition always evaluating to false that have a continue statement, as this continue terminates the loop effectively.

检测条件始终为 false 的 do while 循环中包含 continue 语句的情况，因为这样的 continue 实际上会终止循环。

```c++
void f() {
do {
  // some code
  continue; // terminating continue
  // some other code
} while(false);
```

```c++
void f() {
do {
  // 一些代码
  continue; // 终止循环的 continue
  // 其他代码
} while(false);
```
