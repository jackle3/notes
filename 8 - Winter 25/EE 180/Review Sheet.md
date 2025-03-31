
## 1. ISA Fundamentals

### Key Concepts
- **ISA (Instruction Set Architecture)**: Contract between hardware and software
- **MIPS ISA Design Principles**:
	- Smaller is faster
	- Make the common case fast
	- Good design demands good compromises
	- Simplicity favors regularity

### MIPS System State
- **Registers**: 32 general-purpose 32-bit registers
- **Memory**: 32-bit address space (4GB), byte-addressable

### Instruction Formats
- **R-Format**: Register operations (e.g., `add $rd, $rs, $rt`)
	- Format: `[opcode(6)][rs(5)][rt(5)][rd(5)][shamt(5)][funct(6)]`
- **I-Format**: Immediate operations (e.g., `addi $rt, $rs, imm`)
	- Format: `[opcode(6)][rs(5)][rt(5)][immediate(16)]`
- **J-Format**: Jump operations (e.g., `j target`)
	- Format: `[opcode(6)][target address(26)]`

### MIPS Instruction Categories
- **Arithmetic**: `add`, `sub`, `addi`, `subi`
- **Logical**: `and`, `or`, `xor`, `nor`, `andi`, `ori`, bitshift operations
- **Data Transfer**: `lw`, `sw`, `lb`, `sb`, `lh`, `sh`
- **Control Flow**: `beq`, `bne`, `j`, `jal`, `jr`

### Alignment Rules
- MIPS requires data to be aligned:
	- Words (4 bytes) must be at addresses divisible by 4
	- Half-words (2 bytes) must be at addresses divisible by 2
	- Bytes can be at any address

### Endianness
- **Big-endian**: MSB stored at lowest memory address
- **Little-endian**: LSB stored at lowest memory address

### Key Example: Loading 32-bit Immediates
```assembly
# To load 0x12345678 into $t0:
lui $t0, 0x1234    # Load upper 16 bits
ori $t0, $t0, 0x5678  # OR with lower 16 bits
```

## 2. Control Flow & Branch Instructions

### Types of Control Flow
- **Conditional branches**: `beq`, `bne`, `blt`, `bgt`, etc.
- **Unconditional jumps**: `j`, `jal`, `jr`

### Branch Encoding
- **PC-relative addressing**: Branch target = PC + 4 + (imm × 4)
- **Pseudo-direct addressing** (J-format): Jump target = (PC & 0xF0000000) | (target << 2)
- Long jumps (>256MB) require `jr` with register holding full address

### Control Flow Patterns
- **If-then-else**:
```assembly
      beq $t0, $t1, else    # If t0 == t1, go to else
then: # then code
      j exit                # Skip else part
else: # else code
exit: # continuation
```

- **While loops**:
```assembly
loop: beq $t0, $zero, done  # If condition fails, exit loop
      # loop body
      j loop                # Jump back to start
done: # after loop
```

- **For loops**:
```assembly
      li $t0, 0             # Initialize counter
loop: bge $t0, $t1, done    # Exit if counter >= limit
      # loop body
      addi $t0, $t0, 1      # Increment counter
      j loop                # Jump back to start
done: # after loop
```

- **Switch statements** (via jump table):
```assembly
      sll $t0, $s0, 2       # Multiply case by 4 to get word offset
      add $t0, $t0, $s1     # Add base address of jump table
      lw $t0, 0($t0)        # Load address to jump to
      jr $t0                # Jump to case
```

## 3. Procedure Calls

### Procedure Call Steps
1. Place arguments in `$a0`-`$a3` registers
2. Transfer control to procedure using `jal target`
3. Allocate stack frame for local variables
4. Execute procedure body
5. Place return value in `$v0`-`$v1`
6. Free stack frame
7. Return control using `jr $ra`

### Register Convention
- **Caller-saved**: `$t0`-`$t9`, `$a0`-`$a3`, `$v0`-`$v1`
	- Caller must save these if needed after the call
- **Callee-saved**: `$s0`-`$s7`, `$sp`, `$fp`, `$ra`
	- Callee must preserve these for caller

