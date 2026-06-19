# Distributed Computing: Unit 3-6 Last-Minute Revision

## Unit 3: Distributed Computing Algorithms

### Consensus Algorithms
- Consensus means distributed nodes agree on one value/state even with failures.
- Main properties: agreement, validity, termination, integrity.
- Used in replicated databases, distributed logs, leader election, and fault-tolerant services.

### RAFT
- Roles: leader, follower, candidate.
- Term: logical time used during elections.
- Leader election: follower times out, becomes candidate, requests votes, wins on majority.
- Log replication: leader receives client request, appends log entry, sends AppendEntries to followers, commits after majority ACK.
- Safety: a candidate must have an up-to-date log to become leader.
- Strength: simpler than Paxos because it separates election, replication, and safety.

### Paxos and Fast Paxos
- Paxos roles: proposer, acceptor, learner.
- Classic Paxos phases: prepare/promise, then accept/accepted.
- Fast Paxos reduces latency by letting clients send values directly to acceptors in fast rounds.
- Fast Paxos needs larger quorums and falls back to classic Paxos when conflicts occur.

### Fault Tolerance and Recovery
- Fault tolerance means the system continues operating even when components fail.
- Fault types: crash, Byzantine, network, timing.
- Techniques: replication, redundancy, consensus protocols.
- Recovery: checkpointing, write-ahead logging, rollback recovery, state transfer from replicas.

### Load Balancing
- Centralized load balancing: one controller/load balancer decides where requests go.
- Pros: simple, global view, easy policy control.
- Cons: bottleneck and single point of failure.
- Distributed load balancing: multiple nodes cooperate or make local decisions.
- Pros: scalable and highly available.
- Cons: harder coordination, partial information.

### WRR vs Least Connection
| Point | Weighted Round Robin | Least Connection |
|---|---|---|
| Decision basis | Static server weights | Current active connections |
| Best for | Predictable workloads | Variable request duration |
| Overhead | Low | Medium |
| Adaptability | Low | High |
| Weakness | Ignores live load | May ignore server capacity |

### Genetic Algorithm for Scheduling
- Encodes a task schedule as a chromosome.
- Steps: initialize population, evaluate fitness, select parents, crossover, mutate, repeat.
- Fitness usually aims to minimize makespan, cost, or energy.
- Useful because task scheduling has a huge search space.

### Reinforcement Learning for Load Balancing
- Agent: load balancer.
- State: CPU, memory, queues, response time, network load.
- Action: assign request to a server.
- Reward: low latency, balanced load, high throughput, no overload.
- Advantage: learns dynamically and adapts better than static rules.

### Communication and Coordination
- Communication models: message passing, RPC, publish-subscribe, distributed shared memory.
- Coordination mechanisms: mutual exclusion, leader election, clock synchronization, distributed transactions, barriers.
- Key challenges: no global clock, message delay, network partitions, partial failures.

### Other Consensus Notes
- Viewstamped Replication: leader-based replication using view numbers.
- ZAB: ZooKeeper Atomic Broadcast; recovery plus broadcast modes.
- Mencius: multi-leader Paxos variant.
- EPaxos: leaderless Paxos variant using dependency tracking.

## Unit 4: Distributed Machine Learning and AI

### Distributed Machine Learning
- Distributed ML trains models across multiple nodes to handle large data, large models, or slow training.
- Main forms: data parallelism and model parallelism.

### Data Parallelism vs Model Parallelism
| Point | Data Parallelism | Model Parallelism |
|---|---|---|
| Split | Dataset | Model/layers |
| Each worker has | Full model + data partition | Part of model |
| Communication | Gradients | Activations/outputs |
| Best for | Large datasets | Large models |
| Complexity | Lower | Higher |

### Distributed Gradient Descent
- Each worker computes gradients on local data.
- Gradients are combined to update global model.
- Synchronous SGD: all workers wait; stable but straggler problem.
- Asynchronous SGD: workers update independently; faster but stale gradients may hurt convergence.

### Parameter Server
- Central server stores global parameters.
- Workers pull parameters, compute gradients, push updates.
- Simple but server may become bottleneck.

### All-Reduce
- Every worker contributes gradients and every worker receives the aggregated result.
- Ring All-Reduce has scatter-reduce and all-gather phases.
- No central bottleneck; used in Horovod and PyTorch DDP.

