# Unit IV: Distributed Machine Learning and AI — Exam-Ready Answers

---

## SECTION A: Theory Topics (Grouped by Similarity)

---

## 1. Introduction to Distributed Machine Learning Algorithms

### Definition

**Distributed Machine Learning (DML)** is the practice of training machine learning models across multiple machines/nodes, distributing computation, data, or both, to handle large-scale datasets and models that cannot fit on a single machine.

### Why Distributed ML?

| Challenge | How DML Solves It |
|-----------|-------------------|
| Data too large for one machine | Split data across nodes (Data Parallelism) |
| Model too large for one GPU | Split model across nodes (Model Parallelism) |
| Training too slow | Parallel computation speeds up training |
| Real-time requirements | Distributed inference for low latency |

---

## 2. Types of Distributed Machine Learning: Data Parallelism and Model Parallelism

### Data Parallelism

**Definition:** The training dataset is split across multiple worker nodes. Each worker has a complete copy of the model and trains on its subset of data.

**Working:**
1. Divide dataset into N partitions (one per worker).
2. Each worker computes gradients on its local data partition.
3. Gradients are aggregated (averaged) across all workers.
4. Model parameters are updated using aggregated gradients.
5. Updated parameters are synchronized across all workers.
6. Repeat until convergence.

**Diagram:**
```
         ┌──────────────────────────────────┐
         │         Full Dataset              │
         └──┬────────┬────────┬────────┬────┘
            ▼        ▼        ▼        ▼
        ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐
        │Data 1│ │Data 2│ │Data 3│ │Data 4│
        │Model │ │Model │ │Model │ │Model │
        │Copy  │ │Copy  │ │Copy  │ │Copy  │
        └──┬───┘ └──┬───┘ └──┬───┘ └──┬───┘
           │        │        │        │
           ▼        ▼        ▼        ▼
         Gradient Gradient Gradient Gradient
           │        │        │        │
           └────────┴────┬───┴────────┘
                         ▼
                  Aggregate Gradients
                         ▼
                  Update Parameters
                         ▼
                  Broadcast to All Workers
```

**Advantages:**
- Simple to implement
- Scales well with data size
- Each worker processes independent data

**Disadvantage:** Communication overhead for gradient synchronization.

### Model Parallelism

**Definition:** The model itself is split across multiple nodes. Each node holds a portion of the model and processes the full data through its portion.

**Working:**
1. Divide model layers/components across workers.
2. Data flows through workers sequentially (pipeline) or in parallel.
3. Each worker computes its portion and passes activations to the next.
4. Backpropagation flows in reverse through the pipeline.

**Diagram:**
```
Input → [Worker 1: Layers 1-3] → [Worker 2: Layers 4-6] → [Worker 3: Layers 7-9] → Output
```

**Advantages:**
- Enables training of very large models (e.g., GPT, BERT)
- Each node needs memory for only part of the model

**Disadvantage:** Sequential dependency can cause idle time (pipeline bubbles).

### Comparison

| Aspect | Data Parallelism | Model Parallelism |
|--------|-----------------|-------------------|
| What is split | Data | Model |
| Model copy | Full model on each worker | Partial model per worker |
| Communication | Gradient synchronization | Activations between layers |
| Best for | Large datasets | Large models |
| Scalability | Easy to scale workers | Limited by model structure |
| Implementation | Simpler | More complex |

---

## 3. Distributed Gradient Descent

### Definition

**Distributed Gradient Descent** extends the standard gradient descent optimization algorithm to work across multiple machines, where each machine computes partial gradients that are combined to update the global model.

### Types

**1. Synchronous Distributed SGD:**
- All workers compute gradients on their local data.
- A barrier synchronization ensures all workers finish before aggregation.
- Aggregated gradient updates the global model.
- All workers get the updated model before next iteration.

```
Worker 1: Compute gradient g1 ──┐
Worker 2: Compute gradient g2 ──┼──→ Aggregate: g = (g1+g2+g3)/3 → Update model
Worker 3: Compute gradient g3 ──┘
                                          ↓
                              Broadcast updated model to all
```

**Pros:** Consistent convergence, equivalent to sequential SGD.  
**Cons:** Slowest worker bottleneck (straggler problem).

**2. Asynchronous Distributed SGD:**
- Workers compute and push gradients independently.
- No waiting for other workers.
- Parameter server updates model immediately upon receiving any gradient.

**Pros:** No straggler problem, higher throughput.  
**Cons:** Stale gradients may cause convergence issues.

### Parameter Server Architecture

```
                ┌─────────────────┐
                │ Parameter Server │
                │  (Global Model)  │
                └───┬───┬───┬─────┘
            Pull/Push │   │   │ Pull/Push
                ┌─────┘   │   └─────┐
                ▼         ▼         ▼
           [Worker 1] [Worker 2] [Worker 3]
           (Data 1)   (Data 2)   (Data 3)
```

---

## 4. Federated Learning

### Definition

**Federated Learning (FL)** is a distributed machine learning approach where the model is trained across multiple decentralized devices (clients) holding local data, without exchanging raw data. Only model updates (gradients) are shared with a central server.

### Working (FedAvg Algorithm)

**Step 1:** Central server initializes global model and sends to all clients.

**Step 2:** Each client trains the model locally on its private data for several epochs.

**Step 3:** Clients send model updates (weights/gradients) to the server — NOT raw data.

**Step 4:** Server aggregates updates (weighted average based on data size).

**Step 5:** Server broadcasts updated global model to clients.

**Step 6:** Repeat Steps 2-5 until convergence.