### Stack Frame
- Grows downward in memory
- Typically contains:
	- Arguments passed on stack (beyond 4 registers)
	- Return address
	- Saved registers
	- Local variables

### Procedure Call Process
1. **Prologue** (start of function):
	- Allocate frame: `addi $sp, $sp, -framesize`
	- Save callee-saved registers
	- Setup frame pointer: `addi $fp, $sp, framesize-4`
2. **Epilogue** (end of function):
	- Place return value in `$v0`-`$v1`
	- Restore callee-saved registers
	- Deallocate frame: `addi $sp, $sp, framesize`
	- Return: `jr $ra`

### Tail Recursion Optimization
- When recursive call is the last operation, can convert to loop
- Eliminates stack frame overhead of recursion

## 4. Parallelism & Efficiency

### Types of Parallelism
1. **Data-level parallelism** (vectorization): Same operation on multiple data
2. **Thread-level parallelism**: Multiple threads of control

### Vector Processing
- **Vector instructions** operate on multiple data elements at once
- **Benefits**:
	- Higher performance by specifying parallel work
	- Amortized instruction fetch/decode cost
- **Example**: SAXPY operation (Y = aX + Y)
- **Strip mining**: Processing vectors larger than max vector length by breaking into chunks

### Thread-level Parallelism
- Multiple cores with shared memory
- **Shared memory synchronization** needed to prevent race conditions
- MIPS uses **load-linked** (`ll`) and **store-conditional** (`sc`) for atomic operations:
	- `ll $rt, offset($rs)`: Load value and track location
	- `sc $rt, offset($rs)`: Store value only if location unchanged since `ll`

### Performance Metrics
- **Cost**: Manufacturing costs, yield
- **Performance**: Latency (execution time), throughput
- **Power consumption**: C × Vdd² × F × (0→1) + Vdd × I_leakage
- **Energy**: Power × execution time

### Execution Time Components
- **CPU time** = Instructions × CPI × Clock cycle time
- **CPI** = Base CPI + Memory stall cycles
- **Amdahl's Law**: Speedup = 1/((1-f) + f/s)
	- f = fraction of code improved
	- s = speedup of improved section

## 5. Compilation & Logic Design

### Compilation Process
- **Front-end** (analysis):
	- Scanner → Parser → Semantic Analyzer → IR Generation
- **Back-end** (synthesis):
	- IR Optimization → Code Generation

### Intermediate Representations
- **Three-address code** (TAC): `x = y op z`
- **Single Static Assignment** (SSA): Each variable assigned exactly once

### Compiler Optimizations
- **Local optimizations**:
	- Constant folding
	- Common subexpression elimination
	- Dead code elimination
	- Register allocation
- **Global optimizations**:
	- Loop invariant code motion
	- Function inlining
	- Strength reduction
	- Loop unrolling/fusion

### GCC Optimization Levels
- **-O1**: Basic optimizations (local optimizations, register allocation)
- **-O2**: Moderate optimizations (global optimizations, no space-speed tradeoffs)
- **-O3**: Aggressive optimizations (function inlining, loop unrolling, vectorization)
- **-Os**: Optimize for size

### Digital Logic Components
- **State elements**: Store information (flip-flops, registers)
- **Combinational logic**: Computes functions of inputs
- **Clock**: Coordinates timing

### Boolean Algebra and Logic Gates
- Basic operations: AND, OR, NOT
- Logic gates: AND, OR, NOT, NAND, NOR, XOR, XNOR
- **Multiplexor (MUX)**: Selects one of multiple inputs based on select signal

### Sequential Logic
- **Flip-flops**: Basic memory unit that stores one bit
	- Updates on clock edge
	- Maintains value until next update
- **Clock period**: Must account for:
	- Propagation delay (t_pd)
	- Setup time (t_s)
	- Clock skew (t_skew)
	- Hold time (t_h)

### Memory Organization
- **SRAM**: Fast, expensive, used for caches
	- 6 transistors per bit
	- Retains value as long as power is on
- **DRAM**: Slower, denser, cheaper, used for main memory
	- 1 transistor + 1 capacitor per bit
	- Requires periodic refreshing

## 6. Processor Design

