#  A Comparative Analysis of Exact Optimization and Genetic Algorithms for Solving the 0–1 Knapsack Problem

---

## 📌 Project Overview

This repository contains a MATLAB-based comparative study evaluating **Exact Optimization methods** against a custom-built **Genetic Algorithm (GA)** for solving the classical **0–1 Knapsack Problem**. 

The study focuses on selecting the optimal subset of items to maximize total value without exceeding a fixed weight capacity constraint. Both optimization paradigms are rigorously benchmarked across **solution quality (optimality gap), convergence behavior, computational execution time, and scalability** as problem dimensionality increases.

---

## 🎯 Objectives

* **Mathematical Formulation:** Formulate the 0–1 Knapsack Problem as a discrete Integer Linear Programming (ILP) optimization model.
* **Exact Baseline Implementation:** Implement exact optimization solvers in MATLAB to guarantee globally optimal solutions.
* **Metaheuristic Design:** Develop a Genetic Algorithm from scratch to search for near-optimal solutions in discrete binary spaces.
* **Benchmarking & Scalability:** Compare algorithmic performance across varying problem sizes ($n \in \{10, 20, 50, 100, 150, 200\}$).
* **Trade-off Evaluation:** Critically analyze the practical advantages, parameter sensitivities, and computational limitations of exact vs. evolutionary algorithms.

---

## 🧮 Mathematical Formulation

Given a set of $n$ items, each with value $v_i$ and weight $w_i$, and a maximum knapsack capacity $W$, the 0–1 Knapsack Problem is formulated as:

$$\max \sum_{i=1}^{n} v_i x_i$$

**Subject to:**

$$\sum_{i=1}^{n} w_i x_i \leq W$$

$$x_i \in \{0, 1\}, \quad \forall i = 1, 2, \dots, n$$

Where $x_i = 1$ indicates that item $i$ is included in the knapsack, and $x_i = 0$ otherwise.

---

## ⚙️ Methodology

### 1. Exact Optimization
* Implemented using deterministic exact solvers (**Dynamic Programming / Integer Linear Programming via `intlinprog`**) to establish the true global optimum ($Z^*$) and serve as the ground-truth benchmark.

### 2. Genetic Algorithm (GA)
* **Chromosome Encoding:** Binary string representation of length $n$ ($x \in \{0,1\}^n$).
* **Fitness Evaluation & Constraint Handling:** Evaluates total item value while applying a penalty/repair mechanism for solutions exceeding capacity $W$.
* **Evolutionary Operators:**
  * **Selection:** Tournament / Roulette-wheel selection to preserve high-fitness candidates.
  * **Crossover:** Single-point / Uniform crossover to recombine genetic material.
  * **Mutation:** Bit-flip mutation to maintain population diversity and prevent premature convergence.

---

## 📊 Experimental Analysis & Key Findings

Two primary experiments were conducted to evaluate algorithmic efficiency:
1. **Convergence Analysis:** Evaluated generation-by-generation fitness progression on a **20-item** benchmark instance.
2. **Scalability Benchmarking:** Tested runtime and solution accuracy across problem instances of **10, 50, 100, 150, and 200 items**.

### Summary of Results

| Problem Size ($n$) | Exact Optimum ($Z^*$) | GA Best Value ($Z_{\text{GA}}$) | GA Optimality (%) | Observation |
| :--- | :---: | :---: | :---: | :--- |
| **10 Items** | Optimal | Optimal | **100.00%** | GA converges rapidly to the exact global optimum. |
| **20 Items** | **541** | **521** | **96.30%** | GA achieves near-optimal solution with minor optimality gap. |
| **50 Items** | Optimal | Near-Optimal | **91.08%** | Performance degrades slightly as search space expands ($2^{50}$). |
| **100+ Items** | Optimal | Infeasible* | **N/A** | Under fixed baseline GA parameters, random initialization struggled to locate feasible regions without repair operators. |

### Key Takeaways
* **Solution Quality vs. Scale:** While the Genetic Algorithm achieves high accuracy on small-to-medium instances ($96.30\%$ on 20 items, $91.08\%$ on 50 items), fixed-parameter GAs face feasibility bottlenecks in tightly constrained high-dimensional spaces ($n \ge 100$).
* **Exact Solver Efficiency:** Modern exact methods (Dynamic Programming / Branch-and-Bound) maintained negligible computational times across all tested sizes up to $n = 200$.
* **Metaheuristic Necessity:** Genetic Algorithms require tailored constraint-handling strategies (e.g., greedy repair operators) and adaptive parameter tuning to remain competitive on large-scale combinatorial problems.

---

## 📂 Repository Structure

```text
├── src/
│   ├── exact_knapsack.m          # Exact optimization solver implementation
│   ├── ga_knapsack.m             # Genetic Algorithm implementation
│   ├── convergence_experiment.m  # 20-item convergence tracking script
│   └── scalability_benchmark.m   # Multi-size (10 to 200 items) benchmark script
├── results/
│   ├── convergence_plot.png      # Fitness vs. Generation visualization
│   └── scalability_table.csv     # Execution time and accuracy metrics
├── report/
│   └── Case_Study_Report.pdf     # Full academic report & mathematical derivations
└── README.md