### Diagram

```
         ┌─────────────────────────┐
         │    Central Server        │
         │  (Aggregates Updates)    │
         └──┬──────┬──────┬────────┘
    Send    │      │      │    Send
   Model    │      │      │   Model
            ▼      ▼      ▼
        ┌──────┐┌──────┐┌──────┐
        │Phone ││Phone ││Phone │  ← Local Data stays on device
        │  A   ││  B   ││  C   │
        └──┬───┘└──┬───┘└──┬───┘
           │       │       │
           └───────┼───────┘
                   ▼
         Send Model Updates ONLY
         (Not raw data)
```

### Key Characteristics

| Feature | Description |
|---------|-------------|
| **Privacy** | Raw data never leaves the device |
| **Communication** | Only model parameters exchanged (less bandwidth) |
| **Heterogeneity** | Handles non-IID (non-identically distributed) data |
| **Decentralized** | No central data storage needed |

### Advantages
1. **Data Privacy:** Sensitive data remains on local devices.
2. **Reduced Communication:** Only model updates sent, not large datasets.
3. **Regulatory Compliance:** Meets GDPR and data sovereignty requirements.
4. **Scalability:** Works with millions of edge devices.

### Challenges
- Non-IID data across clients
- Communication bottleneck with many clients
- Client availability (devices go offline)
- Model poisoning attacks

### Applications
- Google Keyboard (next-word prediction)
- Healthcare (training on hospital data without sharing patient records)
- Autonomous vehicles (sharing driving knowledge)

---

## 5. All-Reduce

### Definition

**All-Reduce** is a collective communication pattern where all workers contribute their local values (gradients) and all workers receive the final aggregated result. It is a key primitive for synchronous distributed training.

### Working

1. Each worker computes local gradients.
2. All-Reduce operation aggregates (sums/averages) gradients across all workers.
3. Every worker receives the identical aggregated result.
4. Each worker independently updates its local model copy.

### Implementation: Ring All-Reduce

The most efficient implementation uses a **ring topology**:

**Phase 1: Scatter-Reduce**
- Gradients divided into N chunks (N = number of workers).
- Each worker sends one chunk to its neighbor in the ring.
- After N-1 steps, each worker has the fully reduced version of one chunk.

**Phase 2: All-Gather**
- Each worker sends its complete chunk around the ring.
- After N-1 steps, every worker has all chunks (complete aggregated gradient).

```
Ring Topology (4 workers):
    W1 → W2 → W3 → W4 → W1 (circular)

Communication Cost: 2(N-1)/N × data_size
    (Nearly constant per worker regardless of N)
```

### Advantages
- **No parameter server needed** — peer-to-peer communication.
- **Bandwidth optimal** — each worker sends/receives fixed amount.
- **Scalable** — communication cost nearly independent of worker count.
- **Consistent** — all workers have identical model after each step.

### All-Reduce vs Parameter Server

| Aspect | All-Reduce | Parameter Server |
|--------|-----------|-----------------|
| Architecture | Decentralized (peer-to-peer) | Centralized server |
| Bottleneck | Distributed evenly | Server can be bottleneck |
| Bandwidth | Optimal utilization | Server bandwidth limited |
| Fault Tolerance | Less tolerant | Server can handle failures |
| Used By | Horovod, PyTorch DDP | TensorFlow (older versions) |

---

## 6. Hogwild!

### Definition

**Hogwild!** is an asynchronous, lock-free approach to stochastic gradient descent where multiple workers/threads update shared model parameters concurrently without any synchronization or locking mechanism.

### Working

1. All workers share a single parameter vector in shared memory.
2. Each worker independently:
   - Reads current parameters (possibly stale).
   - Computes gradient on a random data sample.
   - Updates parameters directly (overwrite) — NO locks.
3. Workers do NOT coordinate or wait for each other.

### Diagram

```
Shared Memory: [θ₁, θ₂, θ₃, ..., θₙ]  (Model Parameters)
                  ↑↓      ↑↓      ↑↓
              ┌───────┐┌───────┐┌───────┐
              │Worker1││Worker2││Worker3│
              │Read θ ││Read θ ││Read θ │
              │Compute││Compute││Compute│
              │Write θ││Write θ││Write θ│
              └───────┘└───────┘└───────┘
              (No locks, no synchronization)
```

### Why It Works (Convergence Guarantee)

Hogwild! converges under the **sparsity condition**:
- If gradient updates are **sparse** (each update touches only a small subset of parameters).
- Probability of two workers updating the same parameter simultaneously is low.
- Conflicts are rare, so overwrites cause minimal damage.
- Mathematically proven to converge at the same rate as sequential SGD for sparse problems.

### Advantages
1. **Extremely fast** — no synchronization overhead.
2. **High throughput** — all workers compute continuously.
3. **Simple implementation** — no locks or barriers.
4. **Near-linear speedup** — for sparse optimization problems.

### Disadvantages
1. Works well only for **sparse** problems (e.g., sparse feature models).
2. For dense problems, too many conflicts degrade convergence.
3. Limited to shared-memory systems (single machine, multiple cores).
4. Stale reads can slow convergence.

### Applications
- Matrix factorization
- Sparse SVM
- Graph-based machine learning problems

---

## 7. Elastic Averaging SGD (EASGD)

### Definition

**Elastic Averaging SGD (EASGD)** is a distributed optimization algorithm where multiple workers maintain their own local model parameters, connected to a central parameter via an elastic force (like a spring). Workers are free to explore the parameter space independently while being "pulled" toward a center variable.

### Working