### Single-Cycle Processor
- One instruction completed per cycle
- Simple design but slow (cycle time = worst-case path)
- Underutilized hardware

### Control Signals
- **RegDst**: Selects destination register (0=rt, 1=rd)
- **ALUSrc**: Selects second ALU input (0=reg, 1=imm)
- **MemtoReg**: Selects data for register write (0=ALU, 1=memory)
- **RegWrite**: Controls register writing
- **MemRead/MemWrite**: Controls memory access
- **Branch/Jump**: Controls PC update
- **ALUOp**: Selects ALU operation

### Pipelined Processor
- Divides instruction execution into 5 stages:
	1. **IF**: Instruction Fetch
	2. **ID**: Instruction Decode & Register Read
	3. **EX**: Execute/Address Calculation
	4. **MEM**: Memory Access
	5. **WB**: Write Back
- Advantages:
	- Higher throughput (ideally CPI=1)
	- Higher clock frequency
- Disadvantages:
	- More complex design
	- Pipeline hazards

## 7. Pipeline Hazards

### Types of Hazards
1. **Structural hazards**: Two instructions need same resource simultaneously
	- Solution: Duplicate resources or stall
2. **Data hazards**: Instruction depends on result of previous instruction
	- Types: RAW (read after write), WAR (write after read), WAW (write after write)
	- Solutions: Forwarding, stalls, compiler scheduling
3. **Control hazards**: Next instruction depends on current instruction (branches)
	- Solutions: Stalling, prediction, delayed branches

### Data Hazard Solutions
#### Forwarding (Data Bypassing)
- Detect when an instruction needs a result from a previous instruction
- Forward the result directly from where it's available
- Key forwarding cases:
	- From EX/MEM to EX stage (1-cycle dependency)
	- From MEM/WB to EX stage (2-cycle dependency)
- Cannot forward from MEM stage for load instructions (need stall)

#### Load-Use Hazard
- When an instruction uses a value loaded by the immediately preceding instruction
- Requires one-cycle stall (even with forwarding)
- Compiler can reorder instructions to fill the delay slot

### Control Hazard Solutions
#### Branch Handling Strategies
1. **Stall on branches**: Wait until branch is resolved (penalty = 3 cycles)
2. **Predict not taken**: Continue with PC+4 (penalty if taken = 3 cycles)
3. **Predict taken**: Calculate branch target early (penalty if not taken = 3 cycles)
4. **Delayed branches**: Execute instruction after branch regardless (MIPS approach)

#### Branch Prediction
- **1-bit predictor**: Remember last outcome
- **2-bit predictor**: Use 4 states (strongly/weakly taken/not taken)
	- More resistant to thrashing between predictions

### Exception Handling
- Must maintain precise exceptions:
	- Instructions before excepting instruction complete
	- Excepting instruction and after do not change state
- Solution: When exception detected:
	- Complete preceding instructions
	- Flush following instructions
	- Save PC in EPC register
	- Jump to exception handler

## 8. Advanced Pipelining

### Deeper Pipelining
- More stages = higher clock frequency
- Challenges: branch delay, load delay, complexity

### Superscalar Processors
- Multiple instructions issued per cycle
- Multiple execution units
- Ideal CPI < 1

### Dynamic Scheduling
- Out-of-order execution
- Register renaming to eliminate false dependencies
- Reservation stations and reorder buffer

### Speculation
- Execute instructions before knowing if they're needed
- Roll back if prediction wrong
- Branch prediction becomes critical

## 9. Memory Hierarchy

### Memory Hierarchy Design
- Fast, small, expensive → Slow, large, cheap:
	- Registers → L1 Cache → L2 Cache → Main Memory → Disk
- Exploits **principle of locality**:
	- **Temporal locality**: Recently used items likely used again
	- **Spatial locality**: Items near recently used items likely used soon

### Cache Organization
- **Block/Line**: Unit of data transfer between cache and memory
- **Mapping policies**:
	- **Direct-mapped**: Each block maps to exactly one cache location
	- **Fully associative**: Block can go anywhere in cache
	- **N-way set associative**: Block can go in any of N locations in a specific set
- **Replacement policies**: LRU, Random, FIFO