### Federated Learning
- Model trained across devices/organizations without sharing raw data.
- FedAvg steps: server sends model, clients train locally, clients send updates, server averages updates, repeat.
- Benefits: privacy, lower raw-data transfer, regulatory compliance.
- Challenges: non-IID data, offline clients, communication cost, poisoning attacks.

### Hogwild
- Lock-free asynchronous SGD in shared memory.
- Multiple workers update shared parameters without locks.
- Works best when gradients are sparse, so update conflicts are rare.
- Very fast but poor for dense models with many conflicts.

### Elastic Averaging SGD
- Workers keep local parameters connected to a center variable by an elastic force.
- Allows local exploration while preventing workers from drifting too far.
- Communicates periodically, reducing network overhead.
- Good comparison: Sync SGD is stable but slow; Async SGD is fast but stale; EASGD balances exploration and stability.

### Distributed ML Software
- Spark MLlib: distributed ML on Spark, good for pipelines and large-scale data.
- TensorFlow: deep learning with distributed strategies.
- GraphLab/Turi: graph-parallel ML.
- Petuum: ML system using bounded staleness.

### AI in Distributed Systems
- Intelligent resource management: predictive scaling, RL-based allocation, neural schedulers.
- Anomaly detection: autoencoders, isolation forest, LSTM, clustering.
- Benefits: proactive scaling, lower downtime, adaptive optimization, lower manual tuning.

### Apache Spark Working
- Driver creates SparkContext.
- Cluster manager allocates executors.
- Data represented as RDD/DataFrame partitions.
- Transformations are lazy; actions trigger DAG execution.
- Fault tolerance through RDD lineage.
- In-memory caching speeds iterative ML.

## Unit 5: Big Data Processing

### Big Data Frameworks
| Framework | Model | Main Idea | Best For |
|---|---|---|---|
| Hadoop | Batch | HDFS + MapReduce + YARN | Large ETL, archival processing |
| Spark | Batch + micro-batch | In-memory DAG engine | ML, analytics, iterative jobs |
| Storm | Real-time stream | Spouts and bolts | Low-latency alerts |
| Samza | Stream | Kafka-native processing | Stateful Kafka pipelines |
| Flink | Stream + batch | Event-time, checkpoints | Complex event processing |

### Hadoop
- HDFS splits files into blocks and replicates them.
- Map phase processes blocks into key-value pairs.
- Shuffle/sort groups keys.
- Reduce phase aggregates final output.
- Fault tolerance through replication and task re-execution.

### Spark
- Uses RDDs/DataFrames and lazy evaluation.
- Builds DAG, splits into stages, runs parallel tasks on executors.
- Faster than Hadoop for iterative workloads due to memory caching.
- Libraries: Spark SQL, Streaming, MLlib, GraphX.

### Storm, Samza, Flink
- Storm: true tuple-at-a-time streaming with spouts and bolts.
- Samza: Kafka-based stream processing with local state stores.
- Flink: stream-first engine with event time, watermarks, windows, and exactly-once checkpoints.

### Flynn's Taxonomy
| Type | Meaning | Example |
|---|---|---|
| SISD | Single instruction, single data | Single-core sequential machine |
| MISD | Multiple instruction, single data | Fault-tolerant redundant systems |
| SIMD | Single instruction, multiple data | GPU, vector processing |
| MIMD | Multiple instruction, multiple data | Multicore CPUs, clusters |

### SIMD vs MIMD
- SIMD: one instruction applied to many data items in lockstep; best for matrix/image operations.
- MIMD: independent processors run different instructions on different data; best for general distributed systems.

### SPMD and MPP
- SPMD: same program runs on many processors, each with different data partitions.
- MPP: thousands of shared-nothing nodes connected by high-speed network.

### Scalable Data Ingestion
- Batch ingestion: scheduled large chunks.
- Stream ingestion: continuous event-by-event data.
- Micro-batch: small frequent batches.
- Log-based CDC: captures database changes from transaction logs.
- Message queue ingestion: Kafka/RabbitMQ buffer producers and consumers.
- Event-driven ingestion: triggers on webhooks, file upload, or system events.

### Real-Time Analytics
- Real-time analytics gives immediate insight from fresh data.
- Types: operational, on-demand, continuous, interactive, predictive, complex event processing.
- Streaming analytics can be stateless, stateful, windowed, event-time based, or CEP-based.
- Windows: tumbling is non-overlapping; sliding is overlapping; session is activity-gap based.