**Architecture:** Multiple workers + one central (master) parameter.

**Update Rules:**

For each worker i:
```
Local update:    θᵢ ← θᵢ - η∇f(θᵢ) - α(θᵢ - θ̃)
Center update:   θ̃  ← θ̃ + β∑(θᵢ - θ̃)
```

Where:
- θᵢ = local parameters of worker i
- θ̃ = center variable (global parameters)
- η = learning rate
- α = elasticity (pull toward center) — worker side
- β = elasticity — center side
- ∇f(θᵢ) = gradient computed on local data

### Intuition (Spring/Elastic Analogy)

```
         Center Variable (θ̃)
        ╱    │    ╲
  elastic  elastic  elastic
  force    force    force
      ╱      │      ╲
   θ₁       θ₂      θ₃
(Worker1) (Worker2) (Worker3)

Each worker explores independently but is
pulled back toward the center (like a spring).
```

### Step-by-Step Process

1. Initialize center variable θ̃ and broadcast to all workers.
2. Each worker trains locally using SGD on its data partition.
3. Periodically (every τ steps), workers communicate with center:
   - Worker sends its parameters to center.
   - Center computes average displacement and updates.
   - Worker receives elastic pull toward center.
4. Workers continue local exploration.
5. Repeat until convergence.

### Key Hyperparameters

| Parameter | Role | Effect |
|-----------|------|--------|
| α (alpha) | Elastic force on worker | Higher = workers stay closer to center |
| β (beta) | Center update rate | Controls how fast center moves |
| τ (tau) | Communication period | Higher = less communication, more exploration |

### Advantages

1. **Exploration:** Workers explore different regions of parameter space → avoids local minima.
2. **Communication efficiency:** Workers communicate only periodically (every τ steps), not every iteration.
3. **Fault tolerance:** If one worker fails, others and center continue.
4. **Stability:** Elastic force prevents workers from diverging too far.
5. **Better generalization:** Exploration of parameter space leads to better solutions.

### Comparison with Other Methods

| Aspect | Synchronous SGD | Async SGD | EASGD |
|--------|----------------|-----------|-------|
| Synchronization | Full barrier | None | Periodic (every τ steps) |
| Staleness | None | High | Controlled |
| Exploration | Low | Medium | High |
| Communication | Every step | Every step | Every τ steps |
| Convergence | Stable | Unstable | Stable |

### Applications
- Deep neural network training
- Large-scale image classification
- Natural language processing models

---

## 8. Software to Implement Distributed ML

### Apache Spark (MLlib)

| Feature | Details |
|---------|---------|
| **Type** | General-purpose distributed computing framework |
| **ML Library** | MLlib (built-in) |
| **Core Abstraction** | RDD (Resilient Distributed Dataset) |
| **Processing** | In-memory computing, iterative algorithms |
| **Languages** | Python, Scala, Java, R |
| **Best For** | Large-scale data processing + ML pipelines |

### GraphLab (Turi Create)

| Feature | Details |
|---------|---------|
| **Type** | Graph-parallel computation framework |
| **Focus** | Machine learning on graph-structured data |
| **Abstraction** | Vertex programs on graph |
| **Processing** | Asynchronous, shared-memory, graph-based |
| **Best For** | PageRank, Collaborative Filtering, Graph Neural Networks |
| **Key Feature** | Automatic parallelization of graph algorithms |

### Google TensorFlow

| Feature | Details |
|---------|---------|
| **Type** | Deep learning framework with distributed support |
| **Distribution Strategy** | MirroredStrategy, ParameterServer, MultiWorker |
| **Abstraction** | Computation graph (dataflow graph) |
| **Hardware** | CPU, GPU, TPU support |
| **Best For** | Deep learning, neural network training at scale |
| **Key Feature** | tf.distribute API for multi-device/multi-machine training |

### Parallel ML System (Petuum / CMU)

| Feature | Details |
|---------|---------|
| **Type** | Distributed ML platform |
| **Focus** | Bounded staleness model (SSP - Stale Synchronous Parallel) |
| **Key Innovation** | Allows bounded inconsistency for faster convergence |
| **Processing** | Parameter server with staleness guarantees |
| **Best For** | Topic models, Deep learning, Matrix factorization |
| **Key Feature** | Balances consistency and speed with SSP model |

### Comparison Table

| Framework | Paradigm | Best Use Case | Scalability |
|-----------|----------|---------------|-------------|
| Spark | Data-parallel MapReduce | ETL + ML Pipelines | Very High |
| GraphLab | Graph-parallel | Graph ML algorithms | High |
| TensorFlow | Dataflow graph | Deep Learning | Very High |
| Petuum | Parameter Server + SSP | Large-scale ML models | High |

---

## 9. Systems and Architectures for Distributed Machine Learning

### Architecture Types

#### 1. Parameter Server Architecture

**Description:** Central server(s) store global model parameters. Workers compute gradients locally and push/pull parameters from the server.

```
┌────────────────────────────────┐
│     Parameter Server(s)         │
│   [Global Model Parameters]     │
└──┬────────┬────────┬───────────┘
   │ Push/  │ Push/  │ Push/
   │ Pull   │ Pull   │ Pull
   ▼        ▼        ▼
[Worker1] [Worker2] [Worker3]
(Data 1)  (Data 2)  (Data 3)
```

**Characteristics:**
- Centralized parameter storage
- Supports synchronous and asynchronous updates
- Server can shard parameters across multiple machines
- Used by: Original TensorFlow, Petuum

#### 2. All-Reduce (Decentralized) Architecture

