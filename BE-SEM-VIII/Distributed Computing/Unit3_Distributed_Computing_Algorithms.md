# Unit III: Distributed Computing Algorithms — Exam-Ready Answers

## Q1) What is Consensus Algorithm? Explain any one algorithm (RAFT). [6 marks]

### Definition of Consensus Algorithm

A **consensus algorithm** is a protocol used in distributed systems to achieve agreement among multiple nodes on a single data value or system state, even in the presence of failures. It ensures that all non-faulty nodes agree on the same value, maintaining consistency across the distributed system.

**Key Properties of Consensus:**

1. **Agreement** – All correct nodes decide on the same value.
2. **Validity** – The decided value must have been proposed by some node.
3. **Termination** – Every correct node eventually decides.
4. **Integrity** – Each node decides at most once.

### RAFT Consensus Algorithm

RAFT is a consensus algorithm designed to be more understandable than Paxos while providing the same guarantees.

**Key Concepts:**

| Component | Description                                             |
| --------- | ------------------------------------------------------- |
| Leader    | Handles all client requests and log replication         |
| Follower  | Passive nodes that respond to Leader/Candidate requests |
| Candidate | Node seeking to become a Leader                         |
| Term      | Logical clock; increases with each election             |

**Three Sub-problems RAFT Solves:**

**1. Leader Election:**

- All nodes start as Followers.
- If a Follower receives no heartbeat within a timeout, it becomes a Candidate.
- Candidate increments its term and requests votes from other nodes.
- A node receiving a majority of votes becomes the Leader.
- Only one leader per term is possible.

**2. Log Replication:**

- Leader receives client requests and appends them to its log.
- Leader sends AppendEntries RPCs to all Followers.
- Once a majority acknowledges, the entry is committed.
- Leader notifies Followers to apply committed entries.

**3. Safety:**

- A Candidate cannot win an election unless its log is at least as up-to-date as a majority.
- This ensures the Leader always has all committed entries.

**Diagram:**

```
Client Request → Leader → AppendEntries → Followers
                   ↓
             Majority ACK
                   ↓
              Commit Entry
```

**Advantages of RAFT:**

- Easier to understand than Paxos
- Strong leader model simplifies log replication
- Clear separation of concerns (election, replication, safety)

---

## Q2) List and explain any one variant of Paxos in detail / Describe how it differs from original Paxos. [6 marks]

### Original Paxos (Brief Overview)

Paxos achieves consensus through three roles:

- **Proposer** – Proposes values
- **Acceptor** – Votes on proposals
- **Learner** – Learns the decided value

Original Paxos has two phases:

- **Phase 1 (Prepare):** Proposer sends prepare request with proposal number; Acceptors promise not to accept lower-numbered proposals.
- **Phase 2 (Accept):** Proposer sends accept request; Acceptors accept if no higher-numbered prepare seen.

### Fast Paxos (Variant)

**Fast Paxos** reduces message delays from 3 to 2 in the common case by allowing clients to send proposals directly to Acceptors.

**Key Differences from Original Paxos:**

| Aspect            | Original Paxos                        | Fast Paxos                   |
| ----------------- | ------------------------------------- | ---------------------------- |
| Message Delays    | 3 (Client→Proposer→Acceptors→Learner) | 2 (Client→Acceptors→Learner) |
| Leader Role       | Always mediates                       | Bypassed in fast round       |
| Quorum Size       | Majority (n/2 + 1)                    | Larger quorum (2n/3 + 1)     |
| Conflict Handling | Leader resolves                       | Falls back to classic round  |

**How Fast Paxos Works:**

1. **Fast Round:** Leader sends a special "any" proposal allowing Acceptors to accept any value directly from clients.
2. **Client Proposal:** Client sends proposed value directly to Acceptors.
3. **Acceptors** accept the first value they receive and inform Learners.
4. **If no conflict:** Value is decided in 2 message delays.
5. **If conflict occurs:** Leader detects it and initiates a classic Paxos round to resolve.

**Advantages:**

- Lower latency in conflict-free scenarios
- Better performance for geo-distributed systems

**Disadvantages:**

- Requires larger quorums
- More complex conflict resolution
- Falls back to classic Paxos on conflicts

---

## Q3) Explain Fault Tolerance and Recovery in context of Distributed Systems. [6 marks]

### Fault Tolerance

**Definition:** Fault tolerance is the ability of a distributed system to continue functioning correctly even when some of its components fail.

### Types of Faults

