# Basics of Designing Large-Scale Systems

## Overview

System design focuses on creating effective architectures to solve massive problems, especially in the context of system design interviews. The emphasis is on **trade-offs** rather than perfect solutions—analyzing improvements, sacrifices, and thought processes. Interviews don't require memorizing details but evaluating options. At a high level, system design revolves around three core operations:

- **Moving data**: Transferring data between components (e.g., within a computer or across global machines via networks).
- **Storing data**: Persisting data efficiently, considering trade-offs like persistence (disk vs. RAM) and structures (e.g., arrays vs. binary search trees; databases vs. blob stores).
- **Transforming data**: Manipulating data (e.g., aggregating logs to calculate success rates) using application code.

These operations scale to handle massive data volumes, drawing parallels to data structures and algorithms for efficiency. Poor architectural choices (e.g., wrong database) are hard to fix later, requiring data migration and code rewrites—focus on high-level design over perfect code.

## Key Measurements and Trade-Offs

Systems are evaluated using metrics like availability, reliability, throughput, and latency. Improvements in one often trade off with others (e.g., complexity vs. scalability).

### 1. Availability

- **Definition**: Percentage of time the system is operational and responsive (uptime / total time).
  - Example: 23/24 hours up = 96% availability.
- **"Nines" Measurement**: Expressed as number of 9s in uptime percentage.
  | Nines | Availability | Annual Downtime |
  |-------|--------------|-----------------|
  | 2 | 99% | ~3.65 days |
  | 3 | 99.9% | ~8.76 hours |
  | 4 | 99.99% | ~52.6 minutes |
  | 5 | 99.999% | ~5.26 minutes |
- **Challenges**: 100% is impossible (e.g., disasters, attacks). Reducing downtime by 10x (e.g., 1% to 0.1%) boosts availability from 99% to 99.9%, but gains diminish at higher nines.
- **Related Concepts**:
  - **SLO (Service Level Objective)**: Internal target (e.g., aim for 5 nines availability for a database).
  - **SLA (Service Level Agreement)**: Customer-facing contract including SLO + consequences (e.g., AWS refunds if below target).

### 2. Reliability, Fault Tolerance, and Redundancy

- **Reliability**: Probability the system won't fail (e.g., single server is less reliable than multiple).
- **Fault Tolerance**: System continues functioning despite partial failures (e.g., one server crashes, others handle load).
- **Redundancy**: Duplicate components (e.g., extra servers running identical code) to enable fault tolerance and boost reliability.
- **Interconnections**: Redundancy → Fault Tolerance → Higher Reliability → Better Availability.
- **Examples**:
  - Single server: Vulnerable to crashes/DDoS (Distributed Denial of Service) attacks.
  - Vertical scaling (upgrade one server): Improves availability/throughput but not reliability (still single point of failure).
  - Horizontal scaling (add servers): Increases reliability/redundancy but adds complexity (e.g., load balancing).
- **Enhancements**: Place redundant servers in different data centers to avoid shared failures (e.g., hurricanes).

### 3. Throughput

- **Definition**: Amount of operations/data processed over time (e.g., requests/queries per second or bytes per second).
- **Contexts**:
  - **Requests/Queries per Second (RPS/QPS)**: For servers/databases (e.g., handle 100 RPS peak for 1,000 users).
  - **Bytes per Second**: For data pipelines (e.g., process 1 GB/s; 1 TB data takes ~1,000 seconds).
- **Scaling Trade-Offs**:
  - Vertical: Easier but limited (Moore's Law plateauing; single point of failure).
  - Horizontal: More scalable but complex (e.g., data sync across distributed databases; outdated data risks).

### 4. Latency

- **Definition**: Time for a single operation to complete (end-to-end, from user perspective; e.g., 1 second for request-response).
- **vs. Throughput**: Throughput = operations/time; Latency = time/operation. Low latency enables higher throughput.
- **Causes**: Network distance, processing delays (e.g., cache reads in nanoseconds vs. RAM in microseconds).
- **Reductions**:
  - Global servers (reduces user distance).
  - CDNs (Content Delivery Networks) for caching content closer to users.
- **Trade-Offs**: Multi-server setups improve latency/throughput/availability but increase complexity.

## Key Takeaways

- **Goal**: Build fault-tolerant, high-throughput, low-latency systems that handle failures and scale efficiently.
- **System Design Mindset**: Focus on data movement/storage/transformation; weigh trade-offs (e.g., simplicity vs. redundancy); use metrics to evaluate.
- **Applications**: Applies to real-world systems like Twitter (move/store/transform tweets) or YouTube (videos/comments/views).
- Upcoming: Deeper dives into networking, databases, and techniques like CDNs.

## Questions for Review

1. What are the three core operations in system design, and how do they relate to building applications like YouTube?
2. Explain the difference between SLO and SLA, and provide an example of how an SLA might include consequences for failing to meet availability targets.
3. How do reliability, fault tolerance, and redundancy interrelate, and why might placing redundant servers in different data centers improve a system?
4. Compare throughput and latency: How can reducing latency (e.g., via caching or global servers) impact overall system performance?
5. Why is vertical scaling simpler than horizontal scaling for improving throughput, but what are its key limitations?
