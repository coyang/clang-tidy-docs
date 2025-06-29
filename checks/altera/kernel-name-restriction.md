# altera-kernel-name-restriction

Finds kernel files and include directives whose filename is
[kernel.cl]{.title-ref}, [Verilog.cl]{.title-ref}, or
[VHDL.cl]{.title-ref}. The check is case insensitive.

Such kernel file names cause the offline compiler to generate
intermediate design files that have the same names as certain internal
files, which leads to a compilation error.

Based on the [Guidelines for Naming the Kernel]{.title-ref} section in
the [Intel FPGA SDK for OpenCL Pro Edition: Programming
Guide](https://www.intel.com/content/www/us/en/programmable/documentation/mwh1391807965224.html#ewa1412973930963).