### Cache Performance
- **Miss types**:
	- **Compulsory misses**: First reference to a block
	- **Capacity misses**: Cache too small to hold all needed blocks
	- **Conflict misses**: Multiple blocks map to same cache location
- **AMAT** (Average Memory Access Time) = Hit time + (Miss rate × Miss penalty)

### Write Policies
- **Write-through**: Write to both cache and memory
	- Simpler but more memory traffic
	- Often uses write buffer to hide latency
- **Write-back**: Write only to cache, mark dirty, write to memory when evicted
	- Less memory traffic but more complex
- **Write allocation**: Bring block into cache on write miss
- **No-write allocation**: Write directly to memory on miss

### Multilevel Caches
- **L1 cache**: Optimized for hit time, usually split I/D
- **L2/L3 cache**: Optimized for miss rate, usually unified
- **Inclusive**: Higher level contains all data in lower levels
- **Exclusive**: Each level contains unique data

## 10. Cache Coherence

### Cache Coherence Problem
- Multiple caches may have copies of same memory location
- Updates in one cache must be visible to other caches

### Coherence Protocols
- **MSI Protocol** (Modified, Shared, Invalid):
	- **Modified (M)**: Cache has exclusive, dirty copy
	- **Shared (S)**: Multiple caches have clean copies
	- **Invalid (I)**: Cache does not have valid copy
- Bus operations: BusRd, BusRdX, BusWB

### Key Invariants
- **Single-Writer, Multiple-Reader (SWMR)**: At any time, either one writer or multiple readers
- **Data-Value**: Memory value matches last writer's value

### Memory Consistency
- Defines when writes become visible to other processors
- Sequential consistency: All processors see same order of memory operations

## 11. Virtual Memory

### Virtual Memory Concepts
- Provides illusion of large, private address space to each process
- Maps virtual addresses to physical addresses
- Allows memory sharing and protection

### Address Translation
- **Page**: Block size for virtual memory (e.g., 4KB, 2MB)
- **Page table**: Maps virtual page numbers to physical page numbers
- **Translation process**:
	- VPN used to index page table
	- PTE provides physical page number
	- Offset added to form physical address

### Page Faults
- Occur when accessed page is not in physical memory
- OS handles by:
	- Loading page from disk
	- Updating page table
	- Restarting faulting instruction

### TLB (Translation Lookaside Buffer)
- Hardware cache for page table entries
- Caches virtual to physical mappings
- Reduces page table access overhead
- TLB miss can be handled by hardware or software (MIPS uses software)

### Memory Protection
- Each PTE contains protection bits (read, write, execute)
- Controls access rights to pages
- Enforced by hardware during translation

### Virtually Indexed Caches
- **Physically indexed, physically tagged (PIPT)**:
	- Translation before cache access
	- Simple but slower
- **Virtually indexed, physically tagged (VIPT)**:
	- Index cache while TLB lookup happens
	- Faster but has size constraints: Cache size ≤ Page size × Associativity

## 12. I/O And Storage

### I/O Device Classification
- **Behavior**: Input, output, or storage
- **Partner**: Human or machine
- **Data rate**: Speed of the device
- **Response time**: How quickly device responds

### Memory-Mapped I/O
- I/O devices mapped to memory address space
- CPU communicates with devices using regular load/store instructions
- I/O addresses protected via virtual memory mechanism

### I/O Notification Methods
- **Polling**: CPU periodically checks device status
	- Simple but wastes CPU cycles
	- Good for predictable, high-rate devices
- **Interrupts**: Device signals CPU when ready
	- Efficient for unpredictable, low-rate devices
	- Higher overhead per event

### Direct Memory Access (DMA)
- Allows devices to transfer data directly to/from memory without CPU involvement
- CPU sets up transfer then continues other work
- Issues:
	- **Memory pinning**: Pages must stay in physical memory during transfer
	- **Cache coherence**: DMA bypasses cache

### Storage Devices
- **Hard disk**: Mechanical storage
	- Access time = Seek time + Rotational latency + Transfer time
	- Performance improved via scheduling, larger transfers

### Bus Communication
- **Bus**: Shared communication path
	- Control lines, address lines, data lines
