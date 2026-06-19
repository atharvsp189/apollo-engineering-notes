# Unit III: Evolutionary Computing - SPPU Exam Preparation

## 📝 How to Write Answers in SPPU Exams

| Marks           | Strategy                                                                        |
| --------------- | ------------------------------------------------------------------------------- |
| **[5-6 marks]** | Definition (1-2 lines) + Key Points (4-5 bullets) + Small Diagram if applicable |
| **[8 marks]**   | Definition + Detailed Explanation + Diagram + Example                           |
| **[10 marks]**  | Definition + Detailed Explanation + Diagram + Example + Comparison/Advantages   |

**Tips:**

- Always start with a clear definition
- Use diagrams wherever possible (flowcharts, block diagrams)
- Use bullet points for readability
- Write key terms in **bold** or underline them
- For "Compare" questions, always use a table

---

## 1. Introduction to Evolutionary Computing

**Definition:** Evolutionary Computing (EC) is a family of population-based optimization algorithms inspired by biological evolution. They use mechanisms like reproduction, mutation, recombination, and selection to evolve solutions to problems.

**Key Characteristics:**

- Population-based (works with a set of candidate solutions)
- Stochastic (uses randomness)
- Iterative (improves solutions over generations)
- Fitness-driven (better solutions survive)

---

## 2. Terminologies of Evolutionary Computing

| Term                 | Meaning                                               |
| -------------------- | ----------------------------------------------------- |
| **Individual**       | A single candidate solution                           |
| **Population**       | A collection of individuals                           |
| **Chromosome**       | Encoded representation of a solution                  |
| **Gene**             | A single element/position in a chromosome             |
| **Allele**           | The value a gene can take                             |
| **Fitness Function** | A function that evaluates how good a solution is      |
| **Generation**       | One iteration of the evolutionary cycle               |
| **Genotype**         | The encoded form (internal representation)            |
| **Phenotype**        | The decoded/actual solution (external representation) |
| **Search Space**     | The set of all possible solutions                     |

---

## 3. Genetic Operators

### Definition:

Genetic operators are mechanisms used to generate new candidate solutions from existing ones. They drive the search process in evolutionary algorithms.

### Three Main Operators:

#### a) Selection

- Chooses individuals from the population for reproduction
- Fitter individuals have higher probability of being selected
- Types: Roulette Wheel, Tournament, Rank-based, Elitism

#### b) Crossover (Recombination)

- Combines genetic material from two parents to produce offspring
- Explores the search space by mixing existing solutions
- Types: Single-point, Two-point, Uniform, Arithmetic crossover

```
Parent 1:  1 0 1 | 1 0 0 1
Parent 2:  0 1 0 | 0 1 1 0
           ------+--------
Child 1:   1 0 1 | 0 1 1 0  (Single-point crossover)
Child 2:   0 1 0 | 1 0 0 1
```

#### c) Mutation

- Makes small random changes to an individual
- Maintains diversity and prevents premature convergence
- Types: Bit-flip, Swap, Inversion, Gaussian mutation

```
Before mutation: 1 0 1 1 0 0 1
After mutation:  1 0 1 0 0 0 1  (bit-flip at position 4)
```

---

## 4. Evolutionary Algorithms

### General Framework (Flowchart):

```
Initialize Population → Evaluate Fitness → Selection →
Crossover → Mutation → Evaluate Fitness →
[If termination condition met → Stop, else → Selection]
```

### 4.1 Genetic Algorithm (GA)

- **Representation:** Binary strings (commonly)
- **Selection:** Roulette wheel, Tournament
- **Crossover:** Single-point, Multi-point
- **Mutation:** Bit-flip
- **Focus:** Crossover is the primary operator
- **Developed by:** John Holland (1975)

### 4.2 Evolution Strategies (ES)

- **Representation:** Real-valued vectors
- **Selection:** (μ, λ) or (μ + λ) selection
- **Crossover:** Intermediate or discrete recombination
- **Mutation:** Gaussian perturbation (primary operator)
- **Focus:** Self-adaptive mutation step sizes
- **Developed by:** Rechenberg & Schwefel (1960s)

### 4.3 Evolutionary Programming (EP)

- **Representation:** Real-valued vectors
- **Selection:** Tournament selection
- **Crossover:** No crossover (only mutation)
- **Mutation:** Gaussian mutation with self-adaptation
- **Focus:** Behavioral evolution (phenotype level)
- **Developed by:** Fogel (1960s)

