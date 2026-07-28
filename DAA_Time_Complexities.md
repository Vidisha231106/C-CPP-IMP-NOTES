# DAA — Time Complexity Reference Sheet

Notation: n = input size, V = vertices, E = edges, k = bits/keys, W = knapsack capacity, b = branching factor

---

## Unit I — Brute Force

| Algorithm | Best Case | Average Case | Worst Case | Space | Recurrence Relation |
|---|---|---|---|---|---|
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) | N/A (iterative) |
| Bubble Sort | O(n) *(with swap-flag optimization)* | O(n²) | O(n²) | O(1) | N/A (iterative) |

---

## Unit II — Divide & Conquer

| Algorithm | Best Case | Average Case | Worst Case | Space | Recurrence Relation |
|---|---|---|---|---|---|
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) | T(n) = 2T(n/2) + O(n) |
| Quicksort | O(n log n) | O(n log n) | O(n²) | O(log n) avg / O(n) worst | Best/Avg: T(n) = 2T(n/2) + O(n) • Worst: T(n) = T(n−1) + O(n) |
| Multiplication of Long Integers (D&C) | O(n^1.585) | O(n^1.585) | O(n^1.585) | O(n) | T(n) = 3T(n/2) + O(n) |
| Strassen's Matrix Multiplication | O(n^2.807) | O(n^2.807) | O(n^2.807) | O(n²) | T(n) = 7T(n/2) + O(n²) |

## Unit II — Decrease & Conquer

| Algorithm | Best Case | Average Case | Worst Case | Space | Recurrence Relation |
|---|---|---|---|---|---|
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) | T(n) = T(n−1) + O(n) (worst) • T(n) = T(n−1) + O(1) (best) |
| DFS (Depth First Search) | O(V + E) | O(V + E) | O(V + E) | O(V) | T(n) = T(n−1) + O(b) (decrease by 1 node per call) |
| BFS (Breadth First Search) | O(V + E) | O(V + E) | O(V + E) | O(V) | N/A (iterative, queue-based) |
| Topological Sorting | O(V + E) | O(V + E) | O(V + E) | O(V) | N/A (DFS-based, uses DFS recurrence) |

---

## Unit III — Transform & Conquer

| Algorithm | Best Case | Average Case | Worst Case | Space | Recurrence Relation |
|---|---|---|---|---|---|
| Presorting (as enabler, e.g. for search/dedup) | O(n log n) | O(n log n) | O(n log n) | O(n) / O(1) in-place | T(n) = 2T(n/2) + O(n) (if merge-sort based) |
| Heapsort | O(n log n) | O(n log n) | O(n log n) | O(1) | T(n) = T(n/2) + O(log n) (heapify, per call) |
| Problem Reduction (e.g. LCM via GCD) | Depends on reduced problem | Depends on reduced problem | Depends on reduced problem | Depends on reduced problem | N/A (depends on target problem) |

## Unit III — Space & Time Trade-offs

| Algorithm | Best Case | Average Case | Worst Case | Space | Recurrence Relation |
|---|---|---|---|---|---|
| Sorting by Counting (Counting Sort) | O(n + k) | O(n + k) | O(n + k) | O(n + k) | N/A (iterative) |
| Naive String Matching (Brute Force) | O(n) | O(n·m) | O(n·m) | O(1) | N/A (iterative) |
| Horspool's Algorithm | O(n/m) | O(n) | O(n·m) | O(m + k) | N/A (iterative) |
| Boyer-Moore Algorithm | O(n/m) | O(n) | O(n·m) | O(m + k) | N/A (iterative) |

*(n = text length, m = pattern length in string matching rows)*

---

## Unit IV — Dynamic Programming

| Algorithm | Best Case | Average Case | Worst Case | Space | Recurrence Relation |
|---|---|---|---|---|---|
| Binomial Coefficient (DP table) | O(n·k) | O(n·k) | O(n·k) | O(n·k) / O(k) optimized | C(n,k) = C(n−1,k−1) + C(n−1,k), with C(n,0)=C(n,n)=1 |
| Warshall's Algorithm (Transitive Closure) | O(V³) | O(V³) | O(V³) | O(V²) | Rᵏ[i,j] = Rᵏ⁻¹[i,j] OR (Rᵏ⁻¹[i,k] AND Rᵏ⁻¹[k,j]) |
| Floyd's Algorithm (All-Pairs Shortest Path) | O(V³) | O(V³) | O(V³) | O(V²) | Dᵏ[i,j] = min(Dᵏ⁻¹[i,j], Dᵏ⁻¹[i,k] + Dᵏ⁻¹[k,j]) |
| 0/1 Knapsack (DP) | O(n·W) | O(n·W) | O(n·W) | O(n·W) / O(W) optimized | F(i,w) = max(F(i−1,w), vᵢ + F(i−1,w−wᵢ)) if wᵢ ≤ w, else F(i,w) = F(i−1,w) |
| 0/1 Knapsack (Memory Functions) | O(n·W) | O(n·W) | O(n·W) | O(n·W) | Same as above, evaluated top-down with memoization |

