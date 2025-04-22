# 1 CS149 Parallel Computing Cheatsheet

## 1.1 Fundamentals of Parallelism

### 1.1.1 Why Parallelism?
- Parallel computer: Collection of processing elements cooperating to solve problems quickly
- Speedup = Time(1 processor) / Time(P processors)
- Limitations:
	- Communication between processors
	- Imbalance in work assignments
	- Overhead costs

### 1.1.2 Amdahl's Law
- If S = fraction of sequential execution
- Maximum speedup with infinite processors ≤ 1/S
- Dependencies limit maximum possible speedup
- Maximum speedup given P processors ≤ 1/(S + (1-S)/P)

### 1.1.3 Key Themes
1. **Scale**: Design parallel programs that scale well
	- Decompose work safely
	- Assign work efficiently
	- Manage communication/synchronization
2. **Hardware Understanding**: Know how parallel computers work
3. **Efficiency**: Fast ≠ Efficient (2x speedup on 10 processors is poor)

## 1.2 Modern Processor Architecture

### 1.2.1 Basic Components
1. **Control** (fetch/decode): Determines next instruction
2. **Execution unit** (ALU): Performs operations
3. **State** (execution context): Maintains program state

### 1.2.2 Memory Hierarchy
1. **Registers**: Fastest, smallest
2. **Cache Levels** (L1, L2, L3):
	- Each larger but slower than previous
	- Pulled in contiguous chunks (cache lines)
3. **Main Memory**: Largest, slowest

### 1.2.3 Cache Concepts
- **Cache Line**: Contiguous chunk of memory loaded together
- **Cache Policies**: Usually LRU (Least Recently Used)
- **Locality Types**:
	- Spatial: Benefits from cache line loading
	- Temporal: Benefits from repeated access
- **Miss Types**:
	- Cold miss: First access
	- Capacity miss: Cache full
	- Conflict miss: Address mapping conflicts

### 1.2.4 Modern Processor Features
- **Superscalar Execution**:
	- CPU can execute multiple instructions per cycle
	- Instructions must be independent
- Example: 4-way superscalar can start 4 instructions/cycle
	- Benefits:
		- Instruction-level parallelism (ILP)
		- Better utilization of execution units
		- Hides latency of long operations
	- Limited by:
		- Data dependencies
		- Control dependencies
		- Available execution units

## 1.3 Parallel Programming Models

### 1.3.1 Thread-based Parallelism
- Each thread has private memory
- Threads communicate via shared memory
- Synchronization needed for coordination

### 1.3.2 Data Parallel Programming
- Operations on sequences of data
- Common operations:
	- map: Apply function to each element
	- reduce: Combine elements using binary operation
	- scan: Running totals like prefix sum
	- filter: Select elements meeting criteria

### 1.3.3 CUDA Programming
- Hierarchy:
	1. **Thread**: Finest level of execution, runs a single instance of the kernel function. Has its own registers and local memory.
	2. **Warp**: Group of 32 threads that execute in SIMD fashion (Single Instruction Multiple Data). All threads in a warp execute the same instruction simultaneously on different data. When threads in a warp diverge (e.g., due to if/else), the warp executes each branch path sequentially with some threads inactive, reducing performance.
	3. **Block**: Collection of threads (organized into warps) that can cooperate via shared memory and synchronization primitives. Limited by hardware resources.
	4. **Grid**: Array of thread blocks that can be distributed across multiple streaming multiprocessors (SMs). Enables program to scale across different GPU capabilities.
- Memory Types:
	- Global: Accessible by all threads
	- Shared: Per-block memory
	- Local: Per-thread memory

## 1.4 Optimization Techniques

### 1.4.1 Work Distribution
1. **Static Assignment**
	- Fixed work distribution
	- Good when work is predictable
	- Low runtime overhead

2. **Dynamic Assignment**
	- Work stolen/redistributed as needed
	- Better for unpredictable workloads
	- Higher overhead

