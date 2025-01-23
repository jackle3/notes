# 0-timer-int
**The first part of our program is the `staff-start.S`.**
* It runs the `.globl _start` function, which is the entry point of our program.
* This script:
	* Forces the mode to be supervisor
	* Disables interrupts
	* Overrides `cpsr` to do these
	* Then calls `prefetch_flush`
* Focusing on the memmap (which tells the linker how to layout the executable in memory)
	* The executable is loaded to the address `0x8000`.
	* Then we have the read only data.
	* Then we have the data (which are initialized)
	* Then we have the `bss` (which are uninitialized, set to zero)
	* The linker then uses this to load the executable into memory.
* The stack is set to start at `0x8000000` because this is a pretty high address and the stack grows downwards.
* After setup, we then jump to the C starting script `_cstart`
	* In here, we zero out the `bss` section, surrounded by gcc memory barriers.
	* Then we call `notmain` to run our program.

**Once we call `notmain()`**
* We start by initializing interrupts with `interrupt_init()`
	1. Disables interrupts (which sets the 7th bit of `cpsr`)
	2. Then puts interrupt flags in known state by disabling all interrupt sources
		* Putting ones in `IRQ_Disable_1` and `IRQ_Disable_2` to disable all the sources that we don't care about.
		* We don't write to `IRQ_Disable_Basic` because the timer is a basic interrupt source.
	2. Then copies the exception vector table from the assembly file to the address starting at `0x00000000`, which is where the docs say the exception vector table should be.

* The interrupt vector table is defined as:
```asm
_interrupt_table:
  @ Q: why can we copy these ldr jumps and have
  @ them work the same?
  ldr pc, _reset_asm
  ldr pc, _undefined_instruction_asm
  ldr pc, _software_interrupt_asm
  ldr pc, _prefetch_abort_asm
  ldr pc, _data_abort_asm
  ldr pc, _reset_asm
  ldr pc, _interrupt_asm

_reset_asm:                   .word reset_asm
_undefined_instruction_asm:   .word undefined_instruction_asm
_software_interrupt_asm:      .word software_interrupt_asm
_prefetch_abort_asm:          .word prefetch_abort_asm
_data_abort_asm:              .word data_abort_asm
_interrupt_asm:               .word interrupt_asm
_interrupt_table_end:   @ end of the table.
```
* When an exception is hit and we jump to the exception vector table:
	1. We use PC-relative addressing to get to the address of the simple handler.
		* E.g. `ldr pc, _reset_asm`
	2. This simple handler simply stores the 32-bit address of the actual handler.
		* E.g. `_reset_asm:                   .word reset_asm`
	3. Therefore this allows us to load a 32-bit address into the PC register.