- **Types**:
	- Processor-Memory bus: High speed, short distance
	- I/O bus: Longer, more devices, wider range of speeds
- **Arbitration**: Controls which device gets bus access
	- Centralized parallel arbitration

## 13. Hardware Security

### Side-Channel Attacks
- Exploit unintended information leakage through shared hardware resources
- **Transient execution attacks**:
	- **Spectre**: Exploits branch misprediction
	- **Meltdown**: Exploits exception handling

### Security Defenses
- **Leakage contracts**: Specify allowed information flows
- **Hardware verification**: Ensure designs adhere to security contracts
- **Microarchitectural isolation**: Prevent sharing of critical resources

## 14. Custom Accelerators
### Efficiency Challenges
- Most energy in general-purpose processors wasted on instruction overhead
- Only small fraction goes to actual computation

### Solutions
- **Vectorization**: Amortize instruction overhead across multiple operations
- **Custom hardware**: Design specific datapaths for target algorithms
	- Example: Sobel filter accelerator
- **Data reuse**: Buffer data to maximize locality and minimize memory accesses

### Accelerator Design Considerations
- **Bandwidth matching**: Balance compute throughput with memory bandwidth
- **Dataflow optimization**: Choose best approach for data movement
	- Weight stationary, input stationary, output stationary, no local reuse
- **Energy efficiency**: Minimize data movement, which dominates energy use

### FPGAs (Field-Programmable Gate Arrays)
- Reconfigurable hardware
- Trade-offs versus ASICs and processors:
	- More flexible than ASICs
	- More efficient than processors
	- Less efficient than ASICs
	- Less programmable than processors

## Common Exam Problem Types and Solutions
### 1. Cache Analysis
- Calculate hit rate, miss rate, AMAT
- Analyze impact of changing cache parameters
- Example: If block size is doubled…
	- Compulsory misses decrease (spatial locality)
	- Conflict misses may increase (fewer blocks total)
	- Capacity misses unaffected (same total size)

### 2. Performance Calculation
```
CPU Time = IC × CPI × Clock Cycle Time
CPI = Base CPI + Memory stall cycles
Memory stall cycles = Memory accesses/Program × Miss rate × Miss penalty
```

**Example**: Calculate CPI for processor with:
- Base CPI = 1.5
- Instruction miss rate = 5%
- Data miss rate = 8%
- Data references per instruction = 0.4
- Miss penalty = 20 cycles

**Solution**:
```
Memory stall cycles = (1 × 0.05 + 0.4 × 0.08) × 20
                    = (0.05 + 0.032) × 20
                    = 0.082 × 20
                    = 1.64 cycles
CPI = 1.5 + 1.64 = 3.14
```

### 3. Pipeline Analysis
- Identify hazards and solutions
- Draw pipeline diagrams
- Calculate stall cycles

**Example**: Calculate cycles for sequence with load-use hazard:
```
lw  $t0, 0($s0)
add $t1, $t0, $s1
```

**Solution**:
- Without forwarding: 2-cycle stall (3 cycles total)
- With forwarding: 1-cycle stall (2 cycles total)

### 4. Disk Performance

**Example**: Calculate disk access time with:
- Seek time =.8ms
- Rotation speed = 7200 RPM
- Transfer rate = 100 MB/s
- Block size = 4KB

**Solution**:
```
Rotational latency = 0.5 × (60/7200) = 4.17ms
Transfer time = 4KB / 100MB/s = 0.04ms
Access time = 8ms + 4.17ms + 0.04ms = 12.21ms
```

### 5. Amdahl's Law Application

**Example**: If 40% of a program is parallelizable and you have 4 cores, what is the maximum speedup?

**Solution**:
```
Speedup = 1 / ((1-0.4) + 0.4/4)
        = 1 / (0.6 + 0.1)
        = 1 / 0.7
        = 1.43x
```

### 6. Virtual Memory Address Translation

**Example**: Given 32-bit virtual addresses, 4KB pages, translate 0x12345678:
- Virtual page number = 0x12345 (bits 31-12)
- Page offset = 0x678 (bits 11-0)

**Solution**:
- Look up VPN 0x12345 in page table
- If found, combine PPN with offset 0x678
- If not found, page fault
