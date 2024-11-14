# CS149 Parallel Computing Cheatsheet

## 1. Fundamentals of Parallelism

### Why Parallelism?
- Parallel computer: Collection of processing elements cooperating to solve problems quickly
- Speedup = Time(1 processor) / Time(P processors)
- Limitations:
	- Communication between processors
	- Imbalance in work assignments 
	- Overhead costs

### Amdahl's Law
- If S = fraction of sequential execution
- Maximum speedup with infinite processors ≤ 1/S
- Dependencies limit maximum possible speedup
- Maximum speedup given P processors ≤ 1/(S + (1-S)/P)

### Key Themes
1. **Scale**: Design parallel programs that scale well
	- Decompose work safely
	- Assign work efficiently
	- Manage communication/synchronization
2. **Hardware Understanding**: Know how parallel computers work
3. **Efficiency**: Fast ≠ Efficient (2x speedup on 10 processors is poor)

## 2. Modern Processor Architecture

### Basic Components
1. **Control** (fetch/decode): Determines next instruction
2. **Execution unit** (ALU): Performs operations
3. **State** (execution context): Maintains program state

### Memory Hierarchy
1. **Registers**: Fastest, smallest
2. **Cache Levels** (L1, L2, L3):
	- Each larger but slower than previous
	- Pulled in contiguous chunks (cache lines)
3. **Main Memory**: Largest, slowest

### Cache Concepts
- **Cache Line**: Contiguous chunk of memory loaded together
- **Cache Policies**: Usually LRU (Least Recently Used)
- **Locality Types**:
	- Spatial: Benefits from cache line loading
	- Temporal: Benefits from repeated access
- **Miss Types**:
	- Cold miss: First access
	- Capacity miss: Cache full
	- Conflict miss: Address mapping conflicts

### Modern Processor Features
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

## 3. Parallel Programming Models

### Thread-based Parallelism
- Each thread has private memory
- Threads communicate via shared memory
- Synchronization needed for coordination

### Data Parallel Programming
- Operations on sequences of data
- Common operations:
	- map: Apply function to each element
	- reduce: Combine elements using binary operation
	- scan: Running totals like prefix sum
	- filter: Select elements meeting criteria

### CUDA Programming
- Hierarchy:
	1. **Thread**: Finest level of execution, runs a single instance of the kernel function. Has its own registers and local memory.
	2. **Warp**: Group of 32 threads that execute in SIMD fashion (Single Instruction Multiple Data). All threads in a warp execute the same instruction simultaneously on different data. When threads in a warp diverge (e.g., due to if/else), the warp executes each branch path sequentially with some threads inactive, reducing performance.
	3. **Block**: Collection of threads (organized into warps) that can cooperate via shared memory and synchronization primitives. Limited by hardware resources.
	4. **Grid**: Array of thread blocks that can be distributed across multiple streaming multiprocessors (SMs). Enables program to scale across different GPU capabilities.
- Memory Types:
	- Global: Accessible by all threads
	- Shared: Per-block memory
	- Local: Per-thread memory

## 4. Optimization Techniques

### Work Distribution
1. **Static Assignment**
	- Fixed work distribution
	- Good when work is predictable
	- Low runtime overhead

2. **Dynamic Assignment**
	- Work stolen/redistributed as needed
	- Better for unpredictable workloads
	- Higher overhead

### Communication Optimization
1. Minimize data movement
2. Use local memory when possible
3. Batch communications
4. Overlap computation with communication

### Cache Coherence
- Ensures consistent view of memory across processors
- Protocols:
	- MSI (Modified, Shared, Invalid)
	- MESI (Modified, Exclusive, Shared, Invalid)
- Solutions:
	1. Snooping-based
	2. Directory-based

## 5. Performance Considerations

### Memory Bandwidth
- Often the limiting factor
- Calculate arithmetic intensity:
	- Operations per byte of memory accessed
- Use roofline model to identify bottlenecks

### Load Balancing
- Ensure even work distribution
- Methods:
	1. Block distribution
	2. Cyclic distribution
	3. Dynamic scheduling

### Synchronization
- Minimize use of locks
- Use barriers only when necessary
- Consider lock-free alternatives
- Avoid false sharing

### Utilization Analysis
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
			- Application needs: 80 GB/s
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

### Throughput and Bandwidth Calculations

#### Peak Throughput
- **Basic Formula**: 
	Peak FLOPS = Cores × SIMD Width × Clock Speed × Operations per cycle
	- Hardware threads don't increase peak FLOPS, they only help hide latency

- **Example**:
	- Hardware: 8 cores running at 3.2 GHz
	- Each core has 16-wide SIMD
	- Can do 2 Fused Multiply-Add (FMA) operations per cycle
	- Total Peak = 8 cores × 16 floats × 3.2 billion cycles/sec × 2 ops = 819.2 GFLOPS

#### Memory Bandwidth Requirements
- **Basic Formula**: 
	Required Memory Bandwidth = Data accessed per unit time
	= (Bytes needed × Operations per second) / Operations per data access
	= (# of cores × clock) × (SIMD width × bytes per cycle)

- **Real Example**: Vector Addition Program
	- System: 8 cores, 32-wide SIMD, 1 GHz clock
	- Maximum compute: 8 × 32 × 1G = 256 billion ops/sec
	- Memory pattern: Read 12 bytes every 30 compute cycles
	- Required bandwidth = 256G × (12/30) = 102.4 GB/sec

#### Arithmetic Intensity (Computing vs Memory Ratio)
- **Formula**:
	Arithmetic Intensity = Compute Operations / Memory Bytes Accessed
	- Higher is better - means more computation per memory access

- **Real Example**: Simple Matrix Multiplication
	- Each FMA operation: 2 floating point operations
	- Needs to access: 12 bytes of data (3 4-byte floats)
	- Arithmetic Intensity = 2/12 = 0.167 operations/byte
	- This low ratio indicates memory-bound behavior

#### Memory-Limited Performance
- When memory bandwidth is the bottleneck:
	Actual Performance = Available Memory Bandwidth × Arithmetic Intensity
	- Example: With 50 GB/s bandwidth and AI of 0.167:
		Maximum achievable = 50 GB/s × 0.167 = 8.35 GFLOPS

## 6. Common Patterns

### Map-Reduce
- Map: Transform each element independently
- Reduce: Combine results using associative operation
- Good for data-parallel operations

### Fork-Join
- Fork: Spawn parallel tasks
- Join: Wait for completion
- Good for recursive parallelism

### Pipeline Parallelism
- Break computation into stages
- Each stage processes different data
- Good for streaming computations

## 7. Distributed Systems

### HDFS Architecture
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

### Specialized Hardware
- **Double Buffering**:
	- Producer writes to one buffer
	- Consumer reads from other buffer
	- Enables parallel operation
- **Metapipelining**:
	- Multiple pipeline stages run in parallel
	- Each stage can process different data