### AI/Data Science for Big Data
- Data quality detection, pipeline optimization, predictive analytics, real-time anomaly detection, NLP for unstructured data, AI query optimization, recommendations.
- Main value: scale, speed, automation, hidden pattern discovery, continuous improvement.

## Unit 6: Security and Privacy

### Security Challenges
- Confidentiality: prevent unauthorized reading.
- Integrity: prevent/detect tampering.
- Authentication: verify identity.
- Authorization: enforce permissions.
- Insider threats: misuse by authorized users.
- DoS/DDoS: service unavailability through overload.
- Network attacks: MITM, packet injection, session hijacking.
- Policy consistency: same security rules across nodes.
- Replication security: every replica increases attack surface.

### Insider Threats
- Malicious insider: intentional harm or theft.
- Negligent insider: accidental exposure.
- Compromised insider: attacker uses stolen credentials.
- Controls: least privilege, audit logs, UEBA, DLP, access reviews, separation of duties.

### Secure Communication
- TLS/SSL encrypts data in transit.
- TLS handshake: client hello, server certificate, certificate verification, key exchange, encrypted communication.
- PKI uses CA-signed certificates to bind identity to public keys.
- VPN creates encrypted tunnel over public networks.
- AMQP supports secure message queues using authentication, TLS, and access control.

### Privacy Preservation
| Technique | Core Idea | Key Point |
|---|---|---|
| Differential Privacy | Add calibrated noise | Epsilon controls privacy-accuracy tradeoff |
| Homomorphic Encryption | Compute on encrypted data | Strong privacy but expensive |
| SMPC | Parties compute jointly without revealing inputs | Uses secret sharing, garbled circuits, OT |
| Federated Learning | Train locally, share updates only | Can combine with DP and secure aggregation |
| Anonymization | Remove identifiers irreversibly | k-anonymity, l-diversity, t-closeness |
| Pseudonymization | Replace identifiers with tokens | Reversible with mapping key |

### Differential Privacy
- Protects whether any one person's data is included.
- More noise means stronger privacy but lower accuracy.
- Useful for census, telemetry, aggregate analytics.

### Homomorphic Encryption
- Cloud/server performs operations on ciphertext.
- Decrypted result equals plaintext computation result.
- Types: partial, somewhat, fully homomorphic.
- Limitation: high computation and storage cost.

### SMPC
- Multiple parties compute f(x1, x2, ..., xn) without revealing private inputs.
- Secret sharing splits data into shares; no single share reveals the input.
- Garbled circuits encrypt computation steps.
- Oblivious transfer lets a receiver obtain one value without sender knowing which.
- Applications: private auctions, joint fraud analysis, medical research, voting.

### Access Control
- RBAC: permissions based on role.
- ABAC: permissions based on attributes.
- MAC: system-enforced labels.
- DAC: owner decides access.
- Data minimization: collect, retain, access, and process only what is necessary.

### AI-Based Intrusion Detection
- Anomaly detection: learns normal behavior, flags deviations.
- Behavior-based detection/UEBA: profiles users/entities and detects suspicious changes.
- Deep learning classification: labels known attack patterns.
- RL response: learns when to block, throttle, quarantine, or reroute.

### Threat Mitigation
- Auto-block malicious IPs/users.
- Dynamic rate limiting.
- Intelligent quarantine.
- Adaptive firewall rules.
- Honeypot/deception routing.
- Predictive patch prioritization.

### Threat Hunting and Visualization
- Threat hunting is proactive searching for hidden threats that automated tools missed.
- Process: form hypothesis, collect logs, analyze, find IOCs/TTPs, respond, improve rules.
- Visualization types: network graphs, timelines, heat maps, geo maps, dashboards, attack trees, Sankey diagrams.
- Value: faster investigation, lower dwell time, better communication.

## Final Exam Memory Hooks

- Always start with a definition.
- For 6 marks: definition + working + 4-6 points + small diagram/table.
- For 8/9 marks: add comparison, advantages, challenges, and example.
- Use tables for comparison questions.
- Use diagrams for RAFT, Spark, FL, EASGD, ingestion pipeline, TLS, SMPC, and IDS.
- Remember key pairs:
  - RAFT = leader election + log replication + safety.
  - Spark = in-memory + lazy DAG + RDD lineage.
  - Flink = event time + watermarks + checkpointing.
  - FL = local data + shared model updates.
  - DP = noise + epsilon.
  - SMPC = compute together, reveal only output.