**Description:** No central server. Workers communicate directly using collective operations (Ring All-Reduce).

```
     W1 ←→ W2
     ↕       ↕
     W4 ←→ W3
   (Ring or tree topology)
```

**Characteristics:**
- Peer-to-peer communication
- Bandwidth optimal
- Used by: Horovod, PyTorch DDP

#### 3. MapReduce-based Architecture

**Description:** ML algorithms expressed as Map and Reduce operations.

```
Input Data → Map (compute partial results) → Shuffle → Reduce (aggregate) → Output
```

**Characteristics:**
- Simple programming model
- Disk-based (slow for iterative ML)
- Used by: Apache Mahout on Hadoop

#### 4. Dataflow Graph Architecture

**Description:** Computation expressed as a directed graph where nodes are operations and edges are data (tensors) flowing between them.

```
Input → [MatMul] → [Add Bias] → [ReLU] → [Softmax] → Output
           ↑            ↑
        [Weights]    [Bias]
```

**Characteristics:**
- Automatic differentiation
- Device placement optimization
- Used by: TensorFlow, PyTorch (dynamic graph)

#### 5. Graph-Parallel Architecture

**Description:** Designed for algorithms operating on graph data. Each vertex program operates on local neighborhood.

**Characteristics:**
- GAS model (Gather, Apply, Scatter)
- Used by: GraphLab, Pregel (Google)

### Key Design Decisions in DML Systems

| Decision | Options | Tradeoff |
|----------|---------|----------|
| **Synchronization** | Sync / Async / Bounded Staleness (SSP) | Consistency vs. Speed |
| **Communication** | Parameter Server / All-Reduce / Gossip | Centralization vs. Bandwidth |
| **Parallelism** | Data / Model / Pipeline | Scalability vs. Complexity |
| **Fault Tolerance** | Checkpointing / Replication | Overhead vs. Reliability |
| **Computation** | BSP / ASP / SSP | Correctness vs. Performance |

### Synchronization Models

| Model | Description | Example |
|-------|-------------|---------|
| **BSP (Bulk Synchronous Parallel)** | All workers sync at barriers | Synchronous SGD |
| **ASP (Asynchronous Parallel)** | No synchronization | Hogwild!, Async SGD |
| **SSP (Stale Synchronous Parallel)** | Workers can be at most s steps ahead | Petuum |

---

## 10. Integration of AI Algorithms in Distributed Systems

### A. Intelligent Resource Management

**Definition:** Using AI/ML algorithms to dynamically allocate and manage computing resources (CPU, memory, network, storage) in distributed systems.

**Techniques:**

| Technique | How It Works |
|-----------|-------------|
| **Predictive Autoscaling** | ML models predict future resource demand and pre-provision resources |
| **Reinforcement Learning** | RL agent learns optimal resource allocation policy through trial-and-error |
| **Neural Network Scheduling** | DNN predicts best placement of tasks on heterogeneous resources |
| **Anomaly-based Rebalancing** | Detect resource anomalies and trigger reallocation |

**Working Example:**
```
Historical Metrics → ML Model → Predicted Demand → Auto-scale Decision
(CPU, Memory, I/O)   (LSTM/Prophet)  (Next 10 min)    (Add/Remove VMs)
```

**Benefits:**
1. Reduces over-provisioning (cost savings)
2. Prevents under-provisioning (performance maintenance)
3. Handles dynamic workloads automatically
4. Optimizes multi-dimensional resources simultaneously

### B. Anomaly Detection and Fault Tolerance

**Definition:** Using AI to detect unusual patterns in system behavior (anomalies) that indicate faults, and automatically triggering recovery mechanisms.

**AI Techniques Used:**

| Method | Application |
|--------|-------------|
| **Autoencoders** | Learn normal behavior; high reconstruction error = anomaly |
| **Isolation Forest** | Identify outlier data points in system metrics |
| **LSTM Networks** | Detect anomalies in time-series metrics (CPU, latency) |
| **Clustering (DBSCAN)** | Group normal behaviors; outlier clusters = faults |

**Working:**
```
System Metrics → Feature Extraction → AI Model → Normal/Anomaly?
                                                       │
                                          ┌────────────┴────────────┐
                                          ▼                         ▼
                                       Normal                    Anomaly
                                    (Continue)              (Trigger Recovery)
                                                           - Restart service
                                                           - Migrate workload
                                                           - Alert admin
```

**Benefits:**
1. **Proactive:** Detect failures before they happen
2. **Reduced downtime:** Faster detection than threshold-based monitoring
3. **Adaptive:** Learns evolving system behavior patterns
4. **Fewer false positives:** Compared to static thresholds

### C. Predictive Analytics

**Definition:** Using historical data and AI models to predict future system events, performance bottlenecks, and failures.

**Applications in Distributed Systems:**

| Application | AI Technique | Benefit |
|-------------|-------------|---------|
| **Failure Prediction** | Random Forest, LSTM | Replace components before failure |
| **Workload Forecasting** | Time-series models (ARIMA, Prophet) | Pre-scale resources |
| **SLA Violation Prediction** | Classification models | Proactive mitigation |
| **Capacity Planning** | Regression models | Long-term infrastructure decisions |

**Benefits:**
- Minimize unplanned downtime
- Optimize resource utilization
- Improve user experience through proactive scaling
- Data-driven decision making

### D. Intelligent Task Offloading

**Definition:** Using AI to decide which tasks should be offloaded from resource-constrained devices (edge/mobile) to more powerful machines (cloud/fog) based on factors like latency, energy, and computational requirements.

**Decision Factors:**

