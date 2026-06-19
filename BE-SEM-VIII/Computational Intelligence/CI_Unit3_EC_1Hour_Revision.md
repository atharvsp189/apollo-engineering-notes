# Unit III: Evolutionary Computing - 1 Hour Exam Revision

Use this as a quick pre-exam sheet. For most answers, write: **definition + key terms + algorithm/diagram + example + advantages/limitations**.

---

## 1. Evolutionary Computing (EC)

**Definition:** Evolutionary Computing is a family of population-based optimization algorithms inspired by biological evolution. It uses selection, reproduction, crossover, and mutation to evolve better solutions over generations.

### Key Characteristics

- **Population-based:** Works with many candidate solutions at once.
- **Stochastic:** Uses randomness in selection and variation.
- **Iterative:** Improves solutions generation by generation.
- **Fitness-driven:** Better solutions have higher chance of survival.
- **Black-box friendly:** Needs mainly a fitness function, not gradients.

### General EC Flow

```text
Initialize population
      |
      v
Evaluate fitness
      |
      v
Select parents
      |
      v
Apply crossover and mutation
      |
      v
Evaluate offspring
      |
      v
Replace population / keep elites
      |
      v
Termination condition?
      |
      v
Output best solution
```

---

## 2. Important Terminology

| Term | Meaning |
|---|---|
| Individual | One candidate solution |
| Population | Set of candidate solutions |
| Chromosome | Encoded solution |
| Gene | One part/position of chromosome |
| Allele | Value of a gene |
| Fitness function | Measures quality of solution |
| Generation | One iteration of the algorithm |
| Genotype | Encoded/internal form |
| Phenotype | Actual decoded solution |
| Search space | Set of all possible solutions |

### Population Significance

- Explores many regions of search space simultaneously.
- Helps avoid local optima.
- Maintains diversity.
- Enables parallel processing.
- Gives robustness in complex problems.

---

## 3. Genetic Operators

**Definition:** Genetic operators create new candidate solutions from existing ones and drive the evolutionary search process.

### Main Operators

| Operator | Role | Common Types |
|---|---|---|
| Selection | Chooses parents based on fitness | Roulette, tournament, rank, elitism |
| Crossover | Combines parents to create offspring | Single-point, two-point, uniform, arithmetic |
| Mutation | Randomly changes genes | Bit-flip, swap, inversion, Gaussian |

### Selection

- Selects fitter individuals for reproduction.
- Creates **selection pressure**.
- Does not create new genetic material; it only chooses existing solutions.

**Tournament selection:** Randomly choose `k` individuals and select the fittest among them.

```text
k small -> low pressure -> more diversity, slow convergence
k large -> high pressure -> fast convergence, risk of premature convergence
```

### Crossover Example

```text
Parent 1: 1 0 1 | 1 0 0
Parent 2: 0 1 0 | 0 1 1

Child 1:  1 0 1 | 0 1 1
Child 2:  0 1 0 | 1 0 0
```

### Mutation Example

```text
Before: 1 0 1 1 0 0
After:  1 0 1 0 0 0
```

### Key Exam Line

**Selection guides the search, crossover exploits existing good building blocks, and mutation explores new regions while maintaining diversity.**

---

## 4. Types of Evolutionary Algorithms

## 4.1 Genetic Algorithm (GA)

**Developed by:** John Holland, 1975  
**Focus:** Recombination/crossover of encoded solutions.

- Representation: Mostly binary strings.
- Selection: Roulette wheel, tournament.
- Crossover: Single-point, multi-point.
- Mutation: Bit-flip.
- Best for: Combinatorial and discrete optimization.

## 4.2 Evolution Strategies (ES)

**Developed by:** Rechenberg and Schwefel, 1960s  
**Focus:** Real-valued optimization and self-adaptive mutation.

- Representation: Real-valued vectors.
- Selection: `(mu, lambda)` or `(mu + lambda)`.
- Mutation: Gaussian mutation is primary.
- Special feature: Mutation step size adapts.
- Best for: Continuous parameter optimization.

## 4.3 Evolutionary Programming (EP)

**Developed by:** Fogel, 1960s  
**Focus:** Behavioral/phenotype evolution.

- Representation: Real-valued vectors or structures.
- Selection: Tournament.
- Crossover: Usually absent.
- Mutation: Main operator.
- Best for: Continuous optimization and machine learning.