| Fault Type      | Description                          | Example              |
| --------------- | ------------------------------------ | -------------------- |
| Crash Fault     | Node stops functioning               | Server power failure |
| Byzantine Fault | Node behaves arbitrarily/maliciously | Corrupted messages   |
| Network Fault   | Communication links fail             | Network partition    |
| Timing Fault    | Node responds outside expected time  | Overloaded server    |

### Fault Tolerance Techniques

**1. Replication:**

- Data/services are replicated across multiple nodes.
- If one replica fails, others continue serving requests.
- Types: Active replication (all process requests) and Passive replication (primary-backup).

**2. Redundancy:**

- Hardware redundancy: Multiple servers, network links.
- Software redundancy: Multiple implementations of same service.
- Time redundancy: Retry failed operations.

**3. Consensus Protocols:**

- Algorithms like RAFT and Paxos ensure agreement despite failures.
- Tolerate up to (n-1)/2 crash failures with n nodes.

### Recovery Mechanisms

**1. Checkpointing:**

- Periodically saving system state to stable storage.
- On failure, system restores from the last checkpoint.
- Types: Coordinated checkpointing (all nodes checkpoint together) and Uncoordinated checkpointing (independent).

**2. Logging:**

- Write-Ahead Logging (WAL): Log operations before executing them.
- On recovery, replay the log from last checkpoint.

**3. Rollback Recovery:**

- System rolls back to a consistent state after failure.
- Uses checkpoints + message logs to reconstruct state.

**4. Replication-based Recovery:**

- Failed node recovers state from healthy replicas.
- State transfer or log replay from other nodes.

**Diagram:**

```
Normal Operation → Failure Detected → Recovery Initiated
       ↓                                      ↓
  Checkpointing                    Restore from Checkpoint
  + Logging                        + Replay Logs
                                          ↓
                                   Resume Operation
```

---

## Q4) Compare Centralized Load Balancing & Distributed Load Balancing Techniques. [6 marks]

### Centralized Load Balancing

A single central node (load balancer) makes all decisions about distributing workload across servers.

**Working:** All client requests go through the central load balancer, which decides which server handles each request based on algorithms like Round Robin or Least Connection.

### Distributed Load Balancing

Multiple nodes collaboratively make load distribution decisions without a single point of control.

**Working:** Each node independently makes routing decisions based on local and shared state information.

### Comparison Table

| Parameter                   | Centralized Load Balancing             | Distributed Load Balancing                     |
| --------------------------- | -------------------------------------- | ---------------------------------------------- |
| **Control**                 | Single central controller              | Multiple cooperating nodes                     |
| **Single Point of Failure** | Yes – if balancer fails, system fails  | No – failure of one node doesn't affect others |
| **Scalability**             | Limited – bottleneck at central node   | High – scales horizontally                     |
| **Decision Making**         | Global view; optimal decisions         | Local/partial view; near-optimal decisions     |
| **Consistency**             | Easier to maintain consistent state    | Requires synchronization protocols             |
| **Complexity**              | Simple to implement                    | Complex coordination needed                    |
| **Latency**                 | Additional hop through balancer        | Lower – direct routing possible                |
| **Examples**                | NGINX, HAProxy, AWS ELB                | DNS-based, Consistent Hashing, Gossip-based    |
| **Overhead**                | Communication overhead to central node | Message exchange between all nodes             |
| **Suitable For**            | Small to medium systems                | Large-scale distributed systems                |

### When to Use

- **Centralized:** When global optimization is needed, system is small/medium, simplicity is preferred.
- **Distributed:** When high availability is critical, system is large-scale, no single point of failure is acceptable.

---

## Q5) Compare Weighted Round Robin and Least Connection Load Balancing. [6 marks]

### Weighted Round Robin (WRR)

**Definition:** An extension of Round Robin where each server is assigned a weight proportional to its capacity. Servers with higher weights receive more requests.

**Working:**

1. Assign weights to servers based on capacity (e.g., Server A=5, Server B=3, Server C=2).
2. Distribute requests proportionally – A gets 5 out of every 10 requests.
3. Cycle repeats after all weights are exhausted.

**Example:**

```
Servers: A(weight=3), B(weight=1), C(weight=1)
Request sequence: A, A, A, B, C, A, A, A, B, C...
```

### Least Connection

**Definition:** Routes each new request to the server with the fewest active connections at that moment.

**Working:**