| Factor | Consideration |
|--------|--------------|
| Task complexity | CPU/memory requirements |
| Network conditions | Latency, bandwidth availability |
| Energy constraints | Battery level of edge device |
| Deadline | Time-critical vs. delay-tolerant |
| Data size | Transfer cost |

**AI-based Approach:**
```
Task Arrives → Feature Extraction → AI Decision Model → Execute Locally
               (complexity, deadline,     │                    OR
                network state, battery)    │              Offload to Cloud/Fog
                                          ▼
                              ┌──────────────────────┐
                              │ DNN/RL decides:       │
                              │ • Where to execute    │
                              │ • When to offload     │
                              │ • How to partition    │
                              └──────────────────────┘
```

**Benefits:**
1. Reduces latency for computation-heavy tasks
2. Saves energy on edge devices
3. Adapts to dynamic network conditions
4. Balances between local and remote execution optimally

---

---

## SECTION B: PYQ Answers (Written Separately as Asked in Exam)

---

## PYQ: Q3a) Explain Systems and Architectures for Distributed Machine Learning. [9 marks]

### Answer:

**Introduction:**
Distributed Machine Learning (DML) systems enable training of ML models across multiple machines to handle large datasets and complex models. The choice of system architecture significantly impacts performance, scalability, and fault tolerance.

### 1. Parameter Server Architecture

The most widely used architecture for distributed ML.

**Components:**
- **Server nodes:** Store and manage global model parameters (can be sharded across multiple servers).
- **Worker nodes:** Perform computation (gradient calculations) on local data partitions.

**Working:**
1. Workers pull latest parameters from server.
2. Workers compute gradients on local data batch.
3. Workers push gradients to server.
4. Server aggregates gradients and updates parameters.

```
┌──────────────────────────────────┐
│    Parameter Server Group         │
│  [Server 1] [Server 2] [Server 3]│ ← Parameters sharded
└──────┬──────────┬──────────┬─────┘
       │          │          │
  Push/Pull  Push/Pull  Push/Pull
       │          │          │
  [Worker1]  [Worker2]  [Worker3]   ← Compute gradients
  (Data 1)   (Data 2)   (Data 3)
```

**Features:**
- Supports sync and async updates
- Handles node failures through replication
- Example: DistBelief (Google), Petuum

### 2. All-Reduce (Decentralized) Architecture

**Working:**
- No central server; workers communicate peer-to-peer.
- Ring All-Reduce: Each worker sends/receives from neighbors.
- After all-reduce, every worker has the aggregated gradient.
- Each worker updates its model independently.

**Features:**
- Bandwidth optimal — O(2(N-1)/N × data_size)
- No single point of failure
- Example: Horovod (Uber), PyTorch DistributedDataParallel

### 3. Dataflow Graph Architecture

**Working:**
- Computation is a directed acyclic graph (DAG).
- Nodes = operations (matmul, activation); Edges = tensors (data flow).
- System automatically distributes graph across devices.

**Features:**
- Automatic differentiation
- Device placement optimization
- Supports heterogeneous hardware (CPU+GPU+TPU)
- Example: TensorFlow, PyTorch

### 4. MapReduce-based Architecture

**Working:**
- ML expressed as iterative Map + Reduce operations.
- Map: Compute partial statistics/gradients.
- Reduce: Aggregate partial results.

**Features:**
- Simple but slow for iterative algorithms (disk I/O).
- Example: Apache Mahout on Hadoop

### 5. Graph-Parallel Architecture

**Working:**
- For ML on graph data (social networks, knowledge graphs).
- GAS model: Gather → Apply → Scatter on graph vertices.

**Features:**
- Optimized for graph structure
- Example: GraphLab, Google Pregel

### Synchronization Strategies in DML Systems

| Strategy | Description | Trade-off |
|----------|-------------|-----------|
| **BSP** | All sync at barrier | Consistent but slow (stragglers) |
| **ASP** | No sync | Fast but may not converge |
| **SSP** | Bounded staleness (max s steps apart) | Balance of speed and correctness |

### Summary Table

| Architecture | Communication | Best For | Example System |
|-------------|---------------|----------|----------------|
| Parameter Server | Centralized | General DML | TensorFlow PS, Petuum |
| All-Reduce | Decentralized | Data-parallel DNN | Horovod, PyTorch DDP |
| Dataflow Graph | Varies | Deep learning | TensorFlow, PyTorch |
| MapReduce | Centralized | Simple batch ML | Mahout |
| Graph-Parallel | Decentralized | Graph ML | GraphLab, Pregel |

---

## PYQ: Q3b) Write notes on: i) Federated Learning, ii) Hogwild, iii) Elastic Averaging SGD [8 marks]

### i) Federated Learning

**Definition:** A distributed ML approach where the model is trained across decentralized devices holding local data, without exchanging raw data — only model updates are shared.

**Working (FedAvg):**
1. Server sends global model to selected clients.
2. Each client trains locally on its private data.
3. Clients send only model updates (not data) to server.
4. Server averages updates to form new global model.
5. Repeat until convergence.

**Key Properties:**
- Data never leaves the device → Privacy preserved
- Communication efficient (only parameters shared)
- Handles non-IID data distributions

**Applications:** Google Keyboard prediction, healthcare AI, autonomous driving.

**Advantages:** Privacy preservation, regulatory compliance (GDPR), reduced communication cost, scalability to millions of devices.

---

### ii) Hogwild!

**Definition:** A lock-free, asynchronous parallel SGD algorithm where multiple threads update shared parameters concurrently without any synchronization.