### 4.4 Genetic Programming (GP)

- **Representation:** Tree structures (programs)
- **Selection:** Tournament selection
- **Crossover:** Subtree crossover
- **Mutation:** Subtree mutation, point mutation
- **Focus:** Evolving computer programs/expressions
- **Developed by:** John Koza (1990s)

**Application in Symbolic Regression:**

- Goal: Find a mathematical expression that best fits given data
- GP evolves tree-structured expressions (e.g., x² + 3x + 2)
- Terminals: Variables (x, y) and constants
- Functions: +, -, ×, ÷, sin, cos, exp
- Fitness: How well the expression fits the training data (e.g., MSE)

---

## 5. Performance Measures of EA

1. **Convergence Speed** – How quickly the algorithm reaches the optimal/near-optimal solution
2. **Solution Quality** – How close the best solution is to the known optimum
3. **Success Rate** – Percentage of runs that find acceptable solutions
4. **Computational Effort** – Number of fitness evaluations needed
5. **Diversity Maintenance** – Ability to maintain diverse population to avoid premature convergence
6. **Robustness** – Consistency of performance across different problem instances
7. **Scalability** – Performance as problem size increases
8. **Online Performance** – Average fitness of all evaluated solutions
9. **Offline Performance** – Average of best fitness values found up to each generation

---

## 6. Evolutionary Computation vs Classical Optimization