1. Monitor active connections on each server in real-time.
2. New request → assigned to server with minimum active connections.
3. Connection count updated dynamically.

### Comparison Table

| Parameter             | Weighted Round Robin                   | Least Connection                        |
| --------------------- | -------------------------------------- | --------------------------------------- |
| **Basis of Decision** | Pre-assigned static weights            | Real-time active connections            |
| **Adaptability**      | Static – doesn't adapt to current load | Dynamic – adapts to real-time load      |
| **Server Capacity**   | Handled via weight assignment          | Not inherently capacity-aware           |
| **Overhead**          | Low – no monitoring needed             | Medium – must track active connections  |
| **Best For**          | Predictable, uniform request workloads | Variable request processing times       |
| **Fairness**          | May overload if request sizes vary     | Better distribution under varying loads |
| **Implementation**    | Simple counter-based                   | Requires connection tracking            |
| **Starvation**        | No starvation (all get turns)          | Low-capacity servers may get overloaded |

### Performance with Varying Capacities and Network Loads

- **WRR** performs well when request processing times are uniform but servers have different capacities (weights model this).
- **Least Connection** performs better when request processing times vary significantly (long-lived vs short-lived connections).
- **Combined approach:** Weighted Least Connection uses both weights AND active connections for optimal distribution.

---

## Q6) Explain Genetic Algorithms for Task Scheduling. [6 marks]

### Definition

A **Genetic Algorithm (GA)** is a metaheuristic optimization technique inspired by natural evolution (selection, crossover, mutation) applied to find optimal or near-optimal task-to-resource mappings in distributed systems.

### How GA Works for Task Scheduling

**Step 1: Encoding (Chromosome Representation)**

- Each solution (schedule) is encoded as a chromosome.
- Example: For 5 tasks and 3 processors:
  - Chromosome: [2, 1, 3, 1, 2] means Task1→P2, Task2→P1, Task3→P3, Task4→P1, Task5→P2

**Step 2: Initial Population**

- Generate random valid schedules as initial population.
- Population size typically 50-200 chromosomes.

**Step 3: Fitness Evaluation**

- Evaluate each chromosome using fitness function.
- Fitness = 1 / Makespan (total completion time)
- Lower makespan = higher fitness = better schedule.

**Step 4: Selection**

- Select fitter chromosomes for reproduction.
- Methods: Roulette wheel, Tournament selection.

**Step 5: Crossover**

- Combine two parent chromosomes to create offspring.
- Example (Single-point crossover):
  ```
  Parent 1: [2, 1, | 3, 1, 2]
  Parent 2: [1, 3, | 2, 2, 1]
  Child 1:  [2, 1, | 2, 2, 1]
  Child 2:  [1, 3, | 3, 1, 2]
  ```

**Step 6: Mutation**

- Randomly change task assignment to maintain diversity.
- Example: [2, 1, 3, 1, 2] → [2, 1, **1**, 1, 2] (Task 3 moved to P1)

**Step 7: Repeat** until convergence or maximum generations reached.

### Benefits of GA for Task Scheduling

1. **Global Optimization** – Avoids local optima through mutation and crossover.
2. **Parallelizable** – Population can be evaluated in parallel.
3. **Flexible** – Can optimize multiple objectives (makespan, cost, energy).
4. **Scalable** – Handles large search spaces effectively.
5. **No assumptions** – Works without requiring mathematical model of the system.

### Diagram:

```
Initial Population → Fitness Evaluation → Selection → Crossover → Mutation
        ↑                                                              ↓
        ←←←←←←←←←←←← New Generation ←←←←←←←←←←←←←←←←←←←←←←←←←←←←
```

---

## Q7) Explain Reinforcement Learning for Dynamic Load Balancing. [6/9 marks]

### Definition

**Reinforcement Learning (RL)** is a machine learning paradigm where an agent learns optimal actions through trial-and-error interactions with an environment, receiving rewards or penalties for its decisions. Applied to load balancing, the RL agent learns to distribute workloads optimally across servers.

### RL Components in Load Balancing Context

| RL Component    | Load Balancing Mapping                                                             |
| --------------- | ---------------------------------------------------------------------------------- |
| **Agent**       | Load balancer / scheduler                                                          |
| **Environment** | Distributed system (servers, network)                                              |
| **State**       | Current server loads, queue lengths, response times, CPU/memory usage              |
| **Action**      | Assign incoming request to a specific server                                       |
| **Reward**      | Positive for low latency, balanced load; Negative for overload, high response time |
| **Policy**      | Strategy mapping states to optimal actions                                         |