**Working:**
1. Multiple threads share a single parameter vector in memory.
2. Each thread independently: reads parameters → computes gradient → writes update.
3. No locks, no barriers, no coordination.

**Why It Works:**
- Converges for sparse optimization problems.
- When updates are sparse, probability of conflicts is low.
- Provably converges at same rate as sequential SGD.

**Advantages:** Maximum throughput, no synchronization overhead, near-linear speedup.

**Limitations:** Only works for sparse problems; limited to shared-memory (single machine); dense updates cause conflicts.

---

### iii) Elastic Averaging SGD (EASGD)

**Definition:** A distributed optimization algorithm where workers maintain local parameters connected to a center variable via an elastic force, allowing independent exploration while maintaining global consistency.

**Update Rules:**
- Worker update: θᵢ ← θᵢ - η∇f(θᵢ) - α(θᵢ - θ̃)
- Center update: θ̃ ← θ̃ + β∑(θᵢ - θ̃)

**Working:**
1. Workers train locally using SGD.
2. Periodically communicate with center variable.
3. Elastic force pulls workers toward center (prevents divergence).
4. Workers can explore different regions of loss surface.

**Advantages:**
- Better exploration → avoids local minima
- Communication efficient (periodic, not every step)
- Stable convergence with elastic coupling
- Fault tolerant (workers are independent)

---

## PYQ: Q3a) Explain Elastic Averaging SGD. [9 marks]

### Answer:

**Definition:**
Elastic Averaging SGD (EASGD) is a distributed stochastic gradient descent algorithm that allows multiple workers to independently explore the loss function landscape while being elastically coupled to a central parameter (center variable). This elastic coupling acts like a spring, allowing exploration but preventing divergence.

### Architecture

```
              ┌─────────────────┐
              │ Center Variable  │
              │      (θ̃)        │
              └──┬──────┬──────┬┘
         elastic │      │      │ elastic
         force   │      │      │ force
              ┌──┘      │      └──┐
              ▼         ▼         ▼
          [Worker1] [Worker2] [Worker3]
            (θ₁)      (θ₂)      (θ₃)
          [Data 1]  [Data 2]  [Data 3]
```

### Mathematical Formulation

**Worker i update (every iteration):**
```
θᵢ(t+1) = θᵢ(t) - η · ∇f(θᵢ(t)) - α · (θᵢ(t) - θ̃(t))
```
- First term: Standard SGD gradient step
- Second term: Elastic pull toward center

**Center variable update (every τ iterations):**
```
θ̃(t+1) = θ̃(t) + β · Σᵢ(θᵢ(t) - θ̃(t))
```
- Center moves toward the average of all workers

### Algorithm Steps

1. **Initialization:** Set center variable θ̃ = θ₀; Copy to all workers θᵢ = θ₀.

2. **Local Training (Each Worker Independently):**
   - Sample mini-batch from local data
   - Compute gradient ∇f(θᵢ)
   - Update local parameters with SGD + elastic term

3. **Communication (Every τ steps):**
   - Workers send θᵢ to the master
   - Master updates center: θ̃ ← θ̃ + β·Σ(θᵢ - θ̃)
   - Workers receive updated θ̃

4. **Elastic Force Effect:**
   - If worker drifts far from center → strong pull back
   - If worker is close to center → weak pull (free to explore)

5. **Convergence:** Process repeats until loss converges.

### Hyperparameters

| Parameter | Symbol | Role | Effect of Increasing |
|-----------|--------|------|---------------------|
| Learning rate | η | Local SGD step size | Faster but less stable |
| Worker elasticity | α | Pull strength on worker | Workers stay closer to center |
| Center elasticity | β | Center update speed | Center responds faster to workers |
| Communication period | τ | Steps between sync | Less communication, more exploration |

### Advantages

1. **Exploration of Parameter Space:** Each worker explores independently → discovers diverse solutions → avoids local minima.
2. **Communication Efficiency:** Communicate only every τ steps (not every iteration) → reduced network overhead.
3. **Stability:** Elastic force prevents workers from diverging too far → guaranteed convergence.
4. **Fault Tolerance:** Independent workers; failure of one doesn't stop others.
5. **Better Generalization:** Diverse exploration leads to flatter minima → better test accuracy.
6. **Tunable Trade-off:** α and τ control exploration vs. exploitation balance.

### Comparison

| Aspect | Sync SGD | Async SGD | EASGD |
|--------|----------|-----------|-------|
| Communication | Every step | Every step | Every τ steps |
| Synchronization | Barrier | None | Periodic elastic |
| Exploration | Minimal | Some | Maximum |
| Stale Gradients | None | Major issue | Controlled |
| Convergence | Stable, slow | Fast, unstable | Stable, fast |

### Applications
- Training deep neural networks on distributed clusters
- Large-scale image classification (ImageNet)
- NLP model training across multiple GPUs

---

## PYQ: Q4a) What is Apache Spark? Explain working of Apache Spark. [9 marks]

### Answer:

### What is Apache Spark?

**Apache Spark** is an open-source, distributed computing framework designed for fast, large-scale data processing and analytics. It provides an in-memory computing engine that is up to 100x faster than Hadoop MapReduce for iterative algorithms and interactive queries.

**Key Features:**
- In-memory computation (caches intermediate results in RAM)
- Supports batch processing, streaming, ML, and graph processing
- Fault-tolerant through RDD lineage
- Supports multiple languages: Python, Scala, Java, R

### Spark Architecture

