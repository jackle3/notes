# Lab 1
* We will be implementing radix-sort in MIPS assembly.

## Caller-saved Registers
* `$t0 - $t9` (registers 8-15, 24, 25) are caller-saved registers (page 1408)
* The caller is responsible for saving and restoring these registers before calling a function
* The callee can use these registers without saving or restoring them

## Callee-saved Registers
* `$s0 - $s7` (registers 16-23) and `$fp` and `$ra` are callee-saved registers (page 1408)
* The callee is responsible for saving and restoring these registers before returning from a function
* The caller can assume these registers will maintain their values across function calls


# 1. Before Caller Invokes Callee
![[Pasted image 20250113223947.png]]

# 2. Before Callee Starts Running
![[Pasted image 20250113223928.png]]
# 3. Before Callee Returns
![[Pasted image 20250113224023.png]]


# Main Function Example
* This is step 2: before callee starts running (in this case callee is `main`)
![[Pasted image 20250113224110.png]]

* This is step 1: before caller invokes callee (callee is `fact`)
![[Pasted image 20250113224207.png]]

* This is step 3: before callee returns (callee is `main`)
![[Pasted image 20250113224238.png]]

# Factorial Example
* This is step 2: before callee starts running (callee is `fact(n)`)
![[Pasted image 20250113224308.png]]

* This is all of computations, as well as step 1: before caller invokes callee (callee is `fact(n-1)`)
![[Pasted image 20250113224347.png]]

* This is step 3: before callee returns (callee is `fact(n)`)
![[Pasted image 20250113224409.png]]

* This is what the stack loops like
![[Pasted image 20250113224428.png|400]]