## Unit IV — Greedy Technique

| Algorithm | Best Case | Average Case | Worst Case | Space | Recurrence Relation |
|---|---|---|---|---|---|
| Prim's Algorithm (binary heap) | O(E log V) | O(E log V) | O(E log V) | O(V) | N/A (iterative, greedy) |
| Dijkstra's Algorithm (binary heap) | O((V + E) log V) | O((V + E) log V) | O((V + E) log V) | O(V) | N/A (iterative, greedy) |
| Huffman Trees / Codes | O(n log n) | O(n log n) | O(n log n) | O(n) | N/A (iterative, priority-queue based) |
| Fractional Knapsack | O(n log n) (sort-dominated) | O(n log n) | O(n log n) | O(1) / O(n) | N/A (iterative, greedy) |

---

## Unit V — Backtracking

| Algorithm | Best Case | Average Case | Worst Case | Space | Recurrence Relation |
|---|---|---|---|---|---|
| N-Queens Problem | O(N!) (with pruning, much better in practice) | Sub-factorial (pruning-dependent) | O(N!) | O(N) | T(n) = n·T(n−1) + O(n) |
| Sum of Subsets Problem | O(2ⁿ) (pruned) | Sub-exponential (pruning-dependent) | O(2ⁿ) | O(n) | T(n) = 2T(n−1) + O(1) |

## Unit V — Branch and Bound

| Algorithm | Best Case | Average Case | Worst Case | Space | Recurrence Relation |
|---|---|---|---|---|---|
| Travelling Salesperson Problem (B&B) | O(n²) (best-case bounding) | Exponential (bound-dependent) | O(n!) | O(n²) | T(n) = (n−1)·T(n−1) (worst case, no effective pruning) |
| Assignment Problem (B&B) | O(n²) (best-case bounding) | Exponential (bound-dependent) | O(n!) | O(n²) | T(n) = n·T(n−1) (worst case, no effective pruning) |

## Unit V — Decision Trees

| Algorithm | Best Case | Average Case | Worst Case | Space | Recurrence Relation |
|---|---|---|---|---|---|
| Decision Trees for Sorting (comparison-based lower bound) | O(n log n) | O(n log n) | O(n log n) | O(n) | N/A (lower-bound argument, not a recursive algorithm) |

## Unit V — NP and NP-Complete Problems

| Concept | Time Complexity Note | Recurrence Relation |
|---|---|---|
| P Class | Solvable in polynomial time: O(nᵏ) for constant k | N/A |
| NP Class | Verifiable in polynomial time: O(nᵏ); solving may require exponential time O(2ⁿ) | N/A |
| NP-Complete | No known polynomial algorithm; best known solutions are exponential O(2ⁿ) or O(n!) | N/A |
| NP-Hard | At least as hard as NP-Complete; may not even be verifiable in polynomial time | N/A |
| Non-Deterministic Algorithms | Conceptually O(1) guess + O(nᵏ) polynomial verify (theoretical model) | N/A |

---

### Notes
- "Best/Average/Worst" is not meaningful for purely structural graph algorithms (DFS, BFS, Warshall, Floyd, Dijkstra, Prim) since their complexity depends only on input size (V, E), not input arrangement — hence identical across all three columns.
- Backtracking and Branch-and-Bound worst cases assume no effective pruning (exhaustive search); real-world performance is usually far better due to bounding functions.
- Recurrence relations are shown only for algorithms that are naturally recursive (divide-and-conquer, decrease-and-conquer, DP with recursive definitions, backtracking/branch-and-bound). Purely iterative algorithms are marked N/A.
- Solving these recurrences (via Master Theorem or substitution) is what yields the closed-form time complexities listed in the earlier columns.
