# altera-kernel-name-restriction

Finds kernel files and include directives whose filename is  
[kernel.cl]{.title-ref}, [Verilog.cl]{.title-ref}, or  
[VHDL.cl]{.title-ref}. The check is case insensitive.

查找文件名为 [kernel.cl]{.title-ref}、[Verilog.cl]{.title-ref} 或  
[VHDL.cl]{.title-ref} 的内核文件和 include 指令。该检查不区分大小写。

Such kernel file names cause the offline compiler to generate  
intermediate design files that have the same names as certain internal  
files, which leads to a compilation error.

这些内核文件名会导致离线编译器生成与某些内部文件同名的中间设计文件，  
从而引发编译错误。

Based on the [Guidelines for Naming the Kernel]{.title-ref} section in  
the [Intel FPGA SDK for OpenCL Pro Edition: Programming  
Guide](https://www.intel.com/content/www/us/en/programmable/documentation/mwh1391807965224.html#ewa1412973930963).

基于 [Intel FPGA SDK for OpenCL Pro Edition: 编程指南] 中  
[内核命名指南]{.title-ref} 一节的内容。  
(链接：https://www.intel.com/content/www/us/en/programmable/documentation/mwh1391807965224.html#ewa1412973930963)
