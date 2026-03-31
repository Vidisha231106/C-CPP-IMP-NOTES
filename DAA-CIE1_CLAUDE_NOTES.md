# Design and Analysis of Algorithms (DAA)
## Complete Study Notes — Intermediate Level

---

> **How to use these notes:** Each topic builds on the previous. Code examples are in C++. Every algorithm includes: concept → pseudocode → code → complexity analysis → mathematical proof → recurrence relation → recursive tree where applicable.

---

# TABLE OF CONTENTS

1. [Perspectives & Applications](#1-perspectives--applications)
2. [Notion of an Algorithm](#2-notion-of-an-algorithm)
3. [Fundamentals of Algorithmic Problem Solving](#3-fundamentals-of-algorithmic-problem-solving)
4. [Analysis Framework](#4-analysis-framework)
5. [Asymptotic Notations](#5-asymptotic-notations-and-basic-efficiency-classes)
6. [Mathematical Analysis of Non-Recursive Algorithms](#6-mathematical-analysis-of-non-recursive-algorithms)
7. [Mathematical Analysis of Recursive Algorithms](#7-mathematical-analysis-of-recursive-algorithms)
8. [Brute Force — Selection Sort](#8-brute-force--selection-sort)
9. [Brute Force — Bubble Sort](#9-brute-force--bubble-sort)
10. [Divide and Conquer — Merge Sort](#10-divide-and-conquer--merge-sort)
11. [Divide and Conquer — Quick Sort](#11-divide-and-conquer--quick-sort)
12. [Multiplication of Long Integers](#12-multiplication-of-long-integers)
13. [Strassen's Matrix Multiplication](#13-strassens-matrix-multiplication)

---

# UNIT I

---

## 1. Perspectives & Applications

### 1.1 Why Study Algorithms?

An algorithm is the backbone of every software system. Without efficient algorithms, even the most powerful hardware cannot solve large-scale problems in reasonable time. Understanding algorithms is critical across:

---

### 1.2 Business Domain Perspectives

| Domain | Algorithm Use |
|--------|--------------|
| **Banking & Finance** | Fraud detection (graph algorithms), portfolio optimization (dynamic programming), loan risk scoring (ML algorithms), real-time transaction sorting |
| **IT Services** | Load balancing (scheduling algorithms), resource allocation, fault tolerance routing |
| **Manufacturing** | Job scheduling (greedy algorithms), supply optimization, quality control via pattern matching |
| **e-Commerce** | Product recommendation (collaborative filtering), search ranking (PageRank), inventory management |
| **Online Services & Marketing** | Ad auction mechanisms, A/B testing, click-through rate prediction |
| **Logistics & Supply Chain** | Shortest path (Dijkstra, Bellman-Ford), vehicle routing (TSP approximations), warehouse bin packing |
| **Telecommunications** | Network flow optimization, packet routing, compression algorithms (Huffman coding) |

---

### 1.3 Application Perspectives

| Application | Algorithms Involved |
|-------------|-------------------|
| **Communication & Networking** | TCP/IP routing, error-correcting codes, cryptography (RSA, AES) |
| **Search Engines** | PageRank (graph traversal), inverted index (hashing), relevance ranking |
| **Machine Learning** | Gradient descent, backpropagation, decision tree construction |
| **Database Management** | B-tree indexing, query optimization, join algorithms (hash join, merge join) |
| **Software Tools** | Compilers (parsing, code generation), version control (diff algorithms) |
| **Data Organization** | Sorting, searching, hashing, heap operations |
| **GPS Navigation** | Dijkstra's shortest path, A* search, map-matching algorithms |

---

## 2. Notion of an Algorithm

### 2.1 Definition

> An **algorithm** is a finite, well-defined sequence of instructions that takes some input, performs a computation, and produces a correct output in a finite amount of time.

**Formal Definition:**  
An algorithm `A` is a function:
```
A : Input_Space → Output_Space
```
such that for every valid input `I`, `A(I)` terminates and produces the correct output `O`.

---

### 2.2 Properties of a Valid Algorithm

Every algorithm must satisfy **five essential properties** (Knuth):

| Property | Meaning | Example Violation |
|----------|---------|------------------|
| **Finiteness** | Must terminate after finite steps | Infinite loop |
| **Definiteness** | Each step must be precisely defined | "Sort it somehow" |
| **Input** | Zero or more well-defined inputs | Undefined input types |
| **Output** | One or more outputs related to input | No return value |
| **Effectiveness** | Each step must be basic and doable | "Find the largest prime" with no method |

---

### 2.3 Algorithm vs Program

| Aspect | Algorithm | Program |
|--------|-----------|---------|
| Nature | Abstract, language-independent | Concrete, language-specific |
| Termination | Must always terminate | May run indefinitely (OS) |
| Level | Design level | Implementation level |
| Goal | Correctness + efficiency | Correctness + performance |

---

### 2.4 Expressing Algorithms

Algorithms can be expressed as:
1. **Natural language** — English description (ambiguous)
2. **Flowchart** — Graphical representation
3. **Pseudocode** — Structured English-like notation (preferred for design)
4. **Programming language** — Python, C, Java (for implementation)

**Example — Finding Maximum in Array (Pseudocode):**
```
Algorithm FindMax(A[0..n-1])
  // Input: Array A of n numbers
  // Output: The largest element
  
  max ← A[0]
  for i ← 1 to n-1 do
    if A[i] > max
      max ← A[i]
  return max
```

**Same in C++:**
```cpp
int find_max(const std::vector<int>& A) {
    int max_val = A[0];
    for (size_t i = 1; i < A.size(); i++) {
        if (A[i] > max_val) {
            max_val = A[i];
        }
    }
    return max_val;
}
```

---

## 3. Fundamentals of Algorithmic Problem Solving

### 3.1 Algorithm Design Process (Step-by-Step)

```
Problem Statement
       ↓
Understanding the Problem
       ↓
Decide on: Exact vs Approximate Solution
           Data Structure
           Algorithm Design Technique
       ↓
Design the Algorithm
       ↓
Prove Correctness
       ↓
Analyze the Algorithm (Time & Space)
       ↓
Code the Algorithm
       ↓
Test & Debug
```

---

### 3.2 Understanding the Problem

Before designing, answer:
- What is the **exact input**? (Type, size, constraints)
- What is the **exact output**? (Format, precision)
- Are there **edge cases**? (Empty input, single element, duplicates)
- What are **performance requirements**?

**Example:**  
Problem: "Sort a list"  
Questions: What type of elements? Can they repeat? Ascending or descending? In-place or extra space allowed?

---

### 3.3 Algorithm Design Techniques (Taxonomy)

```
Algorithm Design Techniques
├── Brute Force
│   └── Try all possibilities (Selection Sort, Bubble Sort)
├── Divide and Conquer
│   └── Split → Solve → Combine (Merge Sort, Quick Sort)
├── Decrease and Conquer
│   └── Reduce by 1 or factor (Binary Search, Insertion Sort)
├── Transform and Conquer
│   └── Reformulate the problem (Heapsort, AVL Trees)
├── Dynamic Programming
│   └── Overlapping subproblems (Fibonacci, Knapsack)
├── Greedy Technique
│   └── Locally optimal choices (Dijkstra, Huffman)
├── Backtracking
│   └── Systematic trial and error (N-Queens, Sudoku)
└── Branch and Bound
    └── Optimized backtracking (TSP, 0/1 Knapsack)
```

---

### 3.4 Correctness of Algorithms

**Proving correctness** uses:

1. **Loop Invariant** — A condition that holds before, during, and after every loop iteration.

   For FindMax:
   - **Invariant:** After `i` iterations, `max` holds the maximum of `A[0..i]`
   - **Base case:** Before loop, `max = A[0]` ✓
   - **Inductive step:** If true for `i`, then after checking `A[i+1]`, still true ✓
   - **Termination:** Loop ends at `i = n-1`, so `max = maximum of A[0..n-1]` ✓

2. **Mathematical Induction** — Prove for base case, assume for n=k, prove for n=k+1.

---

## 4. Analysis Framework

### 4.1 Why Analyze Algorithms?

Given multiple correct algorithms for the same problem, we need a way to choose the **best one**. Analysis gives us:
- Predicted running time before coding
- Language-independent, hardware-independent comparison
- Scalability behavior as input grows

---

### 4.2 Time vs Space Complexity

| Metric | Definition | Unit |
|--------|-----------|------|
| **Time Complexity** | Number of basic operations as a function of input size | Operations (not seconds) |
| **Space Complexity** | Amount of memory used as a function of input size | Memory units |

> **Basic Operation:** The most frequently executed operation that contributes most to total runtime (comparisons, assignments, arithmetic operations).

---

### 4.3 Input Size

The measure of input size depends on the problem:

| Problem | Input Size Measure |
|---------|--------------------|
| Sorting `n` elements | `n` = number of elements |
| Searching in array | `n` = array length |
| Matrix multiplication (n×n) | `n` = matrix dimension |
| Graph algorithms | `V` = vertices, `E` = edges |
| Polynomial evaluation | `n` = degree of polynomial |
| Integer multiplication | `n` = number of bits |

---

### 4.4 Types of Efficiency

For the same algorithm and same input size `n`, different inputs can give different running times:

```
Best Case  ≤  Average Case  ≤  Worst Case
  Ω(n)           Θ(n)            O(n)
```

**Example — Sequential Search in array of n elements:**

| Case | When | Comparisons |
|------|------|-------------|
| **Best Case** | Key found at first position | 1 |
| **Worst Case** | Key not found or at last position | n |
| **Average Case** | Key at position i (uniform distribution) | (n+1)/2 |

**Average Case Derivation:**
```
If key is present with probability p:
  C_avg = [1 + 2 + ... + n]/n × p + n × (1-p)
        = p(n+1)/2 + n(1-p)

If p = 1 (key always present):
  C_avg = (n+1)/2 ≈ n/2
```

---

### 4.5 Empirical Analysis vs Mathematical Analysis

| Aspect | Empirical | Mathematical |
|--------|-----------|-------------|
| Requires coding | Yes | No |
| Hardware dependent | Yes | No |
| Input specific | Yes | General |
| Precision | Approximate | Exact |
| Best for | Validation | Design |

---

## 5. Asymptotic Notations and Basic Efficiency Classes

### 5.1 Why Asymptotic Notation?

We care about behavior as `n → ∞`. Constants and lower-order terms become insignificant. Asymptotic notation captures **growth rate**.

**Example:**
```
T(n) = 5n² + 3n + 100

For n = 1000:
  5n²  = 5,000,000   (99.99% of total)
  3n   = 3,000
  100  = 100
```
Only `n²` matters — we say T(n) = O(n²).

---

### 5.2 Big-O Notation — O(g(n)) [Upper Bound]

**Definition:**  
`T(n) = O(g(n))` if there exist positive constants `c` and `n₀` such that:
```
T(n) ≤ c · g(n)    for all n ≥ n₀
```

**Meaning:** `g(n)` is an **upper bound** on `T(n)`. The algorithm runs **no slower than** `g(n)` in the worst case.

**Example — Prove 2n² + 5n = O(n²):**
```
We need: 2n² + 5n ≤ c·n²  for all n ≥ n₀

2n² + 5n ≤ 2n² + 5n²   (since n ≤ n² for n ≥ 1)
         = 7n²

So: c = 7, n₀ = 1 works.
∴ 2n² + 5n = O(n²)  ✓
```

**Graph visualization:**
```
T(n)
  |        c·g(n)
  |       /
  |      / ← c·g(n) stays above T(n)
  |    T(n)
  |   /
  |  /
  | /
  |________________
  0    n₀        n →
```

---

### 5.3 Big-Omega Notation — Ω(g(n)) [Lower Bound]

**Definition:**  
`T(n) = Ω(g(n))` if there exist positive constants `c` and `n₀` such that:
```
T(n) ≥ c · g(n)    for all n ≥ n₀
```

**Meaning:** `g(n)` is a **lower bound** on `T(n)`. The algorithm always takes **at least** this long.

**Example — Prove 2n² + 5n = Ω(n²):**
```
We need: 2n² + 5n ≥ c·n²  for all n ≥ n₀

2n² + 5n ≥ 2n²   (since 5n ≥ 0)

So: c = 2, n₀ = 1 works.
∴ 2n² + 5n = Ω(n²)  ✓
```

---

### 5.4 Big-Theta Notation — Θ(g(n)) [Tight Bound]

**Definition:**  
`T(n) = Θ(g(n))` if there exist positive constants `c₁`, `c₂`, and `n₀` such that:
```
c₁ · g(n) ≤ T(n) ≤ c₂ · g(n)    for all n ≥ n₀
```

**Meaning:** `g(n)` is both an upper and lower bound. The algorithm's time is **exactly** the order of `g(n)`.

**Example — Prove 2n² + 5n = Θ(n²):**
```
Since 2n² + 5n = O(n²)  [proved above, c₂ = 7]
  and 2n² + 5n = Ω(n²)  [proved above, c₁ = 2]
∴ 2n² + 5n = Θ(n²)  ✓

With c₁ = 2, c₂ = 7, n₀ = 1
```

---

### 5.5 Little-o Notation — o(g(n)) [Strict Upper Bound]

**Definition:**  
`T(n) = o(g(n))` if for every positive constant `c`, there exists `n₀` such that:
```
T(n) < c · g(n)    for all n ≥ n₀
```

Equivalently:
```
lim [T(n) / g(n)] = 0    as n → ∞
```

**Example:** `n = o(n²)` because `lim(n/n²) = lim(1/n) = 0`

---

### 5.6 Little-omega Notation — ω(g(n)) [Strict Lower Bound]

```
lim [T(n) / g(n)] = ∞    as n → ∞
```

**Example:** `n² = ω(n)` because `lim(n²/n) = lim(n) = ∞`

---

### 5.7 Summary Table

| Notation | Meaning | Analogy |
|----------|---------|---------|
| `O(g(n))` | T(n) grows **at most** as fast as g(n) | T ≤ g (roughly) |
| `Ω(g(n))` | T(n) grows **at least** as fast as g(n) | T ≥ g (roughly) |
| `Θ(g(n))` | T(n) grows **exactly** as fast as g(n) | T = g (roughly) |
| `o(g(n))` | T(n) grows **strictly slower** than g(n) | T < g (strictly) |
| `ω(g(n))` | T(n) grows **strictly faster** than g(n) | T > g (strictly) |

---

### 5.8 Basic Efficiency Classes (Ordered from Best to Worst)

| Class | Name | Example Algorithm | n=10 | n=100 | n=1000 |
|-------|------|------------------|------|-------|--------|
| `O(1)` | Constant | Array access | 1 | 1 | 1 |
| `O(log n)` | Logarithmic | Binary Search | ~3 | ~7 | ~10 |
| `O(n)` | Linear | Linear Search | 10 | 100 | 1000 |
| `O(n log n)` | Linearithmic | Merge Sort | ~33 | ~664 | ~9966 |
| `O(n²)` | Quadratic | Bubble Sort | 100 | 10,000 | 1,000,000 |
| `O(n³)` | Cubic | Naive Matrix Mult | 1000 | 10⁶ | 10⁹ |
| `O(2ⁿ)` | Exponential | Subset Enumeration | 1024 | 10³⁰ | 10³⁰¹ |
| `O(n!)` | Factorial | Brute Force TSP | 3.6M | 10¹⁵⁷ | 10²⁵⁶⁷ |

**Growth rate hierarchy:**
```
O(1) < O(log n) < O(√n) < O(n) < O(n log n) < O(n²) < O(n³) < O(2ⁿ) < O(n!)
```

---

### 5.9 Properties of Asymptotic Notation

**1. Transitivity:**
```
If f = O(g) and g = O(h), then f = O(h)
```

**2. Reflexivity:**
```
f = O(f)    f = Ω(f)    f = Θ(f)
```

**3. Symmetry (only for Θ):**
```
f = Θ(g) ⟺ g = Θ(f)
```

**4. Sum Rule:**
```
O(f) + O(g) = O(max(f, g))
Example: O(n²) + O(n) = O(n²)
```

**5. Product Rule:**
```
O(f) × O(g) = O(f × g)
Example: O(n) × O(log n) = O(n log n)
```

---

### 5.10 Limit Comparison Method

A cleaner way to determine the order relationship:

```
lim [T(n) / g(n)] as n → ∞ =
  0       → T(n) = o(g(n))     [T grows slower]
  c > 0   → T(n) = Θ(g(n))    [Same order]
  ∞       → T(n) = ω(g(n))    [T grows faster]
```

**Example — Is n log n = O(n²)?**
```
lim [(n log n) / n²]
= lim [log n / n]
= 0    (by L'Hôpital's rule: lim [1/n / 1] = 0)

So n log n = o(n²), hence also O(n²)  ✓
```

---

## 6. Mathematical Analysis of Non-Recursive Algorithms

### 6.1 General Plan

To analyze a **non-recursive** algorithm:

1. Decide on **parameter n** indicating input size
2. Identify the **basic operation** (innermost loop body)
3. Check if operation count depends only on `n` (or also on type of input)
4. Set up a **sum** expressing the number of basic operations
5. Use **summation formulas** to find a closed-form expression

---

### 6.2 Useful Summation Formulas

```
Sum of 1s:          Σ(i=1 to n) 1 = n

Sum of integers:    Σ(i=1 to n) i = n(n+1)/2

Sum of squares:     Σ(i=1 to n) i² = n(n+1)(2n+1)/6

Sum of cubes:       Σ(i=1 to n) i³ = [n(n+1)/2]²

Geometric series:   Σ(i=0 to n) aʳⁱ = a(rⁿ⁺¹ - 1)/(r - 1)  for r ≠ 1

Harmonic series:    Σ(i=1 to n) 1/i ≈ ln(n) + γ  where γ ≈ 0.5772

Log sum:            Σ(i=1 to n) log i = log(n!) ≈ n log n - n log e
```

---

### 6.3 Example 1 — Matrix Multiplication (Non-Recursive)

**Problem:** Multiply two n×n matrices.

**Algorithm:**
```
Algorithm MatMul(A[0..n-1][0..n-1], B[0..n-1][0..n-1])
  for i ← 0 to n-1 do
    for j ← 0 to n-1 do
      C[i][j] ← 0
      for k ← 0 to n-1 do
        C[i][j] ← C[i][j] + A[i][k] × B[k][j]
  return C
```

**Analysis:**
- Basic operation: multiplication `A[i][k] × B[k][j]`
- Count:
```
C(n) = Σ(i=0 to n-1) Σ(j=0 to n-1) Σ(k=0 to n-1) 1
     = Σ(i=0 to n-1) Σ(j=0 to n-1) n
     = Σ(i=0 to n-1) n²
     = n³
```
- **Time Complexity: Θ(n³)**
- **Space Complexity: Θ(n²)** for output matrix

---

### 6.4 Example 2 — Checking Unique Elements

```
Algorithm UniqueElements(A[0..n-1])
  for i ← 0 to n-2 do
    for j ← i+1 to n-1 do
      if A[i] = A[j]
        return false
  return true
```

**Analysis (Worst Case — all elements unique):**
```
C(n) = Σ(i=0 to n-2) Σ(j=i+1 to n-1) 1
     = Σ(i=0 to n-2) (n-1-i)
     = (n-1) + (n-2) + ... + 1
     = n(n-1)/2
     = Θ(n²)
```

**Step-by-step unrolling for n=4:**
```
i=0: j = 1,2,3  → 3 comparisons
i=1: j = 2,3    → 2 comparisons
i=2: j = 3      → 1 comparison
Total = 3+2+1 = 6 = 4×3/2 = n(n-1)/2  ✓
```

---

## 7. Mathematical Analysis of Recursive Algorithms

### 7.1 General Plan

1. Decide on **parameter n** for input size
2. Identify the **basic operation**
3. Write a **recurrence relation** for the count M(n)
4. **Solve** the recurrence (substitution, master theorem, or recursion tree)

---

### 7.2 Setting Up Recurrence Relations

**Template:**
```
M(n) = M(smaller problem) + work done at current level
M(base case) = constant
```

---

### 7.3 Example 1 — Factorial

```cpp
long long factorial(int n) {
    if (n == 0) {
        return 1;              // base case
    }
    return n * factorial(n - 1); // recursive case
}
```

**Recurrence:**
```
M(n) = M(n-1) + 1    [1 multiplication at each level]
M(0) = 0             [no multiplication for base case]
```

**Solving by Backward Substitution:**
```
M(n) = M(n-1) + 1
     = M(n-2) + 1 + 1 = M(n-2) + 2
     = M(n-3) + 3
     ...
     = M(n-k) + k
     
When k = n:
     = M(0) + n
     = 0 + n
     = n

∴ M(n) = n = Θ(n)
```

**Recursive Tree:**
```
factorial(4)
├── factorial(3)
│   ├── factorial(2)
│   │   ├── factorial(1)
│   │   │   └── factorial(0) → 1
│   │   └── returns 1×1 = 1
│   └── returns 2×1 = 2
└── returns 3×2 = 6
returns 4×6 = 24
```

---

### 7.4 Example 2 — Tower of Hanoi

```cpp
#include <iostream>
#include <string>

void hanoi(int n, const std::string& source, const std::string& target, const std::string& auxiliary) {
    if (n == 1) {
        std::cout << "Move disk 1 from " << source << " to " << target << "\n";
        return;
    }
    hanoi(n - 1, source, auxiliary, target);
    std::cout << "Move disk " << n << " from " << source << " to " << target << "\n";
    hanoi(n - 1, auxiliary, target, source);
}
```

**Recurrence:**
```
M(n) = 2·M(n-1) + 1    [2 recursive calls + 1 move]
M(1) = 1
```

**Solving:**
```
M(n) = 2M(n-1) + 1
     = 2[2M(n-2) + 1] + 1 = 4M(n-2) + 2 + 1
     = 2²M(n-2) + 2¹ + 2⁰
     = 2³M(n-3) + 2² + 2¹ + 2⁰
     ...
     = 2^(n-1)·M(1) + 2^(n-2) + ... + 2⁰
     = 2^(n-1) + 2^(n-2) + ... + 1
     = 2ⁿ - 1

∴ M(n) = 2ⁿ - 1 = Θ(2ⁿ)
```

**Recursive Tree for n=3:**
```
                    H(3)
               /          \
           H(2)             H(2)
          /    \           /    \
        H(1)  H(1)      H(1)  H(1)

Level 0: 1 node (work = 1)
Level 1: 2 nodes (work = 2)
Level 2: 4 nodes (work = 4 = 2²)
Total moves = 1 + 2 + 4 = 7 = 2³ - 1  ✓
```

---

### 7.5 Master Theorem

The **Master Theorem** provides a direct formula for recurrences of the form:
```
T(n) = a·T(n/b) + f(n)

where:
  a ≥ 1  (number of subproblems)
  b > 1  (size reduction factor)
  f(n)   (work done outside recursive calls)
```

**Three Cases:**

Let `c_crit = log_b(a)` (critical exponent)

```
Case 1: f(n) = O(n^(c_crit - ε))  for some ε > 0
        [Recursion dominates]
        → T(n) = Θ(n^c_crit)

Case 2: f(n) = Θ(n^c_crit · log^k(n))  for k ≥ 0
        [Both equal]
        → T(n) = Θ(n^c_crit · log^(k+1)(n))

Case 3: f(n) = Ω(n^(c_crit + ε))  for some ε > 0  AND
        a·f(n/b) ≤ c·f(n) for some c < 1 [regularity condition]
        [Work at root dominates]
        → T(n) = Θ(f(n))
```

**Quick Reference:**

| Recurrence | a | b | c_crit | f(n) | Case | Result |
|-----------|---|---|--------|------|------|--------|
| T(n) = 2T(n/2) + n | 2 | 2 | 1 | n¹ | Case 2 | Θ(n log n) |
| T(n) = 2T(n/2) + 1 | 2 | 2 | 1 | n⁰ | Case 1 | Θ(n) |
| T(n) = 4T(n/2) + n | 4 | 2 | 2 | n¹ | Case 1 | Θ(n²) |
| T(n) = 4T(n/2) + n² | 4 | 2 | 2 | n² | Case 2 | Θ(n² log n) |
| T(n) = 4T(n/2) + n³ | 4 | 2 | 2 | n³ | Case 3 | Θ(n³) |

---

### 7.6 Recursion Tree Method

The recursion tree method visualizes work done at each level.

**Steps:**
1. Draw the recursion tree
2. Compute work at each level
3. Count number of levels
4. Sum work across all levels

**Example: T(n) = 2T(n/2) + n**

```
Level 0:    n                          → work = n
           / \
Level 1:  n/2  n/2                     → work = n/2 + n/2 = n
         / \  / \
Level 2: n/4 n/4 n/4 n/4              → work = 4×(n/4) = n
         ...
Level k: 2^k nodes, each n/2^k work  → work = n
         ...
Level log₂n: n leaves, each 1 work   → work = n

Number of levels = log₂n + 1

Total = n × (log₂n + 1) = n log n + n = Θ(n log n)
```

---

# UNIT I — BRUTE FORCE

## 8. Brute Force — Selection Sort

### 8.1 Concept

**Brute Force** strategy: Solve a problem in the most straightforward, obvious way based on problem definition. No clever optimization.

**Selection Sort Idea:**
- Scan the entire unsorted portion to find the **minimum element**
- Swap it with the first element of the unsorted portion
- Repeat for n-1 passes

```
[5, 3, 8, 1, 2]   ← Find min in [5,3,8,1,2]: min=1, swap with 5
[1, 3, 8, 5, 2]   ← Find min in [3,8,5,2]:   min=2, swap with 3
[1, 2, 8, 5, 3]   ← Find min in [8,5,3]:     min=3, swap with 8
[1, 2, 3, 5, 8]   ← Find min in [5,8]:       min=5, no swap needed
[1, 2, 3, 5, 8]   ← Sorted!
```

---

### 8.2 Pseudocode

```
Algorithm SelectionSort(A[0..n-1])
  // Input: Array A of n orderable elements
  // Output: Array A sorted in ascending order
  
  for i ← 0 to n-2 do
    min ← i
    for j ← i+1 to n-1 do
      if A[j] < A[min]
        min ← j
    swap(A[i], A[min])
```

---

### 8.3 C++ Implementation (Vector)

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

void selection_sort(std::vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n - 1; i++) {
        int min_idx = i;
        
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[min_idx]) {
                min_idx = j;
            }
        }
        
        if (min_idx != i) {
            std::swap(arr[i], arr[min_idx]);
        }
    }
}

int main() {
    std::vector<int> arr = {64, 25, 12, 22, 11};
    selection_sort(arr);
    
    std::cout << "Sorted array: ";
    for (int num : arr) {
        std::cout << num << " ";
    }
    // Output: 11 12 22 25 64
    return 0;
}
```

---

### 8.4 C++ Implementation (Raw Array)

```cpp
#include <iostream>
#include <algorithm>

void selection_sort(int arr[], int n) {
    for (int i = 0; i < n - 1; i++) {
        int min_idx = i;
        
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[min_idx]) {
                min_idx = j;
            }
        }
        
        if (min_idx != i) {
            std::swap(arr[i], arr[min_idx]);
        }
    }
}

int main() {
    int arr[] = {64, 25, 12, 22, 11};
    int n = sizeof(arr) / sizeof(arr[0]);
    selection_sort(arr, n);
    
    std::cout << "Sorted array: ";
    for (int i = 0; i < n; i++) {
        std::cout << arr[i] << " ";
    }
    // Output: 11 12 22 25 64
    return 0;
}
```

---

### 8.5 Complexity Analysis

**Counting Comparisons (Basic Operation):**

```
C(n) = Σ(i=0 to n-2) Σ(j=i+1 to n-1) 1

Inner sum: Σ(j=i+1 to n-1) 1 = (n-1) - (i+1) + 1 = n-1-i

Outer sum: Σ(i=0 to n-2) (n-1-i)
         = (n-1) + (n-2) + (n-3) + ... + 1
         = Σ(k=1 to n-1) k
         = (n-1)n/2
         = n(n-1)/2
```

**Result:**
```
C(n) = n(n-1)/2 = (n² - n)/2

Best Case:    Θ(n²)   [even if sorted, still scans everything]
Average Case: Θ(n²)
Worst Case:   Θ(n²)
```

> **Key Property:** Selection sort is **not sensitive** to input order — always Θ(n²).

**Swap Count:**
```
Swaps = n-1  (exactly, at most)
This is the MINIMUM possible swaps to sort — better than Bubble Sort!
```

---

### 8.6 Space Complexity

```
Auxiliary Space: O(1)  — only 'min', 'i', 'j', 'temp' variables
In-place: Yes
Total Space: O(n)  — for the input array
```

---

### 8.7 Properties Summary

| Property | Value |
|----------|-------|
| Time (Best) | Θ(n²) |
| Time (Average) | Θ(n²) |
| Time (Worst) | Θ(n²) |
| Space | O(1) auxiliary |
| In-place | Yes |
| Stable | **No** |
| Adaptive | No (always n² comparisons) |
| Swaps | O(n) — minimum possible |

**Why is Selection Sort NOT stable?**
```
[3a, 3b, 1]  ← 3a and 3b are equal elements (a before b)

Pass 1: min = 1 (at index 2), swap with arr[0]=3a
Result: [1, 3b, 3a]  ← 3b now comes before 3a!
Relative order of equal elements is NOT preserved → Unstable
```

---

### 8.8 When to Use Selection Sort

✅ When **number of swaps must be minimized** (e.g., writing to flash memory — writes are expensive)  
✅ Small arrays (n < 20) where simplicity matters  
✅ When memory is extremely limited  
❌ Large data sets  
❌ When stability is required  

---

## 9. Brute Force — Bubble Sort

### 9.1 Concept

**Bubble Sort Idea:**
- Make repeated passes through the array
- Compare adjacent elements; swap if out of order
- Larger elements **bubble up** to their correct positions
- After each pass, one more element is in its final position

```
Initial: [5, 3, 8, 1, 2]

Pass 1:
  [5,3] → swap → [3,5,8,1,2]
  [5,8] → no swap → [3,5,8,1,2]
  [8,1] → swap → [3,5,1,8,2]
  [8,2] → swap → [3,5,1,2,8]   ← 8 in place

Pass 2:
  [3,5] → no swap → [3,5,1,2,8]
  [5,1] → swap → [3,1,5,2,8]
  [5,2] → swap → [3,1,2,5,8]   ← 5 in place

Pass 3:
  [3,1] → swap → [1,3,2,5,8]
  [3,2] → swap → [1,2,3,5,8]   ← 3 in place

Pass 4:
  [1,2] → no swap → [1,2,3,5,8] ← 2 in place

Sorted: [1, 2, 3, 5, 8]
```

---

### 9.2 Pseudocode

**Basic Version:**
```
Algorithm BubbleSort(A[0..n-1])
  for i ← 0 to n-2 do
    for j ← 0 to n-2-i do
      if A[j+1] < A[j]
        swap(A[j], A[j+1])
```

**Improved Version (with early termination):**
```
Algorithm ImprovedBubbleSort(A[0..n-1])
  for i ← 0 to n-2 do
    swapped ← false
    for j ← 0 to n-2-i do
      if A[j+1] < A[j]
        swap(A[j], A[j+1])
        swapped ← true
    if swapped = false
      break              // Array already sorted!
```

---

### 9.3 C++ Implementation (Vector)

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

void bubble_sort(std::vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - 1 - i; j++) {
            if (arr[j] > arr[j + 1]) {
                std::swap(arr[j], arr[j + 1]);
            }
        }
    }
}

void bubble_sort_optimized(std::vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n - 1; i++) {
        bool swapped = false;
        for (int j = 0; j < n - 1 - i; j++) {
            if (arr[j] > arr[j + 1]) {
                std::swap(arr[j], arr[j + 1]);
                swapped = true;
            }
        }
        if (!swapped) {
            break; // Early exit: array is sorted
        }
    }
}

int main() {
    std::vector<int> arr = {64, 34, 25, 12, 22, 11, 90};
    bubble_sort_optimized(arr);
    for (int num : arr) {
        std::cout << num << " ";
    }
    // Output: 11 12 22 25 34 64 90
    std::cout << "\n";
    return 0;
}
```

---

### 9.4 C++ Implementation (Raw Array)

```cpp
#include <iostream>
#include <algorithm>

void bubble_sort(int arr[], int n) {
    for (int i = 0; i < n - 1; i++) {
        bool swapped = false;
        
        for (int j = 0; j < n - 1 - i; j++) {
            if (arr[j] > arr[j + 1]) {
                std::swap(arr[j], arr[j + 1]);
                swapped = true;
            }
        }
        
        // If no swap in this pass, array is sorted
        if (!swapped) break;
    }
}
```

---

### 9.5 Complexity Analysis

**Counting Comparisons (Worst Case — reverse sorted):**

```
C(n) = Σ(i=0 to n-2) Σ(j=0 to n-2-i) 1

Inner sum: Σ(j=0 to n-2-i) 1 = (n-2-i) - 0 + 1 = n-1-i

Outer sum: Σ(i=0 to n-2) (n-1-i)
         = (n-1) + (n-2) + ... + 1
         = n(n-1)/2
```

**Counting Swaps (Worst Case):**
```
Each comparison that is out-of-order results in a swap.
Worst case: Same as comparisons = n(n-1)/2
```

**All Cases:**
```
Best Case:    Ω(n)    — with optimization (already sorted: 1 pass, 0 swaps)
Average Case: Θ(n²)
Worst Case:   Θ(n²)
```

**Best Case Derivation (Optimized Bubble Sort, already sorted):**
```
Pass 1: n-1 comparisons, 0 swaps, swapped = false → EXIT
Total comparisons = n-1 = O(n)
```

---

### 9.6 Space Complexity

```
Auxiliary Space: O(1)
In-place: Yes
Stable: Yes (equal elements maintain relative order)
```

---

### 9.7 Properties Summary

| Property | Basic Bubble Sort | Optimized Bubble Sort |
|----------|------------------|----------------------|
| Time (Best) | Θ(n²) | Θ(n) |
| Time (Average) | Θ(n²) | Θ(n²) |
| Time (Worst) | Θ(n²) | Θ(n²) |
| Space | O(1) | O(1) |
| Stable | **Yes** | **Yes** |
| Adaptive | No | **Yes** |
| In-place | Yes | Yes |

---

### 9.8 Selection Sort vs Bubble Sort

| Criterion | Selection Sort | Bubble Sort |
|-----------|---------------|-------------|
| Comparisons | Always n(n-1)/2 | n(n-1)/2 worst, n-1 best |
| Swaps | At most n-1 | Up to n(n-1)/2 |
| Stable | No | Yes |
| Adaptive | No | Yes (optimized) |
| Best for | Minimizing swaps | Nearly sorted data |

---

# UNIT II — DIVIDE AND CONQUER

## Divide and Conquer: The Paradigm

### Core Idea

```
DivideAndConquer(Problem of size n):
  if n is small enough:
    Solve directly (base case)
  else:
    DIVIDE: Split into a subproblems of size n/b each
    CONQUER: Recursively solve each subproblem
    COMBINE: Merge solutions of subproblems

T(n) = a·T(n/b) + D(n) + C(n)
       ↑            ↑       ↑
   subproblems  divide   combine
```

**Classic D&C algorithms:**
- Merge Sort: a=2, b=2, combine = O(n) → T(n) = O(n log n)
- Quick Sort: a=2, b varies, combine = O(1)
- Binary Search: a=1, b=2, combine = O(1) → T(n) = O(log n)
- Strassen: a=7, b=2, combine = O(n²) → T(n) = O(n^2.807)

---

## 10. Divide and Conquer — Merge Sort

### 10.1 Concept

**Core Idea:**
1. **Divide** array into two halves
2. **Recursively** sort each half
3. **Merge** the two sorted halves into one sorted array

**Key Insight:** Merging two sorted arrays can be done in O(n) time.

---

### 10.2 Visualization

```
Divide Phase:
[38, 27, 43, 3, 9, 82, 10]
         /            \
[38, 27, 43]       [3, 9, 82, 10]
   /      \            /        \
[38,27]  [43]      [3,9]      [82,10]
 /    \             /  \        /   \
[38] [27]          [3] [9]    [82] [10]

Merge Phase (bottom up):
[27,38]  [43]      [3,9]   [10,82]
   \      /            \    /
 [27,38,43]          [3,9,10,82]
         \            /
    [3, 9, 10, 27, 38, 43, 82]  ← Sorted!
```

---

### 10.3 Pseudocode

```
Algorithm MergeSort(A[0..n-1])
  if n > 1
    mid ← ⌊n/2⌋
    B[0..mid-1] ← A[0..mid-1]
    C[0..n-mid-1] ← A[mid..n-1]
    MergeSort(B)
    MergeSort(C)
    Merge(B, C, A)

Algorithm Merge(B[0..p-1], C[0..q-1], A[0..p+q-1])
  i ← 0; j ← 0; k ← 0
  while i < p and j < q do
    if B[i] ≤ C[j]
      A[k] ← B[i]; i ← i+1
    else
      A[k] ← C[j]; j ← j+1
    k ← k+1
  
  // Copy remaining elements
  while i < p do
    A[k] ← B[i]; i ← i+1; k ← k+1
  while j < q do
    A[k] ← C[j]; j ← j+1; k ← k+1
```

---

### 10.4 C++ Implementation (Vector)

```cpp
#include <iostream>
#include <vector>

std::vector<int> merge(const std::vector<int>& left, const std::vector<int>& right) {
    std::vector<int> result;
    size_t i = 0, j = 0;
    
    while (i < left.size() && j < right.size()) {
        if (left[i] <= right[j]) {
            result.push_back(left[i++]);
        } else {
            result.push_back(right[j++]);
        }
    }
    
    while (i < left.size()) result.push_back(left[i++]);
    while (j < right.size()) result.push_back(right[j++]);
    
    return result;
}

std::vector<int> merge_sort(std::vector<int> arr) {
    if (arr.size() <= 1) {
        return arr;
    }
    
    size_t mid = arr.size() / 2;
    std::vector<int> left(arr.begin(), arr.begin() + mid);
    std::vector<int> right(arr.begin() + mid, arr.end());
    
    left = merge_sort(left);
    right = merge_sort(right);
    
    return merge(left, right);
}

// In-place version (more efficient)
void merge_inplace(std::vector<int>& arr, int left, int mid, int right) {
    std::vector<int> left_part(arr.begin() + left, arr.begin() + mid + 1);
    std::vector<int> right_part(arr.begin() + mid + 1, arr.begin() + right + 1);
    
    size_t i = 0, j = 0;
    int k = left;
    
    while (i < left_part.size() && j < right_part.size()) {
        if (left_part[i] <= right_part[j]) {
            arr[k++] = left_part[i++];
        } else {
            arr[k++] = right_part[j++];
        }
    }
    
    while (i < left_part.size()) arr[k++] = left_part[i++];
    while (j < right_part.size()) arr[k++] = right_part[j++];
}

void merge_sort_inplace(std::vector<int>& arr, int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;
        merge_sort_inplace(arr, left, mid);
        merge_sort_inplace(arr, mid + 1, right);
        merge_inplace(arr, left, mid, right);
    }
}

int main() {
    std::vector<int> arr = {38, 27, 43, 3, 9, 82, 10};
    arr = merge_sort(arr);
    for (int num : arr) {
        std::cout << num << " ";
    }
    // Output: 3 9 10 27 38 43 82
    std::cout << "\n";
    return 0;
}
```

---

### 10.5 C++ Implementation (Raw Array)

```cpp
#include <iostream>

void merge(int arr[], int left, int mid, int right) {
    int n1 = mid - left + 1;
    int n2 = right - mid;
    
    // Create temporary arrays
    int* L = new int[n1];
    int* R = new int[n2];
    
    // Copy data
    for (int i = 0; i < n1; i++) L[i] = arr[left + i];
    for (int j = 0; j < n2; j++) R[j] = arr[mid + 1 + j];
    
    // Merge
    int i = 0, j = 0, k = left;
    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) arr[k++] = L[i++];
        else               arr[k++] = R[j++];
    }
    while (i < n1) arr[k++] = L[i++];
    while (j < n2) arr[k++] = R[j++];
    
    delete[] L;
    delete[] R;
}

void merge_sort(int arr[], int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;  // Avoids overflow
        merge_sort(arr, left, mid);
        merge_sort(arr, mid + 1, right);
        merge(arr, left, mid, right);
    }
}
```

---

### 10.6 Recurrence Relation

```
T(n) = 2T(n/2) + Θ(n)

Where:
  2       = number of subproblems
  n/2     = size of each subproblem
  Θ(n)    = time to merge two halves

Base case: T(1) = Θ(1)
```

---

### 10.7 Solving the Recurrence — Three Methods

#### Method 1: Recursion Tree

```
Level 0:  n                                     work = n
         / \
Level 1: n/2  n/2                               work = n/2 + n/2 = n
        /\   /\
Level 2: n/4 n/4 n/4 n/4                        work = 4×(n/4) = n
        ...
Level k: 2^k nodes, each n/2^k                  work = n
        ...
Level log n: n nodes, each 1                    work = n

Total levels = log₂n + 1

T(n) = n × (log₂n + 1) = n log n + n = Θ(n log n)
```

**Diagram for n = 8:**
```
            [8]        → 8 work
           /    \
         [4]    [4]    → 4+4=8 work
        / \    / \
      [2][2] [2][2]    → 2+2+2+2=8 work
      /\ /\ /\ /\
     [1][1]...[1][1]   → 8×1=8 work (8 leaves)

Levels: 4 (= log₂8 + 1)
Total work: 8 × 4 = 32 = 8 × log₂8 = n log n  ✓
```

---

#### Method 2: Substitution (Backward Substitution)

```
T(n) = 2T(n/2) + n

Substitute k times:
T(n) = 2T(n/2) + n
     = 2[2T(n/4) + n/2] + n = 4T(n/4) + n + n = 4T(n/4) + 2n
     = 4[2T(n/8) + n/4] + 2n = 8T(n/8) + n + 2n = 8T(n/8) + 3n
     ...
     = 2^k · T(n/2^k) + k·n

When 2^k = n  (i.e., k = log₂n):
     = n · T(1) + n·log₂n
     = n · 1 + n log n
     = n log n + n
     = Θ(n log n)  ✓
```

---

#### Method 3: Master Theorem

```
T(n) = 2T(n/2) + n

a = 2,  b = 2,  f(n) = n

c_crit = log_b(a) = log_2(2) = 1

f(n) = n = n^1 = n^c_crit

→ Case 2 applies (f(n) = Θ(n^c_crit) with k=0)

∴ T(n) = Θ(n^c_crit · log n) = Θ(n log n)  ✓
```

---

### 10.8 Complexity Summary

| Case | Time | Explanation |
|------|------|-------------|
| Best Case | Θ(n log n) | Even for sorted input, still divides and merges |
| Average Case | Θ(n log n) | Same structure regardless |
| Worst Case | Θ(n log n) | Same for all inputs |
| Space | Θ(n) | Temporary arrays for merging |

---

### 10.9 Properties

| Property | Value |
|----------|-------|
| Time (all cases) | Θ(n log n) |
| Space | Θ(n) — NOT in-place |
| Stable | **Yes** |
| Adaptive | No |
| Parallelizable | Yes (divide phase) |
| External Sort | **Yes** — excellent for disk-based sorting |

**Why Merge Sort is stable:**
```
During merge, when left[i] == right[j]:
  We always pick from LEFT first (left[i] ≤ right[j])
  This preserves relative order of equal elements ✓
```

---

## 11. Divide and Conquer — Quick Sort

### 11.1 Concept

**Core Idea:**
1. **Choose a pivot** element
2. **Partition** array: elements ≤ pivot go left, elements > pivot go right
3. **Recursively** sort left and right parts

**Key Insight:** After partitioning, the pivot is in its **exact final position**.

```
[3, 6, 8, 10, 1, 2, 1]  ← pivot = 1 (last element)

Partition:
  [1, 1, | 8, 10, 6, 2, 3]
   ≤ pivot  > pivot

  Actually: [1, 1, 8, 10, 6, 2, 3]
  Pivot (1) is at index 1
  
Left: [1]   Right: [8, 10, 6, 2, 3]
```

---

### 11.2 Lomuto Partition Scheme

```
Algorithm LomutoPartition(A, low, high)
  pivot ← A[high]
  i ← low - 1
  
  for j ← low to high-1 do
    if A[j] ≤ pivot
      i ← i + 1
      swap(A[i], A[j])
  
  swap(A[i+1], A[high])
  return i+1   // Pivot's final position
```

**Step-by-step trace for [10, 80, 30, 90, 40, 50, 70], pivot=70:**
```
pivot = 70
i = -1

j=0: A[0]=10 ≤ 70 → i=0, swap(A[0],A[0]): [10, 80, 30, 90, 40, 50, 70]
j=1: A[1]=80 > 70 → skip
j=2: A[2]=30 ≤ 70 → i=1, swap(A[1],A[2]): [10, 30, 80, 90, 40, 50, 70]
j=3: A[3]=90 > 70 → skip
j=4: A[4]=40 ≤ 70 → i=2, swap(A[2],A[4]): [10, 30, 40, 90, 80, 50, 70]
j=5: A[5]=50 ≤ 70 → i=3, swap(A[3],A[5]): [10, 30, 40, 50, 80, 90, 70]

Final: swap(A[i+1]=A[4], A[high]=A[6]): [10, 30, 40, 50, 70, 90, 80]
                                                          ↑
                                               pivot 70 in final position!
Returns index 4
```

---

### 11.3 Hoare Partition Scheme

```
Algorithm HoarePartition(A, low, high)
  pivot ← A[low]
  i ← low - 1
  j ← high + 1
  
  while true do
    do i ← i + 1 while A[i] < pivot
    do j ← j - 1 while A[j] > pivot
    
    if i ≥ j
      return j
    
    swap(A[i], A[j])
```

---

### 11.4 Pseudocode

```
Algorithm QuickSort(A[0..n-1], low, high)
  if low < high
    pivot_pos ← Partition(A, low, high)
    QuickSort(A, low, pivot_pos - 1)
    QuickSort(A, pivot_pos + 1, high)
```

---

### 11.5 C++ Implementation (Vector)

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <random>

int partition(std::vector<int>& arr, int low, int high) {
    int pivot = arr[high];
    int i = low - 1; // Index of smaller element
    
    for (int j = low; j < high; ++j) {
        if (arr[j] <= pivot) {
            ++i;
            std::swap(arr[i], arr[j]);
        }
    }
    
    // Place pivot in correct position
    std::swap(arr[i + 1], arr[high]);
    return i + 1;
}

void quick_sort(std::vector<int>& arr, int low, int high) {
    if (low < high) {
        int pivot_idx = partition(arr, low, high);
        quick_sort(arr, low, pivot_idx - 1);
        quick_sort(arr, pivot_idx + 1, high);
    }
}

void quick_sort(std::vector<int>& arr) {
    if (!arr.empty()) {
        quick_sort(arr, 0, arr.size() - 1);
    }
}

// Randomized Quick Sort (better average performance)
void randomized_quick_sort(std::vector<int>& arr, int low, int high) {
    if (low < high) {
        // Random pivot selection
        int rand_idx = low + std::rand() % (high - low + 1);
        std::swap(arr[rand_idx], arr[high]);
        
        int pivot_idx = partition(arr, low, high);
        randomized_quick_sort(arr, low, pivot_idx - 1);
        randomized_quick_sort(arr, pivot_idx + 1, high);
    }
}

int main() {
    std::vector<int> arr = {10, 7, 8, 9, 1, 5};
    quick_sort(arr);
    for (int num : arr) {
        std::cout << num << " ";
    }
    // Output: 1 5 7 8 9 10
    std::cout << "\n";
    return 0;
}
```

---

### 11.6 C++ Implementation (Raw Array)

```cpp
#include <iostream>
#include <algorithm>

int partition(int arr[], int low, int high) {
    int pivot = arr[high];
    int i = low - 1;
    
    for (int j = low; j < high; j++) {
        if (arr[j] <= pivot) {
            i++;
            std::swap(arr[i], arr[j]);
        }
    }
    std::swap(arr[i + 1], arr[high]);
    return i + 1;
}

void quick_sort(int arr[], int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        quick_sort(arr, low, pi - 1);
        quick_sort(arr, pi + 1, high);
    }
}
```

---

### 11.7 Recurrence Relation

**Best Case / Average Case:**
```
Pivot always divides array roughly in half:

T(n) = 2T(n/2) + Θ(n)

By Master Theorem (same as Merge Sort):
T(n) = Θ(n log n)
```

**Worst Case:**
```
Pivot is always smallest or largest element (sorted/reverse sorted array):

T(n) = T(n-1) + T(0) + Θ(n)
     = T(n-1) + Θ(n)

Solving:
T(n) = T(n-1) + cn
     = T(n-2) + c(n-1) + cn
     = T(n-3) + c(n-2) + c(n-1) + cn
     ...
     = c[n + (n-1) + (n-2) + ... + 1]
     = c · n(n+1)/2
     = Θ(n²)
```

---

### 11.8 Recursion Trees

**Best Case Tree (n=8, balanced splits):**
```
Level 0:              [8]               → work: n
Level 1:        [4]       [4]           → work: n
Level 2:     [2][2]   [2][2]            → work: n
Level 3:   [1][1][1][1][1][1][1][1]    → work: n (base cases)

Depth = log₂n = 3
Total work = n × log n = Θ(n log n)
```

**Worst Case Tree (sorted array, pivot = last element):**
```
Level 0:    [n]
Level 1:      [n-1]
Level 2:        [n-2]
Level 3:          ...
Level n-1:          [1]

This is a SKEWED (unbalanced) tree!
Depth = n
Total work = n + (n-1) + (n-2) + ... + 1 = n(n+1)/2 = Θ(n²)
```

---

### 11.9 Average Case Analysis

Assume pivot is equally likely to be the k-th smallest element (k=1 to n):

```
T(n) = (1/n) Σ(k=1 to n) [T(k-1) + T(n-k)] + Θ(n)

By symmetry:
T(n) = (2/n) Σ(k=0 to n-1) T(k) + Θ(n)

Solving this recurrence (multiply both sides by n):
n·T(n) = 2 Σ(k=0 to n-1) T(k) + cn²

Using substitution, the solution is:
T(n) = Θ(n log n)

More precisely: T(n) ≈ 1.39 n log₂n
(About 39% more comparisons than best case, still Θ(n log n))
```

---

### 11.10 Complexity Summary

| Case | Time | When |
|------|------|------|
| Best Case | Θ(n log n) | Pivot always gives balanced split |
| Average Case | Θ(n log n) | Random input |
| Worst Case | Θ(n²) | Sorted/reverse sorted, pivot = first/last |
| Space (avg) | Θ(log n) | Recursion stack |
| Space (worst) | Θ(n) | Skewed recursion |

---

### 11.11 Pivot Selection Strategies

| Strategy | Advantage | Disadvantage |
|----------|-----------|-------------|
| **First/Last element** | Simple | O(n²) on sorted input |
| **Middle element** | Better than first/last | Still has bad cases |
| **Random element** | Expected O(n log n), hard to trigger worst | Requires RNG |
| **Median-of-3** | Avoids sorted-array worst case | Slight overhead |
| **Median of medians** | Guaranteed O(n log n) | Too slow in practice |

**Median-of-3 (used in practice):**
```cpp
int median_of_three(std::vector<int>& arr, int low, int high) {
    int mid = low + (high - low) / 2;
    // Sort arr[low], arr[mid], arr[high]
    if (arr[low] > arr[mid]) std::swap(arr[low], arr[mid]);
    if (arr[low] > arr[high]) std::swap(arr[low], arr[high]);
    if (arr[mid] > arr[high]) std::swap(arr[mid], arr[high]);
    
    // arr[mid] is now the median — use as pivot
    std::swap(arr[mid], arr[high - 1]);
    return arr[high - 1]; // Return pivot value
}
```

---

### 11.12 Properties Summary

| Property | Quick Sort | Merge Sort |
|----------|------------|------------|
| Average Time | Θ(n log n) | Θ(n log n) |
| Worst Time | Θ(n²) | Θ(n log n) |
| Space | O(log n) avg | O(n) |
| Stable | **No** | Yes |
| In-place | **Yes** | No |
| Cache performance | **Better** | Worse |
| Practical speed | **Fastest in practice** | Slower than QS avg |

---

## 12. Multiplication of Long Integers

### 12.1 The Problem

Standard multiplication of two n-digit integers uses O(n²) single-digit multiplications. Can we do better?

**Example:**  
Multiply: `1234 × 5678`

Naive approach:
```
    1234
  × 5678
  ------
    9872   (1234 × 8)
   8638    (1234 × 7, shifted 1)
  7404     (1234 × 6, shifted 2)
 6170      (1234 × 5, shifted 3)
---------
 7006652
```
This uses 4 × 4 = 16 single-digit multiplications = O(n²)

---

### 12.2 Divide and Conquer Approach

**Split** each n-digit number at the midpoint:

```
Let n = number of digits, m = n/2

A = a₁ × 10^m + a₀   (A split into high and low halves)
B = b₁ × 10^m + b₀

A × B = (a₁ × 10^m + a₀)(b₁ × 10^m + b₀)
      = a₁b₁ × 10^(2m) + (a₁b₀ + a₀b₁) × 10^m + a₀b₀
```

**Example: A = 1234, B = 5678, m = 2**
```
a₁ = 12,  a₀ = 34
b₁ = 56,  b₀ = 78

A × B = 12×56 × 10⁴ + (12×78 + 34×56) × 10² + 34×78
      = 672 × 10000 + (936 + 1904) × 100 + 2652
      = 6720000 + 284000 + 2652
      = 7006652  ✓
```

This still requires **4 multiplications**: a₁b₁, a₁b₀, a₀b₁, a₀b₀

**Recurrence:** T(n) = 4T(n/2) + Θ(n) → Θ(n²) — no improvement!

---

### 12.3 Karatsuba Algorithm

**Key Trick:** Compute (a₁b₀ + a₀b₁) using only **3 multiplications** instead of 4!

```
Compute:
  p = a₁ × b₁
  q = a₀ × b₀
  r = (a₁ + a₀) × (b₁ + b₀)

Then: a₁b₀ + a₀b₁ = r - p - q

A × B = p × 10^(2m) + (r - p - q) × 10^m + q
```

**Proof:**
```
r = (a₁ + a₀)(b₁ + b₀)
  = a₁b₁ + a₁b₀ + a₀b₁ + a₀b₀
  = p + (a₁b₀ + a₀b₁) + q

∴ a₁b₀ + a₀b₁ = r - p - q  ✓
```

**Example: 1234 × 5678**
```
a₁=12, a₀=34, b₁=56, b₀=78, m=2

p = 12 × 56 = 672
q = 34 × 78 = 2652
r = (12+34) × (56+78) = 46 × 134 = 6164

Middle = r - p - q = 6164 - 672 - 2652 = 2840

Result = 672 × 10⁴ + 2840 × 10² + 2652
       = 6720000 + 284000 + 2652
       = 7006652  ✓

Only 3 multiplications instead of 4!
```

---

### 12.4 C++ Implementation

```cpp
#include <iostream>
#include <string>
#include <algorithm>
#include <cmath>

long long karatsuba(long long x, long long y) {
    // Base case: single digit
    if (x < 10 || y < 10) {
        return x * y;
    }
    
    // Determine size of numbers
    std::string str_x = std::to_string(x);
    std::string str_y = std::to_string(y);
    int n = std::max(str_x.length(), str_y.length());
    int m = n / 2;
    
    // Split x and y
    // x = a1 * 10^m + a0
    // y = b1 * 10^m + b0
    long long power = std::pow(10, m);
    long long a1 = x / power;
    long long a0 = x % power;
    long long b1 = y / power;
    long long b0 = y % power;
    
    // 3 recursive multiplications
    long long p = karatsuba(a1, b1);            // a1 * b1
    long long q = karatsuba(a0, b0);            // a0 * b0
    long long r = karatsuba(a1 + a0, b1 + b0);  // (a1+a0)(b1+b0)
    
    // Combine: A*B = p*10^(2m) + (r-p-q)*10^m + q
    return p * std::pow(10, 2 * m) + (r - p - q) * power + q;
}

int main() {
    std::cout << karatsuba(1234, 5678) << "\n";       // 7006652
    std::cout << karatsuba(12345678, 87654321) << "\n"; // 1082152022374638
    return 0;
}
```

---

### 12.5 Recurrence and Complexity

```
Karatsuba Recurrence:

T(n) = 3T(n/2) + Θ(n)

Where:
  3   = three recursive subproblems
  n/2 = each subproblem is half size
  Θ(n) = addition/shifting operations

Using Master Theorem:
  a = 3,  b = 2,  f(n) = n

  c_crit = log₂(3) ≈ 1.585

  f(n) = n = n¹  vs  n^c_crit = n^1.585

  n = O(n^(1.585 - ε))  [Case 1 applies]

∴ T(n) = Θ(n^log₂3) = Θ(n^1.585)
```

**Comparison:**

| Method | Multiplications | Time Complexity |
|--------|----------------|----------------|
| Naive (grade school) | n² | Θ(n²) |
| D&C (4 recursive) | n² | Θ(n²) |
| Karatsuba | n^1.585 | Θ(n^1.585) |
| Toom-Cook-3 | — | Θ(n^1.465) |
| Schönhage-Strassen | — | Θ(n log n log log n) |
| Harvey-Hoeven (2019) | — | Θ(n log n) |

---

### 12.6 Recursion Tree for Karatsuba

```
Level 0:  1 problem of size n           → work: n
         /    |    \
Level 1: 3 problems of size n/2         → work: 3(n/2) = 1.5n
       /|\  /|\  /|\
Level 2: 9 problems of size n/4         → work: 9(n/4) = 2.25n
       ...
Level k: 3^k problems of size n/2^k    → work: 3^k × n/2^k = n(3/2)^k

Stops when: n/2^k = 1 → k = log₂n

Total = n × Σ(k=0 to log₂n) (3/2)^k
      = n × [(3/2)^(log₂n+1) - 1] / [(3/2) - 1]
      = O(n × (3/2)^log₂n)
      = O(n × n^(log₂(3/2)))
      = O(n^(1 + log₂(3) - 1))
      = O(n^log₂3)
      = O(n^1.585)
```

---

## 13. Strassen's Matrix Multiplication

### 13.1 The Problem

**Standard Matrix Multiplication** of two n×n matrices requires:
- n³ multiplications
- n²(n-1) additions
- Total: Θ(n³)

For n=1000: **1 billion multiplications!** Can we do better?

---

### 13.2 D&C Matrix Multiplication (Naive)

**Idea:** Split each n×n matrix into four (n/2)×(n/2) submatrices:

```
A = | A11  A12 |    B = | B11  B12 |
    | A21  A22 |        | B21  B22 |

C = A × B

C11 = A11×B11 + A12×B21
C12 = A11×B12 + A12×B22
C21 = A21×B11 + A22×B21
C22 = A21×B12 + A22×B22
```

This requires **8 recursive multiplications** and **4 additions**.

**Recurrence:**
```
T(n) = 8T(n/2) + Θ(n²)

By Master Theorem:
  a=8, b=2, c_crit = log₂8 = 3
  f(n) = n² < n^3 → Case 1
  T(n) = Θ(n³)   — Same as naive!
```

---

### 13.3 Strassen's Algorithm

**Key Insight:** Use only **7 multiplications** instead of 8 by clever linear combinations.

**Strassen's 7 products:**

```
M1 = (A11 + A22) × (B11 + B22)
M2 = (A21 + A22) × B11
M3 = A11 × (B12 - B22)
M4 = A22 × (B21 - B11)
M5 = (A11 + A12) × B22
M6 = (A21 - A11) × (B11 + B12)
M7 = (A12 - A22) × (B21 + B22)
```

**Computing C from M1...M7:**

```
C11 = M1 + M4 - M5 + M7
C12 = M3 + M5
C21 = M2 + M4
C22 = M1 - M2 + M3 + M6
```

---

### 13.4 Verification

Let's verify C11 = A11×B11 + A12×B21:

```
C11 = M1 + M4 - M5 + M7

M1 = (A11+A22)(B11+B22) = A11B11 + A11B22 + A22B11 + A22B22
M4 = A22(B21-B11)       = A22B21 - A22B11
M5 = (A11+A12)B22       = A11B22 + A12B22
M7 = (A12-A22)(B21+B22) = A12B21 + A12B22 - A22B21 - A22B22

C11 = M1 + M4 - M5 + M7
    = (A11B11 + A11B22 + A22B11 + A22B22)
    + (A22B21 - A22B11)
    - (A11B22 + A12B22)
    + (A12B21 + A12B22 - A22B21 - A22B22)

    Cancel A11B22:  +A11B22 - A11B22 = 0
    Cancel A22B11:  +A22B11 - A22B11 = 0
    Cancel A22B22:  +A22B22 - A22B22 = 0
    Cancel A12B22:  -A12B22 + A12B22 = 0
    Cancel A22B21:  +A22B21 - A22B21 = 0

    = A11B11 + A12B21  ✓
```

---

### 13.5 C++ Implementation (Vector)

```cpp
#include <iostream>
#include <vector>

using Matrix = std::vector<std::vector<int>>;

Matrix add(const Matrix& A, const Matrix& B) {
    int n = A.size();
    Matrix C(n, std::vector<int>(n));
    for (int i = 0; i < n; ++i)
        for (int j = 0; j < n; ++j)
            C[i][j] = A[i][j] + B[i][j];
    return C;
}

Matrix sub(const Matrix& A, const Matrix& B) {
    int n = A.size();
    Matrix C(n, std::vector<int>(n));
    for (int i = 0; i < n; ++i)
        for (int j = 0; j < n; ++j)
            C[i][j] = A[i][j] - B[i][j];
    return C;
}

Matrix strassen(const Matrix& A, const Matrix& B) {
    int n = A.size();
    if (n == 1) {
        return {{A[0][0] * B[0][0]}};
    }
    
    int mid = n / 2;
    Matrix A11(mid, std::vector<int>(mid)), A12(mid, std::vector<int>(mid));
    Matrix A21(mid, std::vector<int>(mid)), A22(mid, std::vector<int>(mid));
    Matrix B11(mid, std::vector<int>(mid)), B12(mid, std::vector<int>(mid));
    Matrix B21(mid, std::vector<int>(mid)), B22(mid, std::vector<int>(mid));
    
    for (int i = 0; i < mid; i++) {
        for (int j = 0; j < mid; j++) {
            A11[i][j] = A[i][j];
            A12[i][j] = A[i][j + mid];
            A21[i][j] = A[i + mid][j];
            A22[i][j] = A[i + mid][j + mid];
            
            B11[i][j] = B[i][j];
            B12[i][j] = B[i][j + mid];
            B21[i][j] = B[i + mid][j];
            B22[i][j] = B[i + mid][j + mid];
        }
    }
    
    Matrix M1 = strassen(add(A11, A22), add(B11, B22));
    Matrix M2 = strassen(add(A21, A22), B11);
    Matrix M3 = strassen(A11, sub(B12, B22));
    Matrix M4 = strassen(A22, sub(B21, B11));
    Matrix M5 = strassen(add(A11, A12), B22);
    Matrix M6 = strassen(sub(A21, A11), add(B11, B12));
    Matrix M7 = strassen(sub(A12, A22), add(B21, B22));
    
    Matrix C11 = add(sub(add(M1, M4), M5), M7);
    Matrix C12 = add(M3, M5);
    Matrix C21 = add(M2, M4);
    Matrix C22 = add(sub(add(M1, M3), M2), M6);
    
    Matrix C(n, std::vector<int>(n));
    for (int i = 0; i < mid; i++) {
        for (int j = 0; j < mid; j++) {
            C[i][j] = C11[i][j];
            C[i][j + mid] = C12[i][j];
            C[i + mid][j] = C21[i][j];
            C[i + mid][j + mid] = C22[i][j];
        }
    }
    
    return C;
}

int main() {
    Matrix A = {{1, 2}, {3, 4}};
    Matrix B = {{5, 6}, {7, 8}};
    Matrix C = strassen(A, B);
    
    for (const auto& row : C) {
        for (int val : row) std::cout << val << " ";
        std::cout << "\n";
    }
    // Output: 
    // 19 22
    // 43 50
    return 0;
}
```

---

### 13.6 C++ Implementation (Raw Array Base Case)

```cpp
#include <iostream>

const int N = 4;  // Matrix size (must be power of 2)

// Matrix addition: C = A + B
void mat_add(int A[][N/2], int B[][N/2], int C[][N/2], int n) {
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            C[i][j] = A[i][j] + B[i][j];
}

// Matrix subtraction: C = A - B
void mat_sub(int A[][N/2], int B[][N/2], int C[][N/2], int n) {
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            C[i][j] = A[i][j] - B[i][j];
}
```

---

### 13.7 Recurrence Relation

```
T(n) = 7T(n/2) + Θ(n²)

Where:
  7   = seven recursive matrix multiplications
  n/2 = each submatrix is n/2 × n/2
  Θ(n²) = additions/subtractions of n/2 × n/2 matrices
           (18 additions/subtractions × (n/2)² = 18n²/4 = Θ(n²))
```

---

### 13.8 Solving the Recurrence

#### Method 1: Master Theorem

```
T(n) = 7T(n/2) + Θ(n²)

a = 7,  b = 2,  f(n) = n²

c_crit = log₂7 ≈ 2.807

f(n) = n² = n^2

Since 2 < 2.807:
  f(n) = O(n^(2.807 - ε))  for ε = 0.807

→ Case 1 applies

∴ T(n) = Θ(n^log₂7) = Θ(n^2.807)
```

#### Method 2: Recursion Tree

```
Level 0: 1 problem of size n           → Θ(n²) work
         7 subproblems of size n/2
Level 1: 7 problems of size n/2         → 7(n/2)² = 7n²/4 work
         7² subproblems of size n/4
Level 2: 49 problems of size n/4        → 49(n/4)² = 49n²/16 work
         ...
Level k: 7^k problems of size n/2^k    → 7^k(n/2^k)² = n²(7/4)^k work

Ratio between levels = 7/4 > 1 (geometric series grows!)

Total = n² × Σ(k=0 to log₂n) (7/4)^k
      ≈ n² × (7/4)^log₂n / (7/4 - 1)
      = O(n² × n^log₂(7/4))
      = O(n^(2 + log₂7 - 2))
      = O(n^log₂7)
      = O(n^2.807)
```

---

### 13.9 Complexity Comparison

| Method | Multiplications | Additions | Time Complexity |
|--------|----------------|-----------|----------------|
| Naive | n³ | n²(n-1) | Θ(n³) |
| D&C (8 subproblems) | n^log₂8 = n³ | — | Θ(n³) |
| **Strassen** | **n^log₂7 ≈ n^2.807** | — | **Θ(n^2.807)** |
| Coppersmith-Winograd | — | — | Θ(n^2.376) |
| Best known (2023) | — | — | Θ(n^2.371) |

**Practical comparison for n=1024:**
```
Naive:     1024³ = 1,073,741,824 ≈ 10⁹ multiplications
Strassen:  1024^2.807 ≈ 843,432,765 — roughly 20% fewer
```

---

### 13.10 Strassen vs Standard — When to Use?

| Aspect | Standard | Strassen |
|--------|----------|---------|
| Code complexity | Simple | Complex |
| Numerical stability | Better | Slightly worse |
| Overhead | Low | High constants |
| Break-even point | — | n ≈ 32-64 |
| Practical for | Small/medium n | Large n (>1000) |

**Implementation note:**  
In practice, use **standard multiplication** for submatrices below a threshold (~32×32), then switch to Strassen recursively. This hybrid approach is used in BLAS libraries.

---

### 13.11 Properties

| Property | Value |
|----------|-------|
| Time | Θ(n^2.807) |
| Space | Θ(n² log n) — due to recursive submatrix storage |
| Numerical stability | Slightly worse than standard |
| Best for | Large dense matrices (n > 100) |

---

# SUMMARY TABLES

## Algorithm Complexity Quick Reference

| Algorithm | Best | Average | Worst | Space | Stable | In-place |
|-----------|------|---------|-------|-------|--------|----------|
| Selection Sort | Θ(n²) | Θ(n²) | Θ(n²) | O(1) | No | Yes |
| Bubble Sort | Θ(n) | Θ(n²) | Θ(n²) | O(1) | Yes | Yes |
| Merge Sort | Θ(n log n) | Θ(n log n) | Θ(n log n) | Θ(n) | Yes | No |
| Quick Sort | Θ(n log n) | Θ(n log n) | Θ(n²) | O(log n) | No | Yes |
| Strassen | — | — | Θ(n^2.807) | Θ(n²log n) | — | No |
| Karatsuba | — | — | Θ(n^1.585) | O(n) | — | No |

---

## Recurrence Relations Summary

| Algorithm | Recurrence | Solution |
|-----------|-----------|---------|
| Merge Sort | T(n) = 2T(n/2) + n | Θ(n log n) |
| Quick Sort (avg) | T(n) = 2T(n/2) + n | Θ(n log n) |
| Quick Sort (worst) | T(n) = T(n-1) + n | Θ(n²) |
| Karatsuba | T(n) = 3T(n/2) + n | Θ(n^1.585) |
| Strassen | T(n) = 7T(n/2) + n² | Θ(n^2.807) |
| Binary Search | T(n) = T(n/2) + 1 | Θ(log n) |
| Tower of Hanoi | T(n) = 2T(n-1) + 1 | Θ(2ⁿ) |
| Factorial | T(n) = T(n-1) + 1 | Θ(n) |

---

## Master Theorem Summary

```
T(n) = aT(n/b) + f(n)
c_crit = log_b(a)

┌─────────────────────────────────────────────────────┐
│ Case 1: f(n) = O(n^(c-ε))  → T(n) = Θ(n^c_crit)   │
│ Case 2: f(n) = Θ(n^c)      → T(n) = Θ(n^c log n)  │
│ Case 3: f(n) = Ω(n^(c+ε))  → T(n) = Θ(f(n))       │
└─────────────────────────────────────────────────────┘
```

---

## Divide and Conquer Template

```cpp
Solution divide_and_conquer(Problem problem, int size) {
    // BASE CASE
    if (size <= THRESHOLD) {
        return solve_directly(problem);
    }
    
    // DIVIDE
    std::vector<Problem> subproblems = divide(problem, size);
    
    // CONQUER (recursive calls)
    std::vector<Solution> sub_solutions;
    for (const auto& sub : subproblems) {
        sub_solutions.push_back(divide_and_conquer(sub, size / b));
    }
    
    // COMBINE
    return combine(sub_solutions);
}
```

---

*End of Notes — Design and Analysis of Algorithms (Units I & II)*

*Topics covered: Algorithm fundamentals, Asymptotic notation, Complexity analysis, Brute Force (Selection Sort, Bubble Sort), Divide & Conquer (Merge Sort, Quick Sort, Karatsuba, Strassen)*
