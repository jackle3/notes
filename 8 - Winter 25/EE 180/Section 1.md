# Lab 1
* We will be implementing radix-sort in MIPS assembly.

## Caller-saved registers
* `$t0 - $t9` (registers 8-15, 24, 25) are caller-saved registers
* The caller is responsible for saving and restoring these registers before calling a function
* The callee can use these registers without saving or restoring them

## Callee-saved registers
* `$s0 - $s7` (registers 16-23) are callee-saved registers
* The callee is responsible for saving and restoring these registers before returning from a function
* The caller can assume these registers will maintain their values across function calls