## 4.4 Genetic Programming (GP)

**Developed by:** John Koza, 1990s  
**Focus:** Evolving programs or mathematical expressions.

- Representation: Tree structures.
- Crossover: Subtree crossover.
- Mutation: Subtree or point mutation.
- Best for: Symbolic regression, rule generation, program synthesis.

### GP in Symbolic Regression

Goal: Find a mathematical expression that fits given data.

```text
Terminals: x, y, constants
Functions: +, -, *, /, sin, cos
Fitness: Error such as MSE
Output: Expression like x^2 + 3x + 2
```

---

## 5. GA vs ES vs EP vs GP

| Feature | GA | ES | EP | GP |
|---|---|---|---|---|
| Representation | Binary/string | Real vectors | Real vectors/behavior | Trees/programs |
| Main operator | Crossover | Mutation | Mutation | Crossover + mutation |
| Crossover | Important | Optional | Usually absent | Subtree crossover |
| Mutation | Secondary | Primary | Primary | Tree mutation |
| Best for | Discrete/combinatorial | Continuous optimization | Behavioral optimization | Program/expression evolution |
| Key name | Holland | Rechenberg, Schwefel | Fogel | Koza |

### Similarities

- Population-based.
- Fitness-driven.
- Stochastic and iterative.
- Inspired by biological evolution.
- Do not require gradient information.

---

## 6. Performance Measures of Evolutionary Algorithms

Write any 5-6 for a 6-mark answer:

- **Convergence speed:** How fast it reaches good solution.
- **Solution quality:** How close result is to optimum.
- **Success rate:** Percentage of runs giving acceptable solution.
- **Computational effort:** Number of fitness evaluations/time needed.
- **Diversity maintenance:** Ability to avoid premature convergence.
- **Robustness:** Consistent performance on different problems.
- **Scalability:** Performance when problem size increases.
- **Online performance:** Average fitness of all evaluated solutions.
- **Offline performance:** Average of best-so-far fitness values.

---

## 7. EC vs Classical Optimization

| Point | Evolutionary Computing | Classical Optimization |
|---|---|---|
| Search | Population-based | Single-point |
| Nature | Stochastic | Mostly deterministic |
| Gradient | Not required | Often required |
| Problem type | Discrete, continuous, mixed | Usually continuous |
| Local optima | Better at escaping | Can get stuck |
| Knowledge needed | Fitness function enough | Needs mathematical structure |
| Multi-modal problems | Handles well | Struggles |
| Parallelism | Naturally parallel | Mostly sequential |
| Cost | High due to many evaluations | Lower for smooth problems |
| Guarantee | No global guarantee | Possible for convex problems |

### When to Use EC

- Function is non-differentiable.
- Search space is large or multi-modal.
- Problem is black-box.
- Combinatorial optimization.
- Multiple good solutions are acceptable.

---

## 8. Constraint Handling

**Definition:** Constraint handling means managing solutions that violate equality or inequality constraints in evolutionary algorithms.

### Common Methods

| Method | Idea |
|---|---|
| Penalty function | Add penalty to fitness for constraint violation |
| Repair method | Convert infeasible solution into feasible solution |
| Decoder-based | Encoding always produces feasible solution |
| Feasibility rules | Prefer feasible solutions over infeasible ones |
| Multi-objective approach | Treat constraint violation as another objective |

### Penalty Function

```text
Modified fitness = original fitness + penalty
```

For minimization:

```text
F(x) = f(x) + penalty for violated constraints
```

### Deb's Feasibility Rules

- Feasible vs feasible: choose better fitness.
- Feasible vs infeasible: choose feasible.
- Infeasible vs infeasible: choose less constraint violation.

---

## 9. Multi-objective and Dynamic Optimization

### Multi-objective Optimization

- Deals with multiple conflicting objectives.
- Example: minimize cost and maximize quality.
- No single best solution; output is a **Pareto front**.
- A solution dominates another if it is no worse in all objectives and better in at least one.
- Algorithms: NSGA-II, SPEA2, MOEA/D.

### Dynamic Environments

- Optimization landscape changes over time.
- Challenge: Track moving optima.
- Strategies: Increase mutation, use memory, maintain diversity, use multiple populations.

---

## 10. Swarm Intelligence (SI)