### 1.4.2 Communication Optimization
1. Minimize data movement
2. Use local memory when possible
3. Batch communications
4. Overlap computation with communication

### 1.4.3 Cache Coherence
- Ensures consistent view of memory across processors
- Protocols:
	- MSI (Modified, Shared, Invalid)
	- MESI (Modified, Exclusive, Shared, Invalid)
- Solutions:
	1. Snooping-based
	2. Directory-based

## 1.5 Performance Considerations

### 1.5.1 Memory Bandwidth
- Often the limiting factor
- Calculate arithmetic intensity:
	- Operations per byte of memory accessed
- Use roofline model to identify bottlenecks

### 1.5.2 Load Balancing
- Ensure even work distribution
- Methods:
	1. Block distribution
	2. Cyclic distribution
	3. Dynamic scheduling

### 1.5.3 Synchronization
- Minimize use of locks
- Use barriers only when necessary
- Consider lock-free alternatives
- Avoid false sharing

### 1.5.4 Utilization Analysis
- **Core Utilization** = (actual throughput) / (peak throughput)
	- Example: If peak is 12 GFLOPS and achieving 9 GFLOPS → 75% utilization
	- Detailed Example:
		- Single thread with 50 cycle memory latency + 10 cycle arithmetic
		- Utilization = 10/(50+10) = 16.7%
		- With 6 threads: Utilization = min(1, 6 * 16.7%) = 100%
		- Need ceil(60/10) = 6 threads for full utilization
		- Additional threads won't improve throughput

- **SIMD Utilization** = (active SIMD lanes) / (total SIMD lanes)
	- Example: If 3 of 4 lanes active → 75% SIMD utilization
	- Common causes of low SIMD utilization:
		- Control flow divergence (if/else branches)
		- Memory access patterns (unaligned/scattered)
		- Data dependencies between lanes
		- Partial vector operations
	- Example:
		- 8-wide SIMD, processing array of size 30
		- Last iteration only uses 6 lanes
		- Average utilization = (3*8 + 1*6)/(4*8) = 93.75%

- **Steady State Behavior**:
	- **Compute Bound**: Utilization limited by compute capabilities
		- Utilization = min(1, compute_time/total_time)
		- Common solution: Add more compute units/cores
		- Example:
			- 100 cycle compute, 50 cycle memory
			- Single thread: U = 100/150 = 66.7%
			- Need 2 threads for 100% utilization
			- Adding more threads beyond 2 won't help
			- GPU solution: Use massive parallelism with many compute units
	- **Memory Bandwidth Bound**: Utilization limited by memory bandwidth
		- Utilization = min(1, bandwidth_achieved/bandwidth_peak)
		- Common solution: Caching, changing access patterns, better memory tech
		- Example:
			- Peak bandwidth: 100 GB/s
			- Application achieved is: 80 GB/s
			- Utilization = 80% regardless of threads
			- HBM memory could provide 2-3x more bandwidth
	- **Memory Latency Bound**:
		- Memory latency can be hidden with enough parallel operations
		- Required threads = (memory_latency + compute_time) / compute_time
		- Common solution: Thread-level parallelism to hide latency
		- Example:
			- 50 cycle memory latency, 10 cycle compute
			- Single thread: Stalls 50 cycles waiting for each load
			- Need 6 threads to hide latency completely
			- Thread 1 computes while threads 2-6 wait for memory
			- Perfect pipelining achieves 100% utilization
			- Hardware prefetching can reduce required threads
			- Software prefetching can help if pattern is predictable

- **Key Points**:
	- Memory latency affects single-thread performance but can be hidden
	- Number of threads needed = total_cycle_time / compute_time
	- What matters most:
		1. Peak compute capability (for compute bound)
		2. Peak memory bandwidth (for bandwidth bound)
		3. Arithmetic intensity determines which bound applies
		4. Available parallelism to hide latency

### 1.5.5 Throughput and Bandwidth Calculations