### How RL-based Load Balancing Works

**1. State Observation:**

- Agent observes: server CPU utilization, memory usage, network bandwidth, queue lengths, response times.

**2. Action Selection:**

- Based on current state and learned policy, agent selects which server to assign the request to.
- Uses exploration-exploitation tradeoff (ε-greedy or softmax).

**3. Reward Calculation:**

- Reward function: R = -α(response_time) - β(load_imbalance) + γ(throughput)
- Agent aims to maximize cumulative reward.

**4. Policy Update:**

- Using algorithms like Q-Learning or Deep Q-Network (DQN).
- Q(s, a) ← Q(s, a) + α[r + γ·max Q(s', a') - Q(s, a)]

**5. Continuous Learning:**

- Agent continuously adapts to changing workload patterns.
- No need for manual threshold tuning.

### Advantages over Traditional Methods

| Aspect                | Traditional (e.g., Round Robin) | RL-based                      |
| --------------------- | ------------------------------- | ----------------------------- |
| Adaptability          | Static rules                    | Learns and adapts dynamically |
| Workload Patterns     | Cannot predict                  | Learns patterns over time     |
| Optimization          | Single metric                   | Multi-objective optimization  |
| Configuration         | Manual tuning required          | Self-tuning                   |
| Heterogeneous Servers | Limited handling                | Learns server capabilities    |

### Potential Advantages (for 9-mark answer)

1. **Self-adaptive:** Automatically adjusts to changing traffic patterns without manual intervention.
2. **Proactive:** Can predict load spikes and pre-emptively redistribute.
3. **Multi-dimensional:** Considers multiple metrics simultaneously.
4. **Handles heterogeneity:** Learns different server capabilities.
5. **Continuous improvement:** Performance improves with experience.
6. **Fault-aware:** Can learn to avoid failing servers.

### Diagram:

```
                    ┌─────────────────┐
                    │   RL Agent      │
                    │ (Load Balancer) │
                    └───────┬─────────┘
                   Action   │   ↑ State + Reward
          (Route request)   │   │ (Metrics)
                            ▼   │
              ┌─────────────────────────────┐
              │     Server Cluster           │
              │  [S1] [S2] [S3] ... [Sn]    │
              └─────────────────────────────┘
```

---

## Q8) Apply Load Balancing and Resource Allocation Strategies in Cloud Computing Environment. [6 marks]

### Cloud Computing Load Balancing Challenges

- Varying workloads (unpredictable traffic spikes)
- Heterogeneous resources (different VM types)
- Geographic distribution (multiple data centers)
- Cost optimization (pay-per-use model)

### Implementation Strategy

**1. Multi-tier Load Balancing:**

```
DNS-Level (Geographic) → Application LB (L7) → Server LB (L4)
```

- **DNS Level:** Route users to nearest data center (latency-based routing).
- **Application Level (L7):** Route based on request content (URL, headers).
- **Server Level (L4):** Distribute TCP connections across instances.

**2. Auto-scaling + Load Balancing:**

- Monitor metrics: CPU > 70%, Response time > 500ms, Queue length > threshold.
- Scale OUT: Add more instances when load increases.
- Scale IN: Remove instances when load decreases.
- Load balancer automatically registers/deregisters instances.

**3. Resource Allocation Strategies:**

| Strategy                       | Implementation                                                 |
| ------------------------------ | -------------------------------------------------------------- |
| **Predictive Scaling**         | Use ML models to predict traffic and pre-provision resources   |
| **Spot/Preemptible Instances** | Use cheaper instances for fault-tolerant workloads             |
| **Container Orchestration**    | Kubernetes scheduler allocates pods based on resource requests |
| **Weighted Distribution**      | Assign more traffic to powerful instances                      |

**4. Handling Varying Workloads:**

- **Burst Handling:** Use queue-based decoupling (SQS/Kafka) to buffer spikes.
- **Priority Queuing:** Critical requests get priority routing.
- **Circuit Breaker:** Redirect traffic away from overwhelmed services.
- **Caching:** Reduce backend load with CDN and in-memory caches.

**5. Monitoring and Feedback Loop:**

```
Metrics Collection → Analysis → Decision → Action → Verify
(Prometheus)       (Rules)    (Scale/Route) (Execute) (Health check)
```

---

## Q9) Communication and Coordination in Distributed Systems. [6 marks]

### Communication in Distributed Systems