**Definition:** Swarm Intelligence is the collective behavior of decentralized, self-organized agents inspired by groups such as ants, bees, birds, and fish.

### Key Principles

- No central control.
- Simple agents.
- Local interaction.
- Cooperation.
- Emergent global behavior.
- Adaptability.

### SI vs Traditional Optimization

| Swarm Intelligence | Traditional Optimization |
|---|---|
| Many agents | Usually single search point |
| Decentralized | Centralized/sequential |
| Stochastic | Often deterministic |
| Good for complex problems | Good for structured problems |
| No gradient needed | Often needs gradient |
| Can adapt dynamically | Often static |

Examples: ACO, PSO, Bee Colony Algorithm.

---

## 11. Ant Colony Optimization (ACO)

**Definition:** ACO is a swarm intelligence algorithm inspired by ants finding shortest paths using pheromone trails.

### Key Idea

Artificial ants construct solutions on a graph. Good paths receive more pheromone, so future ants are more likely to choose them. Pheromone evaporation prevents stagnation.

### ACO Algorithm

```text
1. Initialize pheromone on all edges
2. Place ants at starting nodes
3. Each ant constructs a solution probabilistically
4. Evaluate all constructed solutions
5. Evaporate pheromone
6. Deposit pheromone on good paths
7. Repeat until stopping condition
8. Output best solution
```

### Transition Probability

```text
P(i,j) = [tau(i,j)]^alpha * [eta(i,j)]^beta
         -------------------------------------
         sum [tau(i,k)]^alpha * [eta(i,k)]^beta
```

Where:

- `tau(i,j)` = pheromone on edge `(i,j)`.
- `eta(i,j)` = heuristic value, usually `1 / distance`.
- `alpha` = importance of pheromone.
- `beta` = importance of heuristic.
- `rho` = evaporation rate.

### Pheromone Update

```text
Evaporation: tau(i,j) = (1 - rho) * tau(i,j)
Deposit:     tau(i,j) = tau(i,j) + delta_tau
```

### Applications

- Travelling Salesman Problem.
- Vehicle routing.
- Network routing.
- Job scheduling.
- Quadratic assignment.

---

## 12. Must-Remember Names

| Concept | Person | Time |
|---|---|---|
| Genetic Algorithm | John Holland | 1975 |
| Evolution Strategies | Rechenberg, Schwefel | 1960s |
| Evolutionary Programming | Fogel | 1960s |
| Genetic Programming | John Koza | 1990s |
| Ant Colony Optimization | Dorigo | 1990s |

---

## 13. Last-Minute Answer Templates

### If asked: "Explain Evolutionary Computing"

Write definition, key characteristics, general flow, and applications. Mention population, fitness, selection, crossover, mutation, and termination.

### If asked: "Explain Genetic Operators"

Write definition, then explain selection, crossover, mutation with role and example. Add: **selection guides, crossover combines, mutation diversifies**.

### If asked: "Compare GA, ES, EP"

Use a table. GA uses crossover and binary/string representation; ES uses real vectors and self-adaptive Gaussian mutation; EP mainly uses mutation and focuses on behavior.

### If asked: "Explain GP and symbolic regression"

Write GP evolves tree-structured programs. For symbolic regression, terminals are variables/constants, functions are mathematical operators, fitness is error such as MSE, output is a formula.

### If asked: "Explain ACO"

Write ant inspiration, pheromone, heuristic, probability formula, pheromone evaporation/deposit, algorithm steps, and applications.

### If asked: "Explain selection impact"

Selection pressure controls convergence. High pressure gives fast convergence but risk of local optima; low pressure maintains diversity but slows convergence.

---

## 14. One-Page Memory Hook

```text
EC = population + fitness + selection + crossover + mutation

Individual = candidate solution
Population = set of individuals
Fitness = quality measure
Generation = one iteration

Selection -> choose good parents
Crossover -> combine good solutions
Mutation -> random change for diversity

GA -> Holland -> binary/string -> crossover
ES -> Rechenberg/Schwefel -> real vectors -> mutation step size
EP -> Fogel -> behavior -> mutation only
GP -> Koza -> tree programs -> symbolic regression
ACO -> Dorigo -> pheromone trails -> shortest/good paths

Must draw:
1. EC/GA flowchart
2. Crossover example
3. Mutation example
4. ACO flowchart
5. Comparison tables
```