| Parameter                   | Evolutionary Computation               | Classical Optimization                                       |
| --------------------------- | -------------------------------------- | ------------------------------------------------------------ |
| **Search Method**           | Population-based, stochastic           | Single-point, deterministic                                  |
| **Gradient Requirement**    | Does not require gradient              | Often requires gradient (e.g., Newton's method)              |
| **Search Space**            | Can handle discrete, continuous, mixed | Usually continuous                                           |
| **Global vs Local**         | Tends toward global optima             | Often gets stuck in local optima                             |
| **Problem Knowledge**       | Only needs fitness function            | Needs mathematical properties (convexity, differentiability) |
| **Parallelism**             | Naturally parallel                     | Mostly sequential                                            |
| **Multi-modal Problems**    | Handles well                           | Struggles with multiple optima                               |
| **Constraints**             | Can handle complex constraints         | Needs well-defined constraint formulations                   |
| **Computational Cost**      | Higher (many evaluations)              | Lower for smooth problems                                    |
| **Guarantee of Optimality** | No guarantee (heuristic)               | May guarantee optimality for convex problems                 |

---

## 7. Advanced Topics

### 7.1 Constraint Handling

**Problem:** Many real-world optimization problems have constraints that solutions must satisfy.

**Methods:**

1. **Penalty Function** – Add penalty to fitness for constraint violations
   - Static penalty: Fixed penalty values
   - Dynamic penalty: Increases over generations
   - Adaptive penalty: Adjusts based on search progress
2. **Repair Mechanism** – Fix infeasible solutions to make them feasible
3. **Decoder-based** – Use representation that always produces feasible solutions
4. **Separation of Constraints and Objectives** – Feasible solutions always preferred over infeasible ones
5. **Multi-objective approach** – Treat constraints as additional objectives

### 7.2 Multi-objective Optimization

- Problems with multiple conflicting objectives (e.g., minimize cost AND maximize quality)
- No single optimal solution → **Pareto Front** (set of non-dominated solutions)
- Algorithms: NSGA-II, MOEA/D, SPEA2
- A solution **dominates** another if it is better in at least one objective and no worse in all others

### 7.3 Dynamic Environments

- Optimization landscape changes over time
- Challenges: Need to track moving optima, maintain diversity
- Strategies: Increase mutation, use memory, multi-population approaches

---

## 8. Swarm Intelligence & Ant Colony Optimization

### Definition of Swarm Intelligence:

Swarm Intelligence (SI) is a collective behavior of decentralized, self-organized systems (natural or artificial). It is inspired by social insects (ants, bees) and other animal groups (bird flocks, fish schools).

**Key Principles:**

- No central control
- Simple individual rules → complex collective behavior
- Emergence, self-organization, stigmergy (indirect communication)

### SI vs Traditional Optimization:

| Swarm Intelligence           | Traditional Optimization       |
| ---------------------------- | ------------------------------ |
| Population-based             | Often single-solution          |
| Decentralized control        | Centralized logic              |
| Uses collective intelligence | Uses mathematical formulations |
| Inspired by nature           | Based on mathematical theory   |
| Handles dynamic environments | Designed for static problems   |
| No gradient needed           | Often needs gradient           |

### Ant Colony Optimization (ACO)

**Inspired by:** Real ants finding shortest paths using pheromone trails

**Key Idea:** Artificial ants build solutions by moving on a graph, depositing pheromone on edges. Better paths get more pheromone → attract more ants.

**Algorithm Steps:**

```
1. Initialize pheromone trails (τ) on all edges
2. Place ants at starting nodes
3. Each ant constructs a solution:
   - At each step, choose next node based on:
     P(i→j) = [τ(i,j)]^α × [η(i,j)]^β / Σ[τ(i,k)]^α × [η(i,k)]^β
     where:
       τ(i,j) = pheromone on edge (i,j)
       η(i,j) = heuristic value (e.g., 1/distance)
       α = pheromone influence
       β = heuristic influence
4. Evaluate all solutions (fitness)
5. Update pheromone:
   - Evaporation: τ(i,j) = (1-ρ) × τ(i,j)
   - Deposit: τ(i,j) = τ(i,j) + Δτ (for edges in good solutions)
6. Repeat from step 2 until termination
```

**Key Components:**

- **Pheromone trail (τ):** Represents learned desirability of a path
- **Heuristic information (η):** Problem-specific guidance
- **Evaporation rate (ρ):** Prevents convergence to suboptimal paths
- **Probabilistic transition rule:** Balances exploration and exploitation

**Applications:** Travelling Salesman Problem, Vehicle Routing, Network Routing, Job Scheduling

## 📋 EXAM ANSWERS (Model Answers)

### Q1. What are the key performance measures used to evaluate the effectiveness of evolutionary algorithms? [6 marks]

**Answer:**

**Definition:** Performance measures are metrics used to assess how well an evolutionary algorithm performs in solving optimization problems.

**Key Performance Measures:**

1. **Convergence Speed:** The rate at which the algorithm approaches the optimal solution. Measured by number of generations or fitness evaluations needed to reach a threshold.

2. **Solution Quality (Best Fitness):** How close the final best solution is to the known global optimum. Often measured as error = |f(best) - f(optimal)|.

3. **Success Rate:** The proportion of independent runs that successfully find an acceptable solution within given computational budget.

4. **Computational Effort:** Total number of fitness function evaluations required. This is the most common cost metric since fitness evaluation is usually the most expensive operation.

5. **Robustness:** Consistency of results across multiple runs. Measured by standard deviation of results over several independent runs.

6. **Diversity Maintenance:** Ability to maintain a spread of solutions in the population. Prevents premature convergence to local optima.

7. **Online vs Offline Performance:**
   - Online: Average fitness of ALL solutions evaluated (measures overall search efficiency)
   - Offline: Average of best-so-far fitness values (measures optimization progress)

8. **Scalability:** How performance degrades as problem dimensionality or complexity increases.

---

### Q2. What is swarm intelligence, and how does it differ from traditional optimization techniques? [6 marks]

**Answer:**

**Definition:** Swarm Intelligence (SI) is a branch of computational intelligence that studies the collective behavior of decentralized, self-organized systems. It is inspired by the social behavior of biological organisms like ants, bees, bird flocks, and fish schools.

**Key Characteristics of SI:**

- Decentralized control (no leader)
- Simple individual agents with limited capabilities
- Complex intelligent behavior emerges from interactions
- Communication through stigmergy (environment modification)

**Differences from Traditional Optimization:**

| Parameter         | Swarm Intelligence                 | Traditional Optimization            |
| ----------------- | ---------------------------------- | ----------------------------------- |
| Control           | Decentralized, self-organized      | Centralized algorithm               |
| Search Strategy   | Population-based, collaborative    | Often single-point search           |
| Gradient          | Not required                       | Often requires gradient/derivative  |
| Inspiration       | Nature (ant colonies, bird flocks) | Mathematical theory                 |
| Adaptability      | Adapts to dynamic changes          | Designed for static problems        |
| Global Search     | Good at avoiding local optima      | May get trapped in local optima     |
| Problem Knowledge | Minimal (only fitness function)    | Needs continuity, differentiability |
| Parallelism       | Inherently parallel                | Mostly sequential                   |

**Examples of SI algorithms:** Ant Colony Optimization (ACO), Particle Swarm Optimization (PSO), Bee Colony Algorithm

---

### Q3. Define Genetic Programming and its application in symbolic regression? [6 marks]

**Answer:**

**Definition:** Genetic Programming (GP) is an evolutionary algorithm technique where the individuals in the population are computer programs represented as tree structures. GP evolves programs that solve a given problem by applying genetic operators like crossover and mutation to program trees.

**Key Features:**

- **Representation:** Tree-structured programs (parse trees)
- **Terminal Set:** Variables (x, y), constants (1, 2, π)
- **Function Set:** Operators (+, -, ×, ÷, sin, cos, log)
- **Crossover:** Subtree exchange between two parent trees
- **Mutation:** Random subtree replacement
- **Fitness:** How well the evolved program solves the problem

**Diagram (GP Tree Example):**

```
        +
       / \
      ×    3
     / \
    x    x     → represents: x² + 3
```

**Application in Symbolic Regression:**

Symbolic regression is the task of finding a mathematical expression that best fits a given set of data points without assuming the form of the equation.

**Process:**

1. **Input:** Set of data points {(x₁, y₁), (x₂, y₂), ..., (xₙ, yₙ)}
2. **Goal:** Find function f(x) such that f(xᵢ) ≈ yᵢ for all data points
3. **GP approach:**
   - Each individual is a mathematical expression (tree)
   - Fitness = negative mean squared error (lower error → higher fitness)
   - Evolution finds the best-fitting expression
4. **Output:** A symbolic formula (e.g., y = 2x² + x - 1)

**Advantage over traditional regression:** No need to predefine the model structure; GP discovers both the form and parameters of the equation.

---

### Q4. Discuss the importance of selection in evolutionary algorithms and its impact on convergence of the algorithm? [6 marks]

**Answer:**

**Definition:** Selection is the genetic operator that determines which individuals from the current population will be chosen as parents for producing the next generation. It is based on the principle of "survival of the fittest."

**Importance of Selection:**

1. **Drives Evolution:** Selection creates selection pressure that pushes the population toward better solutions over generations.

2. **Balances Exploration vs Exploitation:**
   - Strong selection pressure → fast convergence (exploitation) but risk of premature convergence
   - Weak selection pressure → more diversity (exploration) but slow convergence

3. **Maintains Population Quality:** Ensures that good genetic material is preserved and propagated.

**Common Selection Methods:**

- **Roulette Wheel:** Probability proportional to fitness
- **Tournament Selection:** Compare k random individuals, select best
- **Rank-based:** Selection based on rank, not absolute fitness
- **Elitism:** Best individuals always survive to next generation

**Impact on Convergence:**

| Selection Pressure | Effect on Convergence                                              |
| ------------------ | ------------------------------------------------------------------ |
| Too High           | Premature convergence to local optima; loss of diversity           |
| Too Low            | Slow convergence; algorithm behaves like random search             |
| Balanced           | Steady convergence toward global optimum with maintained diversity |

**Key Insight:** Selection does NOT introduce new genetic material—it only determines which existing solutions get to reproduce. Crossover and mutation create new solutions. Selection guides the search direction.

---

### Q5. Explain the concept of population in evolutionary computing and its significance in the optimization process? [6 marks]

**Answer:**

**Definition:** A population in evolutionary computing is a collection of candidate solutions (individuals) that exist simultaneously in a given generation. Each individual represents a potential solution to the optimization problem.

**Key Characteristics:**

- Fixed or variable size (typically fixed)
- Initialized randomly or with heuristics
- Evolves over generations through genetic operators
- Represents parallel exploration of the search space

**Significance in Optimization:**

1. **Parallel Search:** Unlike single-point methods, a population searches multiple regions of the search space simultaneously, increasing the chance of finding the global optimum.

2. **Diversity Maintenance:** A diverse population explores different regions, reducing the risk of getting trapped in local optima.

3. **Information Sharing:** Through crossover, good building blocks from different individuals can be combined to create better solutions.

4. **Robustness:** Multiple solutions provide redundancy—even if some individuals are poor, others may be on good search paths.

**Population Size Considerations:**

| Small Population                     | Large Population          |
| ------------------------------------ | ------------------------- |
| Fast per generation                  | Slow per generation       |
| Less diversity                       | More diversity            |
| Higher risk of premature convergence | Better exploration        |
| May miss good regions                | Higher computational cost |

**Typical size:** 50-200 individuals (problem-dependent)

**Population Initialization Methods:**

- Random initialization (most common)
- Heuristic-based seeding
- Opposition-based initialization

---

### Q6. Compare and contrast genetic algorithms, evolution strategies, evolutionary programming. [6 marks]

**Answer:**

| Parameter            | Genetic Algorithm (GA)           | Evolution Strategies (ES)         | Evolutionary Programming (EP)             |
| -------------------- | -------------------------------- | --------------------------------- | ----------------------------------------- |
| **Developer**        | Holland (1975)                   | Rechenberg & Schwefel (1960s)     | Fogel (1960s)                             |
| **Representation**   | Binary strings (typically)       | Real-valued vectors               | Real-valued vectors                       |
| **Primary Operator** | Crossover                        | Mutation (self-adaptive)          | Mutation (self-adaptive)                  |
| **Crossover**        | Yes (main operator)              | Yes (intermediate/discrete)       | No crossover                              |
| **Mutation**         | Secondary (bit-flip)             | Primary (Gaussian)                | Primary (Gaussian)                        |
| **Selection**        | Roulette wheel, Tournament       | (μ,λ) or (μ+λ) deterministic      | Tournament (stochastic)                   |
| **Self-adaptation**  | No                               | Yes (step sizes evolve)           | Yes (variance evolves)                    |
| **Focus Level**      | Genotype (encoding)              | Genotype + strategy parameters    | Phenotype (behavior)                      |
| **Emphasis**         | Recombination of building blocks | Step-size adaptation              | Species-level evolution                   |
| **Best For**         | Combinatorial problems           | Continuous parameter optimization | Continuous optimization, machine learning |

**Similarities:**

- All are population-based
- All use fitness-based selection
- All are iterative and stochastic
- All inspired by biological evolution
- All do not require gradient information

**Key Differences in Philosophy:**

- GA: Emphasizes combining partial solutions (building blocks hypothesis)
- ES: Emphasizes learning how to mutate (self-adaptation of strategy parameters)
- EP: Emphasizes behavioral adaptation without recombination (species cannot crossbreed)

---

### Q7. Define Swarm Intelligence and Explain Ant Colony Optimization algorithm. [8 marks]

**Answer:**

**Definition of Swarm Intelligence:**
Swarm Intelligence (SI) is a computational intelligence paradigm inspired by the collective behavior of social organisms such as ants, bees, birds, and fish. It involves decentralized, self-organized systems where simple agents interact locally with each other and the environment to produce complex, intelligent global behavior without any centralized control.

**Principles of SI:**

- Self-organization
- Positive feedback (reinforcement)
- Negative feedback (evaporation/decay)
- Randomness/fluctuations
- Multiple interactions among agents

---

**Ant Colony Optimization (ACO):**

**Inspiration:** Real ants deposit a chemical substance called **pheromone** on their path. Other ants detect this pheromone and tend to follow paths with higher concentration, creating a positive feedback loop that leads to shortest path discovery.

**Algorithm (Step-by-step):**

```
Step 1: INITIALIZATION
   - Set pheromone τ(i,j) = τ₀ for all edges
   - Define parameters: α, β, ρ, number of ants m

Step 2: SOLUTION CONSTRUCTION
   For each ant k:
     - Start at a random/designated node
     - At each node i, select next node j with probability:

       P(i→j) = [τ(i,j)]^α × [η(i,j)]^β
                 ─────────────────────────────
                 Σ [τ(i,l)]^α × [η(i,l)]^β

       where: τ(i,j) = pheromone on edge (i,j)
              η(i,j) = heuristic info (e.g., 1/d(i,j))
              α = pheromone importance factor
              β = heuristic importance factor
     - Continue until solution is complete

Step 3: PHEROMONE UPDATE
   a) Evaporation: τ(i,j) = (1 - ρ) × τ(i,j) for all edges
   b) Deposit: For each ant k:
      τ(i,j) = τ(i,j) + Δτₖ(i,j)
      where Δτₖ(i,j) = Q/Lₖ if ant k used edge (i,j)
                      = 0 otherwise
      (Q = constant, Lₖ = tour length of ant k)

Step 4: TERMINATION CHECK
   If stopping criterion met → Output best solution
   Else → Go to Step 2
```

**Diagram:**

```
[Initialize Pheromone] → [Place Ants] → [Construct Solutions]
         ↑                                      ↓
    [Termination?] ←── [Update Pheromone] ←── [Evaluate]
         ↓ (Yes)
    [Output Best Solution]
```

**Key Parameters:**

- **α (alpha):** Controls influence of pheromone (higher = more pheromone influence)
- **β (beta):** Controls influence of heuristic (higher = more greedy)
- **ρ (rho):** Evaporation rate (prevents stagnation)
- **m:** Number of ants

**Applications:**

- Travelling Salesman Problem (TSP)
- Vehicle Routing Problem
- Network Routing
- Job Shop Scheduling
- Quadratic Assignment Problem

---

### Q8. Write short note on: i) Evolutionary Computation versus Classical Optimization ii) Ant Colony Optimization algorithm [10 marks]

**Answer:**

#### i) Evolutionary Computation versus Classical Optimization [5 marks]

**Evolutionary Computation (EC)** refers to a family of algorithms inspired by biological evolution (GA, ES, EP, GP). **Classical Optimization** includes mathematical methods like gradient descent, Newton's method, linear programming, etc.

**Comparison:**

| Aspect             | Evolutionary Computation     | Classical Optimization                      |
| ------------------ | ---------------------------- | ------------------------------------------- |
| Approach           | Population-based, stochastic | Single-point, deterministic                 |
| Gradient           | Not needed                   | Usually required                            |
| Search Space       | Discrete, continuous, mixed  | Usually continuous                          |
| Global Search      | Naturally explores globally  | Often trapped in local optima               |
| Constraints        | Handles complex constraints  | Needs well-defined mathematical formulation |
| Problem Structure  | Works on black-box problems  | Requires problem structure knowledge        |
| Convergence        | No mathematical guarantee    | Convergence proofs for convex problems      |
| Computational Cost | High (many evaluations)      | Low for smooth, convex problems             |
| Parallelism        | Naturally parallel           | Mostly sequential                           |
| Multi-modal        | Handles multiple optima      | Converges to single optimum                 |

**When to use EC:** Non-differentiable functions, multi-modal landscapes, no closed-form available, black-box optimization, combinatorial problems.

**When to use Classical:** Smooth convex problems, when gradient is available, when mathematical guarantees needed, small well-defined problems.

---

#### ii) Ant Colony Optimization Algorithm [5 marks]

_(Refer to the detailed explanation in Q7 above — write the algorithm steps, formula, diagram, and applications. For 5 marks, include: definition, biological inspiration, algorithm steps with probability formula, pheromone update rule, and 2-3 applications.)_

---

### Q9. Write short note on: i) Tournament Selection Method ii) Constraint Handling [10 marks]

**Answer:**

#### i) Tournament Selection Method [5 marks]

**Definition:** Tournament selection is a selection method where a fixed number of individuals (tournament size k) are randomly chosen from the population, and the fittest among them is selected as a parent.

**Algorithm:**

```
1. Randomly select k individuals from population
2. Compare their fitness values
3. Select the individual with best fitness
4. Repeat for each parent needed
```

**Example (k=3):**

```
Population: [A(fit=8), B(fit=5), C(fit=12), D(fit=3), E(fit=9)]
Tournament 1: Pick {B, C, E} → Winner: C (fitness=12)
Tournament 2: Pick {A, D, E} → Winner: E (fitness=9)
Parents: C and E
```

**Advantages:**

- Easy to implement
- No need to sort the entire population
- Selection pressure adjustable via tournament size k
- Works well with negative fitness values
- Parallelizable

**Effect of Tournament Size:**

- k = 1 → Random selection (no selection pressure)
- k = 2 → Binary tournament (moderate pressure)
- k = N → Always selects best (maximum pressure, like elitism)
- Larger k → Higher selection pressure → Faster but riskier convergence

**Selection Pressure Control:** By adjusting k, we can balance exploration (low k) and exploitation (high k).

---

#### ii) Constraint Handling [5 marks]

**Definition:** Constraint handling refers to techniques used in evolutionary algorithms to deal with optimization problems that have equality or inequality constraints limiting the feasible solution space.

**Problem Formulation:**

```
Minimize f(x)
Subject to: gᵢ(x) ≤ 0  (inequality constraints)
            hⱼ(x) = 0  (equality constraints)
```

**Methods of Constraint Handling:**

1. **Penalty Function Method:**
   - Add a penalty term to the fitness for constraint violations
   - F(x) = f(x) + Σ rᵢ × max(0, gᵢ(x))²
   - Types: Static, Dynamic (increases over generations), Adaptive
   - Simple but penalty coefficients are hard to tune

2. **Repair Methods:**
   - Transform infeasible solutions into feasible ones
   - Apply repair operator after mutation/crossover
   - Problem-specific; can be computationally expensive

3. **Feasibility Rules (Deb's rules):**
   - Between two feasible solutions → select better fitness
   - Between feasible and infeasible → select feasible
   - Between two infeasible → select less violated
   - No penalty parameters needed

4. **Decoder-based Approach:**
   - Design the representation such that all genotypes map to feasible solutions
   - Guarantees feasibility but may limit search

5. **Multi-objective Approach:**
   - Treat constraint violations as additional objectives
   - Use Pareto dominance to handle constraints

**Most Common in Exams:** Penalty function and Deb's feasibility rules.

---

### Q10. What are genetic operators and what is their role in evolutionary algorithms? [8 marks]

**Answer:**

**Definition:** Genetic operators are the mechanisms in evolutionary algorithms that manipulate individuals (candidate solutions) to create new offspring. They drive the evolutionary search process by introducing variation and selecting promising solutions.

**The Three Main Genetic Operators:**

---

**1. Selection Operator**

**Role:** Chooses individuals from the current population for reproduction based on fitness.

**Types:**

- Roulette Wheel: P(selection) = fitness(i) / Σfitness
- Tournament: Pick k random, select best
- Rank-based: Selection probability based on rank
- Elitism: Best n individuals always survive

**Role in EA:** Creates selection pressure; guides search toward promising regions.

---

**2. Crossover (Recombination) Operator**

**Role:** Combines genetic information from two parent individuals to produce offspring with potentially superior traits.

**Types:**

- **Single-point crossover:**
  ```
  P1: 1 0 1 1 | 0 0 1    → Child1: 1 0 1 1 | 1 1 0
  P2: 0 1 0 0 | 1 1 0    → Child2: 0 1 0 0 | 0 0 1
  ```
- **Two-point crossover:** Two cut points; swap middle segment
- **Uniform crossover:** Each bit randomly from either parent
- **Arithmetic crossover (real-valued):** child = α×P1 + (1-α)×P2

**Role in EA:** Enables exploration of search space by combining building blocks from different solutions (exploitation of existing genetic material).

---

**3. Mutation Operator**

**Role:** Makes small random changes to an individual to introduce new genetic material into the population.

**Types:**

- **Bit-flip mutation (binary):** Flip 0→1 or 1→0 with probability pₘ
  ```
  Before: 1 0 1 1 0 0 1
  After:  1 0 1 0 0 0 1  (position 4 flipped)
  ```
- **Gaussian mutation (real-valued):** x' = x + N(0, σ)
- **Swap mutation:** Swap two positions
- **Inversion mutation:** Reverse a substring

**Role in EA:** Maintains diversity; prevents premature convergence; enables exploration of new regions.

---

**Summary of Roles:**

| Operator  | Primary Role         | Effect                      |
| --------- | -------------------- | --------------------------- |
| Selection | Choose parents       | Guides search direction     |
| Crossover | Combine solutions    | Exploits known good regions |
| Mutation  | Introduce randomness | Explores new regions        |

**Relationship:**

- Selection without variation → no progress (same solutions repeated)
- Variation without selection → random search (no direction)
- Together → directed, adaptive search toward optimal solutions

**Crossover Rate:** Typically 0.6–0.9 (applied frequently)
**Mutation Rate:** Typically 0.001–0.05 (applied sparingly to maintain stability)

---

## 🎯 Quick Revision Tips for Exam Day

1. **Always draw diagrams** — flowcharts of GA, ACO, GP trees
2. **For comparison questions** — use tables (easy to write, easy to read)
3. **Formulas to remember:**
   - ACO probability formula
   - Roulette wheel: P(i) = f(i) / Σf
   - Penalty function: F(x) = f(x) + penalty
4. **Key names:** Holland (GA), Koza (GP), Rechenberg/Schwefel (ES), Fogel (EP), Dorigo (ACO)
5. **For "explain" questions:** Definition → Mechanism → Example → Advantages
6. **Time management:** [6 marks] ≈ 12 min, [8 marks] ≈ 16 min, [10 marks] ≈ 20 min