```
┌─────────────────────────────────────────────┐
│              Driver Program                   │
│         ┌─────────────────┐                  │
│         │  SparkContext    │                  │
│         └────────┬────────┘                  │
└──────────────────┼───────────────────────────┘
                   │
         ┌─────────┴─────────┐
         ▼                   ▼
┌─────────────────┐  ┌─────────────────┐
│ Cluster Manager  │  │  Cluster Manager │
│ (YARN/Mesos/    │  │  (Standalone)    │
│  Kubernetes)    │  │                  │
└────────┬────────┘  └────────┬────────┘
         │                    │
    ┌────┴────┐          ┌───┴────┐
    ▼         ▼          ▼        ▼
[Executor] [Executor] [Executor] [Executor]
 [Task]     [Task]     [Task]     [Task]
 [Task]     [Task]     [Task]     [Task]
 [Cache]    [Cache]    [Cache]    [Cache]
```

### Core Components

| Component | Role |
|-----------|------|
| **Driver Program** | Main application; creates SparkContext; defines transformations and actions |
| **SparkContext** | Entry point; connects to cluster manager; coordinates job execution |
| **Cluster Manager** | Allocates resources across applications (YARN, Mesos, K8s, Standalone) |
| **Executors** | Worker processes that run tasks and store cached data |
| **Tasks** | Units of work sent to executors |

### Working of Apache Spark

**Step 1: Application Submission**
- User submits Spark application (driver program).
- SparkContext connects to Cluster Manager.

**Step 2: Resource Allocation**
- Cluster Manager allocates executors on worker nodes.
- Each executor gets CPU cores and memory.

**Step 3: RDD Creation (Core Abstraction)**
- **RDD (Resilient Distributed Dataset):** Immutable, distributed collection of objects partitioned across nodes.
- Created from: HDFS files, local files, other RDDs via transformations.

**Step 4: DAG Construction**
- User defines **Transformations** (lazy): map, filter, join, groupBy.
- User calls **Actions** (trigger execution): count, collect, save.
- Spark builds a DAG (Directed Acyclic Graph) of stages.

**Step 5: Job Execution**
- DAG Scheduler divides job into stages (based on shuffle boundaries).
- Task Scheduler assigns tasks to executors.
- Executors process data partitions in parallel.

**Step 6: In-Memory Processing**
- Intermediate results cached in memory (not written to disk).
- Subsequent operations reuse cached data → massive speedup for iterative ML algorithms.

**Step 7: Fault Recovery**
- If a partition is lost, Spark recomputes it using RDD lineage (DAG of transformations).
- No need for data replication.

### Spark Ecosystem (Libraries)

| Library | Purpose |
|---------|---------|
| **Spark SQL** | Structured data processing with SQL queries |
| **Spark Streaming** | Real-time stream processing (micro-batches) |
| **MLlib** | Machine learning library (classification, clustering, regression) |
| **GraphX** | Graph processing and analytics |

### Why Spark for Distributed ML?

1. **In-memory:** Iterative ML algorithms (gradient descent) are 10-100x faster than MapReduce.
2. **MLlib:** Built-in distributed ML algorithms.
3. **Pipeline API:** End-to-end ML pipelines (load → transform → train → evaluate).
4. **Scalability:** Processes petabytes of data across thousands of nodes.
5. **Fault tolerance:** Automatic recovery through lineage.

### Spark vs Hadoop MapReduce

| Aspect | Hadoop MapReduce | Apache Spark |
|--------|-----------------|--------------|
| Speed | Slow (disk-based) | Fast (in-memory) |
| Iterative ML | Very slow (disk I/O each iteration) | Fast (cached in memory) |
| Ease of Use | Complex (Java) | Simple (Python/Scala API) |
| Real-time | Not supported | Spark Streaming |
| Libraries | Separate tools needed | Unified ecosystem |

---

## PYQ: Q4b) Explain how integration of AI algorithms in distributed systems can help in Intelligent Resource Management, Anomaly Detection. [8 marks]

### Answer:

### Introduction

The integration of AI algorithms into distributed systems enables intelligent, self-managing infrastructure that can predict, detect, and respond to changes automatically without human intervention.

---

### A. Intelligent Resource Management using AI

**Definition:** Using machine learning and AI techniques to dynamically allocate, scale, and optimize computing resources (CPU, memory, storage, network) in distributed systems.

**AI Techniques Applied:**

**1. Predictive Autoscaling (Time-Series ML):**
- Train LSTM/Prophet models on historical metrics (CPU, traffic, memory).
- Predict demand 5-15 minutes ahead.
- Pre-provision resources before load spike arrives.

**2. Reinforcement Learning for Allocation:**
- RL agent observes: current utilization, pending requests, SLA requirements.
- Actions: allocate/deallocate VMs, migrate workloads, adjust priorities.
- Reward: minimize cost + SLA violations.
- Agent learns optimal policy through experience.

**3. Neural Network Scheduling:**
- DNN trained on job characteristics (size, deadline, dependencies).
- Predicts optimal placement on heterogeneous hardware (CPU vs GPU).
- Outperforms heuristic schedulers for complex workloads.

**Working Diagram:**
```
Monitoring Data → Feature Engineering → AI Model → Decision → Execution
(CPU, Memory,     (Aggregation,          (LSTM,     (Scale up,  (API calls to
 Network, I/O)    Normalization)         RL, DNN)   Migrate,    cloud/scheduler)
                                                    Rebalance)
         ↑                                                        │
         └────────── Feedback Loop (Actual Outcomes) ─────────────┘
```

