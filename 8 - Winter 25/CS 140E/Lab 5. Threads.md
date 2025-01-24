# Threads
* We have two main types of threads:
	1. Non-preemptive threads (i.e. cooperative threads) run until they explicitly yield the processor.
	2. Pre-emptive threads can be interrupted at any time (e.g., if their time slice expires, or a higher-priority thread becomes runnable, etc).
* The trade-offs are:
	1. Cooperative threads:
		* Pros: Easy to preserve large invariants since all code is effectively a critical section until we yield the processor
		* Cons: Can introduce large, hard-to-debug latencies if threads don't yield frequently enough
	2. Pre-emptive threads:
		* Pros: Don't need to trust that code yields control appropriately, making them suitable for multi-user OSes
		* Cons: Much harder to write correct code since interruption can happen at any time
			* Example: Our GPIO and UART implementations would need significant changes to be thread-safe
* Generally, each thread needs:
	1. Its own stack that's large enough to prevent overflow
	2. A thread control block (TCB) to store thread state
	3. Assembly code to perform context switching:
		* Save all registers of current thread to its stack
		* Update TCB with new stack pointer location
		* Load saved registers of new thread from its stack
		* Update PC to resume the new thread

# Registers and Register Categories
* The ARM processor has 16 general-purpose registers (r0-r15) that can be saved and restored using standard load/store instructions.
	* `r4 --- r11`: callee-saved.
	* `r13`: stack pointer (sp). Today it doesn't matter, but in the general case where you save registers onto the sack, you will be using the stack pointer sp. Be careful when you save this register. (How to tell if the stack grows up or down?)
	* `r14`: link register (lr), holds the address of the instruction after the call instruction that called the current routine (in our case rpi_yield). Since any subsequent call will overwrite this register we need to save it during context switching.
	* `r15`: program counter (pc). Writing a value val to pc causes in control to immediately jump to val. (Irrespective of whether addr points to valid code or not.) From above: moving lr to pc will return from a call. Oddly, we do not have to save this value!

* **Callee-saved registers** (`r4-r11`, `sp`, `lr`)
	* Must be preserved by the callee (called function)
	* Callee must save these to stack before using them
	* Callee must restore them before returning
	* Caller can rely on values being preserved across function calls

* **Caller-saved registers** (`r0-r3`, `r12`)
	* Can be freely used by callee without saving/restoring
	* Used for:
		* `r0-r3`: Parameter passing and return values
		* `r12`: Intra-procedure scratch register
	* If caller needs these values preserved across a function call, caller must save/restore them

# Context Switching
* To context switch, we just need to save and restore the registers that are currently in use. For our non-preemptive threading system:
	* Context switches only occur during explicit yield calls (via `rpi_yield()`)
	* We only need to save callee-saved registers (`r4-r11`, `sp`, `lr`)
	* We don't need to save caller-saved registers (`r0-r3`, `r12`) because:
		* Any caller-saved registers containing live values will have already been preserved by the compiler-generated code if they're needed after the yield
		* The compiler handles this automatically when generating function calls
* So, to summarize, context-switching must save:
	* `r4 --- r11`, `r13`, `r14`. You can use `push` and `pop` to do most of this.