#### 1.5.5.1 Peak Throughput
- **Basic Formula**:
	Peak FLOPS = Cores × SIMD Width × Clock Speed × Operations per cycle
	- Hardware threads don't increase peak FLOPS, they only help hide latency

- **Example**:
	- Hardware: 8 cores running at 3.2 GHz
	- Each core has 16-wide SIMD
	- Can do 2 Fused Multiply-Add (FMA) operations per cycle
	- Total Peak = 8 cores × 16 floats × 3.2 billion cycles/sec × 2 ops = 819.2 GFLOPS

#### 1.5.5.2 Memory Bandwidth Requirements
- **Basic Formula**:
	Required Memory Bandwidth = Data accessed per unit time
	= (Bytes needed × Operations per second) / Operations per data access
	= (# of cores × clock) × (SIMD width × bytes per ops)
- Note that the units are:
	- (cores × cycles per second per core) × (ops per cycle × bytes per op) = bytes per second
$$
\text{cores} \times \frac{\text{cycle}}{\text{second} ×\text{core}}× \frac{\text{ops}}{\text{cycle}}× \frac{\text{bytes}}{\text{op}} = \frac{\text{bytes}}{\text{second}}
$$

- **Real Example**: Vector Addition Program
	- System: 8 cores, 32-wide SIMD, 1 GHz clock
	- Maximum compute: 8 × 32 × 1G = 256 billion ops/sec
	- Memory pattern: Read 12 bytes every 30 compute cycles
	- Required bandwidth = 256G × (12/30) = 102.4 GB/sec

#### 1.5.5.3 Arithmetic Intensity (Computing Vs Memory Ratio)
- **Formula**:
	Arithmetic Intensity = Compute Operations / Memory Bytes Accessed
	- Higher is better - means more computation per memory access

- **Real Example**: Simple Matrix Multiplication
	- Each FMA operation: 2 floating point operations
	- Needs to access: 12 bytes of data (3 4-byte floats)
	- Arithmetic Intensity = 2/12 = 0.167 operations/byte
	- This low ratio indicates memory-bound behavior

- To find the maximum arithmetic intensity, divide the peak throughput by the memory bandwidth.
$$
\text{Arithmetic Intensity (ops per byte)} = \frac{\text{Peak Throughput (ops per second)}}{\text{Memory Bandwidth (bytes per second)}}
$$
- This corresponds to the "knee" part of the **roofline plot**
- **Example:**
	- Suppose we have a 1GHz dual-core processor with 4-wide SIMD. The peak throughput is 8 GFLOPS/sec.
	- Suppose the memory system provides 4 GB/sec of bandwidth.
	- The knee is at 8/4 = 2 ops per byte
![Pasted image 20241114152534](../../attachments/Pasted%20image%2020241114152534.png)

#### 1.5.5.4 Memory-Limited Performance
- When memory bandwidth is the bottleneck:
	Actual Performance = Available Memory Bandwidth × Arithmetic Intensity
	- Example: With 50 GB/s bandwidth and AI of 0.167:
		Maximum achievable = 50 GB/s × 0.167 = 8.35 GFLOPS

## 1.6 Common Patterns

### 1.6.1 Map-Reduce
- Map: Transform each element independently
- Reduce: Combine results using associative operation
- Good for data-parallel operations

### 1.6.2 Fork-Join
- Fork: Spawn parallel tasks
- Join: Wait for completion
- Good for recursive parallelism

### 1.6.3 Pipeline Parallelism
- Break computation into stages
- Each stage processes different data
- Good for streaming computations

## 1.7 Distributed Systems

### 1.7.1 HDFS Architecture
- **NameNode (Master)**:
	- Stores metadata
	- Tracks file locations
	- Manages load balancing
- **DataNodes (Chunks)**:
	- Store actual data chunks
	- Usually 64-256MB chunks
	- 3x replication typical
- **Client Operations**:
	- Read: Contact NameNode → DataNode
	- Write: Pipeline through DataNodes

### 1.7.2 Specialized Hardware
- **Double Buffering**:
	- Producer writes to one buffer
	- Consumer reads from other buffer
	- Enables parallel operation
- **Metapipelining**:
	- Multiple pipeline stages run in parallel
	- Each stage can process different data

### 1.7.3 Synchronization Mechanisms

#### 1.7.3.1 Locks and Mutexes
- **Basic Operations**:
	- lock(): Acquire exclusive access
	- unlock(): Release exclusive access
- **Properties**:
	- Mutual exclusion
	- Progress guarantee
	- Bounded waiting
- **Implementation Considerations**:
	- Test-and-Set (TAS)
	- Compare-and-Swap (CAS)
	- Load-Linked/Store-Conditional (LL/SC)

#### 1.7.3.2 Fine-Grained Synchronization
- **Hand-over-hand locking**:
	- Lock one node at a time
	- Release previous node's lock
	- Good for linked data structures
- **Reader-Writer Locks**:
	- Multiple readers OK
	- Single writer needs exclusive access
	- Can lead to writer starvation

#### 1.7.3.3 Lock-Free Programming
- **Properties**:
	- At least one thread makes progress
	- No mutual exclusion
	- Uses atomic operations
- **Common Patterns**:
	- Double-Compare-and-Swap (DCAS)
	- ABA problem and solutions
	- Version counting

#### 1.7.3.4 Transactional Memory
- **Basic Concept**:
	- Atomic { … } blocks
	- System tracks reads/writes
	- Automatic conflict detection
- **Types**:
	- Software (STM)
	- Hardware (HTM)
	- Hybrid approaches
- **Conflict Resolution**:
	- Optimistic: Detect at commit
	- Pessimistic: Detect at access
	- Contention management policies

## 1.8 Advanced Parallel Patterns

### 1.8.1 Work Stealing
- **Basic Algorithm**:
	- Each thread has work queue
	- Idle threads steal from others
	- Random victim selection
- **Properties**:
	- Good load balancing
	- Locality-aware
	- Low synchronization overhead

### 1.8.2 Event-Based Programming
- **Event Loop**:
	- Single thread processes events
	- Non-blocking operations
	- Callbacks for completion
- **Async/Await Pattern**:
	- Syntactic sugar for callbacks
	- Maintains sequential logic
	- Efficient resource usage

### 1.8.3 Task Graphs
- **Components**:
	- Nodes represent tasks
	- Edges represent dependencies
	- Weights indicate costs
- **Scheduling**:
	- Critical path analysis
	- List scheduling
	- Work stealing

## 1.9 Performance Analysis

### 1.9.1 Critical Path Analysis
- **Definitions**:
	- Work (T1): Total operations
	- Span (T∞): Longest path
	- Parallelism: T1/T∞
- **Bounds**:
	- Tp ≥ max(T1/p, T∞)
	- Good parallelization: Tp ≈ T1/p

### 1.9.2 Scheduling Theory
- **Metrics**:
	- Makespan
	- Response time
	- Resource utilization
- **Algorithms**:
	- Work stealing
	- List scheduling
	- Gang scheduling

### 1.9.3 Memory Models
- **Sequential Consistency**:
	- All operations appear in program order
	- Global ordering of operations
- **Relaxed Models**:
	- Total Store Ordering (TSO)
	- Partial Store Ordering (PSO)
	- Release Consistency

## 1.10 Advanced Hardware Concepts

### 1.10.1 NUMA Architecture
- **Properties**:
	- Non-uniform memory access times
	- Local vs remote memory
	- Memory affinity important
- **Optimization**:
	- Thread placement
	- Memory allocation
	- Data migration

### 1.10.2 Cache Coherence Protocols
- **MESI Protocol Details**:
	- Modified: Dirty exclusive
	- Exclusive: Clean exclusive
	- Shared: Clean shared
	- Invalid: Not in cache
- **Directory-Based**:
	- Centralized directory
	- Tracks cache line state
	- Scales better than snooping

### 1.10.3 Performance Monitoring
- **Hardware Counters**:
	- Cache misses
	- Branch mispredictions
	- Memory bandwidth
	- FLOPS
- **Analysis Tools**:
	- Profilers
	- Trace collectors
	- Visualization tools

## 1.11 Specialized Hardware

### 1.11.1 TPU (Tensor Processing Unit)
- **Architecture**:
	- Systolic array design
	- 8-bit integer operations
	- Matrix multiply focused
	- Weight stationary algorithm
- **Benefits**:
	- High throughput for DNNs
	- Energy efficient
	- Dense matrix operations

### 1.11.2 Modern GPUs (H100)
- **Key Features**:
	- Tensor cores
	- Transformer engines
	- High bandwidth memory (HBM)
	- Async execution/copy
- **Memory Hierarchy**:
	- Thread block clusters
	- SM-to-SM network
	- Tensor Memory Accelerator (TMA)
- **Optimization**:
	- 16x16 tile operations
	- Asynchronous execution
	- Pipeline overlapping

### 1.11.3 Domain Specific Languages
- **ThunderKittens**:
	- Tile-based processing
	- Async compute/memory
	- Pipeline coordination
	- Templated data types
- **Performance Considerations**:
	- Tensor core utilization
	- Memory bandwidth
	- Pipeline efficiency

### 1.11.4 Reconfigurable Hardware
- **SambaNova Architecture**:
	- PCUs (Processing Core Units)
	- Distributed memory (PMUs)
	- Reconfigurable dataflow
- **Programming Model**:
	- Dataflow patterns
	- Metapipelining
	- Decoupled access/execute
	- Double buffering
- **Benefits**:
	- Simplified programming
	- Efficient pipelining
	- Reduced synchronization

## 1.12 Cache Coherence

### 1.12.1 Cache Basics
- **Purpose**: Automatically improve locality
- **Types of Locality**:
	- Temporal: Repeated access to same address
	- Spatial: Access to nearby addresses
- **Cache Line**: Contiguous chunk loaded together
	- Improves spatial locality
	- Common sizes: 32-128 bytes

### 1.12.2 Cache Misses
- **Cold Miss**: First access to address
- **Capacity Miss**: Cache full, had to evict
- **Conflict Miss**: Address mapping conflict
	- Due to set associativity limits

### 1.12.3 Cache Associativity Types
- **Direct Mapped (1-way)**:
	- One possible cache location
	- Based on address % cache size
	- Simple but more conflicts
- **Fully Associative**:
	- Can use any cache location
	- Best miss rate but expensive
	- Needs many comparators
- **N-way Set Associative**:
	- Compromise approach
	- N possible locations per set
	- Common: 2-way, 4-way, 8-way

### 1.12.4 Cache Design Features
- **Cache Line Contents**:
	- Dirty bit
	- Line state
	- Tag
	- Data bytes
- **Write Policies**:
	- Write-back: Update memory on eviction
	- Write-through: Update memory immediately
	- Write-allocate: Load line on write miss
	- No-write-allocate: Write directly to memory

### 1.12.5 Cache Coherence Problem
- Multiple processors see different values
- Caused by local cache copies
- Need coherence protocol to maintain consistency
- **Requirements**:
	- Latest write visible to all readers
	- Writes serialized across processors
	- Cache state coordination

### 1.12.6 Cache Coherence Solutions

#### 1.12.6.1 Shared Cache Approach
- Single cache shared by all processors
- Pros: Simple coherence
- Cons:
	- Higher latency (further from CPU)
	- Limited bandwidth
	- Destructive interference between processors

#### 1.12.6.2 Snooping Cache Coherence
- Broadcast coherence messages to all processors
- Cache controllers monitor ("snoop") memory operations
- Write-through vs Write-back protocols
- Key bus properties:
	- Broadcast medium - all see transactions
	- Serialized access - ordered operations

#### 1.12.6.3 MSI Protocol States
- Modified (M): Exclusive dirty copy
- Shared (S): Clean copy, others may have
- Invalid (I): Line not present/valid
- Bus transactions:
	- BusRd: Read request
	- BusRdX: Read exclusive
	- BusWB: Write back dirty data

#### 1.12.6.4 MESI Protocol
- Adds Exclusive (E) clean state
- Allows silent transition E->M
- More efficient for private data
- Common in modern processors

#### 1.12.6.5 Directory-Based Coherence
- Avoids broadcast with central directory
- Tracks cache line state and sharers
- Point-to-point messages vs broadcast
- More scalable for many processors

#### 1.12.6.6 False Sharing Issues
- Multiple processors access same cache line
- But different data within line
- Causes unnecessary coherence traffic
- Solutions:
	- Pad data structures
	- Align to cache line boundaries
	- Separate frequently written data

### 1.12.7 Synchronization and Locking

#### 1.12.7.1 Deadlock
- System state where no operation can make progress
- Each operation holds resources needed by others
- Required conditions:
	- Mutual exclusion
	- Hold and wait
	- No preemption
	- Circular wait

#### 1.12.7.2 Livelock and Starvation
- **Livelock**: Threads execute but make no progress
	- Example: Cars repeatedly moving to let others pass
- **Starvation**: Some threads unable to make progress
	- Often due to unfair resource allocation

#### 1.12.7.3 Test-and-Set Locks
- Atomic read-modify-write operation
- Basic implementation:
	- Spin until lock value is 0
	- Atomically set to 1 to acquire
- Issues:
	- High cache contention
	- Poor scalability with many processors

#### 1.12.7.4 Test-and-Test-and-Set Lock
- Optimized version that reduces bus traffic
- Read lock value in shared state
- Only attempt test-and-set when lock appears free
- Better cache behavior but still has contention on release

#### 1.12.7.5 Ticket Lock
- Fair queuing system for lock acquisition
- Two counters:
	- next_ticket: Atomically incremented to get position
	- now_serving: Indicates current holder
- Properties:
	- FIFO ordering
	- One write per lock acquisition
	- Efficient spinning on reads

#### 1.12.7.6 Compare and Swap (CAS)
- More general atomic operation
- Atomically checks and updates value
- Used to implement:
	- Lock-free data structures
	- Atomic operations (e.g., atomicMin)
	- Efficient spin locks

#### 1.12.7.7 Fine-Grained Locking
- **Hand-over-hand locking**:
	- Lock per data structure node
	- Acquire next lock before releasing current
	- Good for linked data structures
- Benefits:
	- Better concurrency
	- Reduced contention
- Challenges:
	- More complex implementation
	- Deadlock prevention needed
	- Higher overhead per operation

#### 1.12.7.8 Lock-Free Algorithms
- Non-blocking algorithm where at least one thread makes progress
- No thread can indefinitely block others
- System-wide progress guaranteed even with preemption
- Examples:
	- Lock-free stack using CAS
	- Lock-free queue with separate push/pop threads
	- Lock-free linked list

#### 1.12.7.9 Memory Coherence Vs Consistency
- **Cache Coherence:**
	- **Maintains consistency for single location**
	- **Makes caches transparent to program**
	- **Only needed with shared memory caches**
	- **Ensures writes to same address are seen by all processors**
- **Memory Consistency:**
	- **Defines ordering across all locations**
	- **Required in any parallel system**
	- **Affects compiler and hardware optimizations**
	- **Governs reads/writes to different addresses**

#### 1.12.7.10 Memory Consistency Models
- **Sequential Consistency**:
	- Operations appear in program order
	- All threads see same global ordering
	- Most intuitive but restrictive model
	- Limits hardware/compiler optimizations

- **Total Store Order (TSO)**:
	- Allows reads to bypass earlier writes
	- Uses write buffers for performance
	- Used by x86 processors
	- Properties:
	 - Writes by same processor seen in order
	 - Once a write is visible, all processors see it
	 - Reads can bypass pending writes to different addresses

- **Relaxed Consistency**:
	- Allows reordering of independent operations
	- Types of relaxations:
	 - Read after write reordering
	 - Write after write reordering
	 - Write after read reordering
	- Benefits:
	 - Better single-thread performance
	 - More efficient write buffering
	 - Hardware optimization opportunities
	- Challenges:
	 - Harder to reason about
	 - Requires explicit synchronization
	 - Can lead to subtle bugs

#### 1.12.7.11 Data Races and Synchronization
- Data races occur with concurrent access
- Prevention requires:
	- Proper synchronization (locks, fences)
	- Memory fence instructions
	- Atomic operations
	- Following memory model rules
- Synchronized programs yield sequentially consistent results
- Well-synchronized code behaves predictably across models

### 1.12.8 Transactional Memory
- **Definition**: Higher-level synchronization primitive
- **Key Properties (A.I.S.)**:
	- Atomicity: All operations execute as one unit
	- Isolation: No partial results visible
	- Serializability: Equivalent to some sequential order
- **Advantages**:
	- Simpler than fine-grained locking
	- Automatic deadlock avoidance
	- Composable operations
	- Only serializes conflicting transactions

#### 1.12.8.1 Implementation Approaches
- **Data Versioning**:
	- Eager (undo-log based):
	 - Write immediately to memory
	 - Keep log to undo on abort
	- Lazy (write-buffer based):
	 - Buffer writes until commit
	 - Discard buffer on abort

- **Conflict Detection**:
	- Pessimistic (eager):
	 - Check conflicts immediately
	 - Good for high-conflict scenarios
	 - Can lead to livelocks
	- Optimistic (lazy):
	 - Check at commit time
	 - Better progress guarantees
	 - May waste work on conflicts

#### 1.12.8.2 Trade-offs
- **Pessimistic Detection**:
	- Pros: Early conflict detection
	- Cons: More communication overhead, possible livelocks
- **Optimistic Detection**:
	- Pros: Better progress, bulk conflict checks
	- Cons: Late detection, wasted work
- **Usage Considerations**:
	- Best for low-conflict scenarios
	- Not suitable when immediate visibility needed
	- Requires careful transaction sizing

#### 1.12.8.3 Software TM Implementation
- **Runtime Data Structures**:
	- Transaction descriptor (per-thread):
	 - Tracks read/write sets
	 - Contains undo log/write buffer
	 - Used for conflict detection
	- Transaction record (per data):
	 - Guards shared data access
	 - Tracks transactional state
	 - Similar to cache coherence

- **Conflict Detection Granularity**:
	- Object-level: Coarse-grained, false conflicts
	- Field-level: Fine-grained, more precise
	- Trade-off between overhead and accuracy

#### 1.12.8.4 Intel McRT STM Example
- **Key Properties**:
	- Eager versioning (immediate writes)
	- Optimistic reads (no locks)
	- Pessimistic writes (with locks)
- **Version Tracking**:
	- Global timestamp for commits
	- Local timestamp per transaction
	- Transaction records track locks/versions
- **Operations**:
	- Read: Direct memory access, validate version
	- Write: Lock data, log undo, write memory
	- Commit: Update global timestamp, validate reads
	- Validation: Check read set consistency

#### 1.12.8.5 Hardware TM (HTM)
- **Implementation**:
	- Uses cache for versioning
	- Extends coherence for conflicts
	- Checkpoints register state
- **Cache Extensions**:
	- Read/Write bits per line
	- Tracks transaction sets
	- Cleared on commit/abort
- **Conflict Detection**:
	- Via coherence messages
	- Read-write conflicts
	- Write-write conflicts
	- Write-read conflicts

#### 1.12.8.6 HTM Vs STM Trade-offs
- **HTM Advantages**:
	- Lower overhead
	- Hardware-speed detection
	- Strong atomicity possible
- **STM Advantages**:
	- More flexible policies
	- Unlimited transaction size
	- No hardware constraints
- **Common Limitations**:
	- Abort handling overhead
	- Progress guarantees
	- System call handling
