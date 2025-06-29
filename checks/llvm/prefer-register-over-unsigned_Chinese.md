# llvm-prefer-register-over-unsigned

Finds historical use of unsigned to hold vregs and physregs and rewrites them to use Register.

查找历史上使用 unsigned 来保存虚拟寄存器（vregs）和物理寄存器（physregs）的情况，并将其重写为使用 Register。

Currently this works by finding all variables of unsigned integer type whose initializer begins with an implicit cast from Register to unsigned.

目前的做法是查找所有无符号整数类型的变量，其初始化器以从 Register 到 unsigned 的隐式转换开始。

```c++
void example(MachineOperand &MO) {
  unsigned Reg = MO.getReg();
  ...
}
```

```c++
void example(MachineOperand &MO) {
  Register Reg = MO.getReg();
  ...
}
```