**Definition:** Communication refers to the exchange of information between distributed nodes through message passing over a network.

**Types of Communication:**

| Type             | Description                               | Example         |
| ---------------- | ----------------------------------------- | --------------- |
| **Synchronous**  | Sender blocks until receiver acknowledges | RPC calls       |
| **Asynchronous** | Sender continues without waiting          | Message queues  |
| **Unicast**      | One-to-one communication                  | TCP connection  |
| **Multicast**    | One-to-many communication                 | Group messaging |
| **Broadcast**    | One-to-all communication                  | ARP requests    |

**Communication Models:**

1. **Message Passing:** Nodes communicate by sending/receiving messages (MPI).
2. **Remote Procedure Call (RPC):** Invoke procedures on remote nodes as if local.
3. **Publish-Subscribe:** Nodes publish events; interested nodes subscribe.
4. **Shared Memory (DSM):** Logical shared memory over distributed nodes.

### Coordination in Distributed Systems

**Definition:** Coordination ensures that distributed nodes work together consistently to achieve a common goal.

**Coordination Mechanisms:**

**1. Mutual Exclusion:**

- Ensures only one process accesses a critical section at a time.
- Algorithms: Lamport's, Ricart-Agrawala, Token-based.

**2. Leader Election:**

- Selecting a single coordinator among distributed nodes.
- Algorithms: Bully algorithm, Ring algorithm.

**3. Clock Synchronization:**

- Logical clocks: Lamport timestamps, Vector clocks.
- Physical clocks: NTP (Network Time Protocol).

**4. Distributed Transactions:**

- Two-Phase Commit (2PC): Prepare → Commit/Abort.
- Three-Phase Commit (3PC): Adds pre-commit phase.

**5. Barriers and Synchronization:**

- Ensure all processes reach a point before any proceeds.

### Challenges

- Network delays and partitions
- No global clock
- Partial failures
- Ordering of events

---

## Q10) Other Consensus Algorithms (Brief Notes for Short Answers)

### Viewstamped Replication

- Leader-based replication protocol.
- Uses "view numbers" to track leader changes.
- Similar to RAFT but predates it.
- Leader replicates operations to replicas; commits after majority acknowledge.

### ZAB (Zookeeper Atomic Broadcast)

- Used in Apache Zookeeper.
- Guarantees total order of all state changes.
- Two modes: Recovery (leader election) and Broadcast (normal operation).
- Leader proposes, followers acknowledge, leader commits.

### Mencius

- Multi-leader Paxos variant.
- Distributes leader role among all nodes in round-robin fashion.
- Reduces latency for geo-distributed systems.
- Each node "owns" certain consensus slots.

### Egalitarian Paxos (EPaxos)

- Leaderless protocol – any replica can propose.
- Achieves optimal commit latency (1 round-trip for non-conflicting commands).
- Uses dependency tracking for ordering.
- Better throughput than leader-based protocols.

---

## Quick Reference: AI Techniques for Distributed Computing

| Technique              | Application              | How It Helps                                       |
| ---------------------- | ------------------------ | -------------------------------------------------- |
| Machine Learning       | Resource Allocation      | Predicts resource needs, reduces waste             |
| Reinforcement Learning | Dynamic Load Balancing   | Self-adapting, learns optimal distribution         |
| Genetic Algorithms     | Task Scheduling          | Near-optimal scheduling in large search spaces     |
| Swarm Intelligence     | Distributed Optimization | Decentralized optimization using agent cooperation |

### Swarm Intelligence for Distributed Optimization (Brief)

- Inspired by collective behavior of ants, bees, birds.
- Algorithms: Ant Colony Optimization (ACO), Particle Swarm Optimization (PSO).
- **ACO for routing:** Agents (ants) explore paths, deposit pheromones on good paths; over time, optimal routes emerge.
- **PSO for resource allocation:** Particles (solutions) move through search space, influenced by personal best and global best positions.
- **Benefits:** Decentralized, scalable, fault-tolerant, self-organizing.

---

## Exam Tips

1. **For 6-mark questions:** Write definition (1 mark) + explanation with points/steps (3-4 marks) + diagram/example (1-2 marks).
2. **For 9-mark questions:** Add more depth, comparison tables, advantages/disadvantages, and real-world examples.
3. **Always draw diagrams** – even simple ones fetch marks.
4. **Use tables** for comparison questions – evaluators can quickly verify points.
5. **Start answers with definitions** – shows clarity of concept.