**Benefits:**
1. **Cost Reduction:** 30-50% savings by eliminating over-provisioning.
2. **Proactive Scaling:** Resources ready before demand arrives.
3. **Multi-objective:** Simultaneously optimizes cost, performance, and energy.
4. **Adaptive:** Learns changing workload patterns automatically.
5. **Handles Heterogeneity:** Learns which workloads suit which resource types.

---

### B. Anomaly Detection and Fault Tolerance using AI

**Definition:** Using AI/ML algorithms to identify abnormal system behavior patterns that indicate potential failures, security breaches, or performance degradation, and triggering automated recovery.

**AI Techniques Applied:**

**1. Autoencoders (Unsupervised):**
- Train on normal system behavior (metrics: CPU, latency, error rates).
- Input → Encode → Decode → Compare with input.
- High reconstruction error = anomaly detected.

**2. Isolation Forest:**
- Randomly partitions feature space.
- Anomalies are isolated quickly (fewer partitions needed).
- Used for: detecting unusual traffic patterns, resource usage spikes.

**3. LSTM Networks (Sequential):**
- Model temporal patterns in system metrics.
- Predict expected next values.
- Deviation from prediction = anomaly.

**4. Clustering-based (DBSCAN):**
- Group normal behaviors into clusters.
- Points not belonging to any cluster = anomalies.

**Working Diagram:**
```
                   ┌──────────────────┐
System Metrics ──→ │  AI Anomaly      │ ──→ Normal? → Continue monitoring
(CPU, Latency,     │  Detection Model │
 Errors, Traffic)  │  (Autoencoder/   │ ──→ Anomaly? → Trigger Response:
                   │   LSTM/IF)       │         • Auto-restart service
                   └──────────────────┘         • Migrate to healthy node
                                                • Scale resources
                                                • Alert operations team
```

**Comparison with Traditional Methods:**

| Aspect | Traditional (Threshold) | AI-based Detection |
|--------|------------------------|-------------------|
| Detection | Fixed rules (CPU > 90%) | Learned patterns |
| Adaptability | Manual updates needed | Self-adapting |
| Complex anomalies | Cannot detect | Detects subtle correlations |
| False positives | High | Low (learns normal variation) |
| Novel threats | Cannot detect | Identifies unknown patterns |

**Benefits:**
1. **Early Detection:** Identifies degradation before failure occurs.
2. **Reduced Downtime:** Faster response (seconds vs. minutes).
3. **Fewer False Alarms:** ML learns normal variability.
4. **Discovers Unknown Issues:** Detects patterns humans might miss.
5. **Self-healing:** Automated recovery without human intervention.
6. **Continuous Learning:** Model improves as it sees more data.

---

## PYQ: Q4b) Explain i) Federated Learning ii) Elastic Averaging SGD. [8 marks]

### Answer:

### i) Federated Learning [4 marks]

**Definition:** Federated Learning is a distributed ML technique where a model is trained collaboratively across multiple decentralized devices, with data remaining on each device — only model updates are communicated.

**Working (FedAvg Algorithm):**
1. Server initializes and distributes global model to clients.
2. Each client trains locally on its private data (multiple SGD steps).
3. Clients send model weight updates to server (NOT raw data).
4. Server aggregates: θ_global = Σ(nₖ/n) · θₖ (weighted average by data size).
5. Updated model broadcast to clients. Repeat until convergence.

```
Server: Global Model θ
        ↓ Send θ           ↑ Receive Δθ₁, Δθ₂, Δθ₃
   [Client 1]  [Client 2]  [Client 3]
   (Local Data) (Local Data) (Local Data)
   Train locally  Train locally  Train locally
```

**Key Advantages:**
- Privacy: Raw data never leaves the device.
- Communication efficient: Only model parameters transmitted.
- Scalable: Supports millions of edge devices.
- Regulatory compliant: Meets data protection laws.

**Applications:** Mobile keyboard prediction, healthcare ML, autonomous vehicles.

---

### ii) Elastic Averaging SGD (EASGD) [4 marks]

**Definition:** EASGD is a distributed optimization algorithm where workers maintain independent local parameters connected to a central variable via an elastic force, enabling exploration while maintaining convergence.

**Mathematical Formulation:**
- Worker: θᵢ ← θᵢ - η∇f(θᵢ) - α(θᵢ - θ̃)
- Center: θ̃ ← θ̃ + β·Σ(θᵢ - θ̃)

**Working:**
1. Workers train independently using local SGD.
2. Every τ steps, communicate with center variable.
3. Elastic force α(θᵢ - θ̃) pulls workers toward center.
4. Center variable moves toward worker average.

```
Center (θ̃) ←─── elastic force ───→ Worker (θᵢ)
```

**Key Advantages:**
- **Exploration:** Workers discover diverse regions → avoids local minima.
- **Communication efficient:** Sync only every τ steps.
- **Stable convergence:** Elastic coupling prevents divergence.
- **Fault tolerant:** Workers operate independently.

**Comparison:** Unlike synchronous SGD (sync every step) or async SGD (no control), EASGD provides controlled periodic coupling — balancing speed and stability.

---

## Exam Writing Tips for Unit IV

1. **For 9-mark questions:** Write 1.5–2 pages. Include definition + working steps + diagram + comparison table + advantages.
2. **For 8-mark short notes:** Write ~1 page per sub-topic. Definition + working + key points + one advantage/application.
3. **Diagrams are essential:** Architecture diagrams for Parameter Server, All-Reduce, Spark, EASGD, and Federated Learning.
4. **Use tables** for comparisons — they show structured understanding.
5. **Mention real examples:** TensorFlow, Horovod, Google Keyboard, Petuum — adds credibility to answers.
