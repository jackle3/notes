
1. Interrupts and Caller-saved Registers:
	- Interrupts only save caller-saved registers because they are like an unexpected function call that could happen anywhere in the code
	- The code that was running didn't know it was about to be interrupted, so it couldn't save any registers itself
	- Since any caller is expected to assume caller-saved registers could be modified by any function call, it's safe for the interrupt to modify these registers as long as it restores them before returning
	- The interrupted code would have already saved any caller-saved registers it cared about before making any function calls

2. Cooperative Threads and Callee-saved Registers:
	- In cooperative threading, thread switches only happen at well-defined yield points
	- The thread knows it's about to yield, so it can save any caller-saved registers it cares about before yielding
	- The yield function, like any function, is responsible for preserving callee-saved registers
	- Since yields only happen in specific places where the code is prepared, there's no need to save registers that the code was already responsible for saving

3. Preemptive Threads and Both Types:
	- Preemptive thread switches can happen at any instruction, like interrupts
	- The code doesn't know when it might be switched out, so it can't save registers itself
	- Unlike interrupts, the thread switch might not return to this code for a long time
	- When the thread eventually resumes, both types of registers need to have their original values:
		- Caller-saved registers because the code might be in the middle of calculations
		- Callee-saved registers because any functions that were active on the stack expected these to be preserved
