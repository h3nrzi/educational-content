# Computer Architecture (High-Level Overview)

## Main Components of a Computer (Software Perspective)

1. **Disk (Storage, Hard Drive, HDD/SSD)**

   - **Purpose**: Stores data persistently, meaning data remains even if the computer restarts or crashes.
   - **Types**:
     - **HDD (Hard Disk Drive)**: Traditional storage, slower.
     - **SSD (Solid State Drive)**: Faster, more common today.
   - **Capacity**: Measured in hundreds of gigabytes (GB) or terabytes (TB).
     - **1 byte = 8 bits** (each bit is a 0 or 1, used to represent data like characters, programs, or videos).
     - **1 GB = 10^9 bytes** (billion bytes).
     - **1 TB = 10^12 bytes** (trillion bytes).
   - **Key Feature**: Persistent storage ensures files (e.g., documents, games) are saved even after a crash.

2. **RAM (Random Access Memory)**

   - **Purpose**: Temporary storage for data and programs being actively used.
   - **Capacity**: Smaller than disk, typically 2–32 GB (or more in high-end systems).
   - **Speed**: Much faster than disk.
     - **Read/Write Time**: Measured in **microseconds (10^-6 seconds)**.
     - **Disk Read/Write Time**: Measured in **milliseconds (10^-3 seconds)**, making RAM 10–1000 times faster.
   - **Cost**: More expensive than disk per unit of storage.
   - **Limitation**: Non-persistent; data is lost if the computer crashes or restarts.

3. **CPU (Central Processing Unit)**

   - **Purpose**: The "brain" of the computer, responsible for:
     - Executing code by reading instructions from RAM.
     - Reading/writing data to/from RAM and disk.
     - Performing computations (e.g., arithmetic operations like addition/subtraction).
   - **Role in Code Execution**:
     - Code (written in high-level languages) is compiled into machine instructions (0s and 1s) that the CPU understands.
     - CPU processes both code (instructions) and data (e.g., variables like arrays) stored in RAM.
   - **Key Feature**: Manages communication between RAM and disk, as they cannot directly interact.

4. **Cache**
   - **Purpose**: A small, fast memory component within the CPU to speed up read/write operations from RAM.
   - **Capacity**: Measured in megabytes (MB), much smaller than RAM or disk.
   - **Speed**: Operates in **nanoseconds (10^-9 seconds)**, significantly faster than RAM.
   - **Function**: Stores frequently accessed portions of RAM to reduce access time.
   - **Limitation**: Non-persistent and expensive, so limited in size.
   - **Optimization**: CPU checks cache first for data; if not found, it accesses RAM (slower), then may store data in cache for future use.

## Key Concepts

- **Data Representation**: All data (code, files, videos) is stored as 0s and 1s (bits/bytes) with different abstractions for interpretation.
- **Speed Hierarchy**: Cache (nanoseconds) > RAM (microseconds) > Disk (milliseconds).
- **Persistence**: Disk is persistent; RAM and cache are not.
- **CPU Limitations**: Governed by hardware constraints, notably Moore’s Law.
  - **Moore’s Law**: Observation by Gordon Moore (Intel co-founder) that the number of transistors in a CPU (a proxy for speed) doubles every two years, with costs halving.
  - **Reality Check**: CPU speed growth is plateauing (past ~10 years), limiting single-computer performance.

## Distributed Systems Connection

- A single computer (with disk, RAM, CPU, cache) is a building block for distributed systems.
- **Limitations of a Single Computer**:
  - Finite storage (disk), memory (RAM), and processing power (CPU).
  - Moore’s Law is slowing, so CPU speed increases are not infinite.
- **Solution**: Combine multiple computers in a distributed system to overcome these limitations and solve larger problems.

## Questions for Review

1. What is the main difference between persistent and non-persistent storage in a computer, and which components provide each type?
2. Why is RAM faster than disk, and how does this impact the way a computer processes data?
3. How does the CPU interact with RAM and disk to execute code and manage data?
4. What role does the cache play in improving computer performance, and why is it limited in size?
5. How does Moore’s Law relate to CPU performance, and why is it significant that its trend is plateauing?
