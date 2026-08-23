# Algorithms & Graph Theory — Advanced Study Notes

---

## PART 1 — Principles of Algorithms

### 1.1 The Core Idea

The goal of algorithm design is: **solve real-world problems (RWP) using a computer.**

A real-world problem deals with **physical objects**. A computer only understands **mathematical objects**. So every RWP must first be translated into a **mathematical problem inside a mathematical model** before a computer can touch it.

```
Real-World Problem  <---->  Mathematical Problem (in a Mathematical Model)
   (physical objects)            (mathematical/abstract objects)
```

A **mathematical model** (algebraic system) = **(Set of abstract objects, Set of operations)**.

Examples of this pairing:
- **Integers** → operations: `+, −, ×, mod, div`
- **Sets** → operations: `union, intersection, insert, delete, search`
- In Computer Science, this pairing is formalized as an **Abstract Data Type (ADT)**:
  > **ADT = Mathematical Model + Set of Operations**

So the pipeline is:

```
Real World Problem → Mathematical Problem (ADT) → Algorithm
```

### 1.2 Computational Problems — Formal Anatomy

Once a problem is inside a mathematical model, it becomes a **computational problem**, which has three parts:

| Component | Meaning |
|---|---|
| **Input** | The set of values the problem must work with |
| **Process** | The sequence of operations applied to the input |
| **Output** | The result of the process |

On top of this, every computational problem has:

- **Specification** — the *relation between input and output values* (i.e., **what** should happen, stated declaratively — not how).
- **Solution** — an *explicit description of the process* that gets you from input to output (i.e., **how** it happens, at a conceptual/mathematical level).
- **Algorithm** — the **execution** of that solution on an actual input, such that it produces the corresponding output. An algorithm is the *operational realization* of a solution — concrete enough to run step by step.

> **Key distinction (commonly tested):** Specification tells you *what* the answer should look like. Solution tells you *the method*. Algorithm is the *executable* version of that method. Many problems can have multiple solutions, and each solution can have multiple algorithms (implementations).

### 1.3 Complexity — Two Axes

Every algorithm is judged along two axes:
1. **Time Complexity** — how much computational work it does.
2. **Space Complexity** — how much memory it uses.

### 1.4 Input Size — "The Problem of Scale"

Complexity is always measured **as a function of input size**, because raw running time (seconds, minutes) is machine/hardware-dependent and not a fair measure. What input size means depends on the problem:

| Problem | Input size parameter(s) |
|---|---|
| Sorting | `n` = number of items to be sorted |
| Multiplying matrices A(p×q) · B(q×r) = C(p×r) | `p, q, r` (general case) |
| Multiplying two `n×n` matrices | `n` |
| Graph `G = (V, E)`, `\|V\|=n`, `\|E\|=m` | `n, m` |

**Why not measure in seconds?** Because "time complexity" really means: *given the specification/description of the algorithm, how many operations (instruction count) does it perform* — independent of the machine executing it. Running time (seconds, ms) is the *actual execution* of code on hardware; time complexity abstracts this away using **operation count**, which is always a **positive integer**.

> Example: For sorting, the natural atomic operation to count is **"comparison."**

---

## PART 2 — Time Complexity: The Deep Dive

### 2.1 Worst-Case Complexity (WCC) — Formal Definition

Let:
- `I` = a single input instance
- `IC(I)` = instruction count when running the algorithm on input `I`
- `I_n` = **the set of all inputs of size `n`**

Then the **Worst-Case Complexity function** is:

$$WCC(n) = \max_{I \in I_n} IC(I)$$

In words: *WCC(n) associates an input size `n` to the **maximum** instruction count over all possible inputs of that size.* This maximum-instruction-count function is written as `f(n)` and called the **complexity function**. The efficiency of an algorithm is judged by its complexity function `f(n)`.

### 2.2 Why We Need Asymptotics — Growth Rate Intuition

Suppose two algorithms have complexity functions:
- Algorithm A: `f(n) = 7n² + 9n + 10`
- Algorithm B: `g(n) = 10n + 23`

For **small `n`**, either could be faster depending on constants — but we care about **large `n`** (scalability). Compare growth:

| n | n² | n³ |
|---|---|---|
| 10 | 100 | 1,000 |
| 100 | 10⁴ | 10⁶ |
| 1,000 | 10⁶ | 10⁹ |

Even with *favorable* constants (A = 10000n², B = (1/100)n³), the polynomial with the **higher degree eventually always wins**:

| n | A = 10000n² | B = (1/100)n³ |
|---|---|---|
| 10 | 10⁶ | 10 |
| 1,000 | 10¹⁰ | 10⁷ |
| 10⁶ | 10¹⁶ | 10¹⁶ (crossover) |
| 10⁸ | 10²⁰ | 10²² (B overtakes) |

**Lesson:** constants become irrelevant as `n → ∞`. This is exactly why we use **asymptotic notation** — it strips away constants and lower-order terms and focuses purely on **growth rate**.

### 2.3 Big-O Notation — Formal Definition (Upper Bound)

> `f(n)` is `O(g(n))` if `f(n) ≤ c·g(n)` for some constant `c > 0`, for all `n ≥ n₀`.

`O` gives an **upper bound** — it says `f` grows *no faster than* `g` (up to a constant factor), beyond some threshold `n₀`.

**Worked proof:** Show `7n² + 9n + 10` is `O(n²)`.

$$7n^2+9n+10 \le 7n^2+9n^2+10n^2 = 26n^2 \quad \text{(valid since } n\ge 1 \Rightarrow n \le n^2 \le n^2\text{)}$$

So `7n²+9n+10 ≤ 26n²` for all `n ≥ 1` ⟹ with `c = 26`, `n₀ = 1`, **`7n²+9n+10` is `O(n²)`.** ✔

**Important subtlety:** the *same* function is *also* `O(n⁴)`:

$$7n^2+9n+10 \le 7n^4+9n^4+10n^4 = 26n^4, \quad \forall n \ge 1$$

> **Takeaway (frequently tested):** Big-O is **not unique/tight** — a function can be `O` of many larger functions. `O(n²)` is the *tightest* (most informative) bound here, but `O(n⁴)`, `O(n^{100})`, `O(2^n)` are all *technically true* upper bounds too. This is why questions like "which one is FALSE" often hinge on whether a **looser** `O` bound is still valid (it is) versus whether a **Theta** claim with the wrong degree is valid (it is not — see 2.4).

### 2.4 Theta (Θ) Notation — Formal Definition (Tight Bound)

> `f(n)` is `Θ(g(n))` if there exist constants `c₁, c₂ > 0` such that
> `c₁·g(n) ≤ f(n) ≤ c₂·g(n)` for all `n ≥ n₀`.

Θ means `f` and `g` have the **same growth rate** — both an upper *and* lower bound simultaneously.

**Worked proof:** Show `7n²+9n+10` is `Θ(n²)`.

We already have the upper half: `7n²+9n+10 ≤ 26n²` for `n ≥ 1`.
For the lower half, note `7n²+9n+10 ≥ 7n² ≥ 3n²` (loosely) for `n ≥ 1`.

So: `3n² ≤ 7n²+9n+10 ≤ 26n²` for all `n ≥ 1` ⟹ `c₁=3, c₂=26, n₀=1`.

**∴ `7n²+9n+10` is `Θ(n²)`.**

> **Critical rule:** `Θ` requires the growth rates of `f` and `g` to be **exactly the same**. This is *not* true for `O` — `O` only needs `g` to grow *at least as fast*. This single distinction is the crux of most "true/false" asymptotic quiz questions.

### 2.5 The Limit Test (a fast shortcut for proofs)

Instead of algebra, you can classify using the limit of the ratio:

$$
\lim_{n\to\infty}\frac{f(n)}{g(n)} = \begin{cases}
0 & \Rightarrow f(n) = o(g(n)) \text{ (strictly smaller order)}\\
c, \ c>0 & \Rightarrow f(n) = \Theta(g(n))\\
\infty & \Rightarrow f(n) = \omega(g(n)) \text{ (strictly larger order, and } g(n)=O(f(n)) \text{ but not vice versa)}
\end{cases}
$$

Equivalently: if `lim (f/g) = c ≥ 0` (finite), then `f(n)` is `O(g(n))`. If `lim (g/f) = c ≥ 0` (finite, i.e. `lim f/g = ∞` or a positive constant), that also confirms `O`/`Θ` relationships. Use whichever ratio is easier to compute.

**This limit test is the fastest way to settle True/False questions** like Q1 and Q2 below — no need for the full `c, n₀` proof every time.

### 2.6 Growth Rate Hierarchy (memorize this order)

$$
\Theta(\log n) \;<\; \Theta(n) \;<\; \Theta(n\log n) \;<\; \Theta(n^2) \;<\; \Theta(n^3) \;<\; \cdots \;<\; \Theta(2^n) \;<\; \Theta(n!) \;<\; \Theta(n^n)
$$

- An `Θ(n log n)` algorithm is **always** asymptotically better than an `Θ(n²)` algorithm for large `n` — this is *why* comparison-based sorts like Merge Sort / Heap Sort (`n log n`) beat Bubble/Insertion Sort (`n²`) at scale.
- **Exponentials beat all polynomials, no matter the polynomial's degree.** `2^n` is *not* `O(n^{10000})` — it's the reverse: `n^{10000} = O(2^n)`.
- **Factorial beats all exponentials with a fixed base.** `n!` is not `O(3^n)`, but any fixed exponential `a^n` (constant `a`) **is** `O(n!)`.
- A useful reasoning trick for `aⁿ` vs `bⁿ⁻¹` type comparisons: rewrite `bⁿ⁻¹ = bⁿ/b`, so it reduces to comparing `aⁿ` vs `(1/b)·bⁿ` — i.e., compare the **bases** `a` and `b`. If `a < b`, then `aⁿ = o(bⁿ⁻¹)` (strictly smaller, not Θ). If `a = b`, they're `Θ` of each other (constant factor `b` only rescales).

### 2.7 Logarithm Simplification Rule

`log_b(n^k) = k · log_b(n)`. Since `k` is a constant, and change of base (`log_10 x = log_2 x / log_2 10`) only introduces another constant multiplier, **all logarithms of `n` — regardless of base or power — are `Θ(log n)` of each other**, as long as the base and exponent are fixed constants (not functions of `n`).

---

## PART 3 — Graphs: Foundations (Directed)

### 3.1 Why Graphs?

Graphs model **relationships/connections**: communication networks, social networks, transportation networks, distributed systems.

### 3.2 Formal Definition

A **directed graph** `G = (V, E)` where:
- `V` — a finite, non-empty set (the **vertex set**)
- `E ⊆ V × V` — the **edge set**, a subset of *ordered pairs*

Since `V × V` = set of **all ordered pairs** of elements of `V`:
- If `|V| = n`, then `|V × V| = n²`
- `(1,2) ≠ (2,1)` as ordered pairs — this is exactly what makes the graph **directed**. (Contrast with sets, where `{1,2} = {2,1}`.)

### 3.3 Directed Edges

`(a,b) ∈ E` is a **directed edge**: drawn as `a → b`. We say the edge **leaves `a`** and **arrives at `b`**; the arrowhead encodes direction.

### 3.4 Degree in Directed Graphs

- **Out-degree(v)** = number of edges **leaving** `v`
- **In-degree(v)** = number of edges **arriving at** `v`

**Fundamental identity (always test-relevant):**

$$\sum_{v \in V} \text{out-degree}(v) = |E| = \sum_{v \in V} \text{in-degree}(v)$$

*Why:* every edge contributes exactly **one unit** to some vertex's out-degree (its tail) and exactly **one unit** to some vertex's in-degree (its head) — regardless of self-loops.

### 3.5 Simple Directed Graphs

A directed graph is **simple** if it has:
- **no self-loops** (no edge `(a,a)`)
- **no multiple/parallel edges** between the same ordered pair

### 3.6 Adjacency Matrix Representation (Directed)

For `|V| = n`, the adjacency matrix `A` is `n × n`:

$$A[i,j] = \begin{cases}1 & \text{if } (i,j)\in E\\ 0 & \text{if } (i,j)\notin E\end{cases}$$

- **Total size:** `n²` entries (this is the theoretical maximum possible number of 1's — every ordered pair could be an edge).
- A directed graph's adjacency matrix is generally **not symmetric** (`A[i,j]` need not equal `A[j,i]`) — reflects direction.

### 3.7 Adjacency List Representation (Directed)

- An **array of `n` linked lists**, one per vertex (`|V| = n` lists).
- The list for vertex `i` contains every `j` such that `(i,j) ∈ E` — i.e., all of `i`'s out-neighbors.
- **Total size of adjacency list (directed) = `|E|`** — each edge appears exactly once (in the tail vertex's list).

---

## PART 4 — Graphs: Undirected Graphs

### 4.1 Formal Definition

`G = (V, E)` where edges are now **unordered pairs**: `{a,b}` instead of `(a,b)`.

Key conceptual contrast:
- **Undirected edges** behave like **"two-way" streets** — `{a,b}` allows travel both `a→b` and `b→a`.
- **Directed edges** behave like **"one-way" streets** — `(a,b)` allows only `a→b`.

### 4.2 Adjacency Matrix (Undirected) — Always Symmetric

If `{i,j} ∈ E`, then **both** `A[i,j] = 1` **and** `A[j,i] = 1` (the edge is bidirectional by nature — it's the *same* edge counted from both endpoints).

> **This is the key structural fact:** *the adjacency matrix of an undirected graph is always symmetric* (`A = Aᵀ`). Compare with directed graphs where a single directed edge `(i,j)` sets only `A[i,j]=1`, leaving `A[j,i]=0` unless the reverse edge also explicitly exists.

### 4.3 Adjacency List (Undirected) — Double Bookkeeping

For an undirected edge `{i,j}`:
- We include `j` in `i`'s adjacency list, **AND**
- We include `i` in `j`'s adjacency list.

So the **same single edge** `{i,j}` is stored **twice** (once in each endpoint's list).

> **Total size of adjacency list for an undirected graph = `2|E|`** (vs. `|E|` for directed graphs, where each edge is stored exactly once).

### 4.4 Degree in Undirected Graphs

**Degree(v)** = number of edges **incident on** `v` (no in/out distinction — undirected edges aren't oriented).

**Handshake Lemma:**

$$\sum_{v \in V} d(v) = 2|E|$$

*Why the factor of 2:* every undirected edge `{i,j}` contributes **+1 to `d(i)`** and **+1 to `d(j)`** — i.e., each edge is "counted from both ends," so summing all degrees double-counts every edge.

*(Compare: for directed graphs, `Σ in-degree(v) = |E|` and `Σ out-degree(v) = |E|` separately — no factor of 2, because in-degree and out-degree are tracked independently.)*

---

## PART 5 — Advanced Worked Problem: Max Ones in an Adjacency Matrix

This is a classic "derive-it-from-first-principles" style question (appears in the quiz below as Q8 & Q9). Here's the full reasoning, generalized for any `n`.

**Setup:** `G` is a directed graph on `n` vertices. The adjacency matrix has `n²` total cells. We want the **maximum possible number of 1's**, under varying constraints.

**Case A — No self-loops allowed (general directed graph, anti-parallel edges OK):**
- Self-loop cells are the diagonal: `n` cells, all forced to `0`.
- Remaining off-diagonal cells: `n² − n = n(n−1)`.
- Since **anti-parallel edges are allowed** (both `(i,j)` and `(j,i)` can independently exist), *every one* of these `n(n−1)` off-diagonal cells can be `1` simultaneously.
- **Max ones = `n(n−1)`.** For `n = 10`: `10 × 9 = 90`. ✔ *(matches Q8's accepted answer of 90)*

**Case B — Self-loops allowed, but NO anti-parallel edges** (i.e., if `(x,y) ∈ E` then `(y,x) ∉ E`):
- Self-loops: all `n` diagonal cells can be `1` (self-loops don't conflict with the anti-parallel rule, since `(x,x)` and its "reverse" are the same cell) → contributes `n` ones.
- Off-diagonal: for each **unordered pair** `{i,j}` (`i ≠ j`), the two ordered cells `(i,j)` and `(j,i)` are now **mutually exclusive** — at most **one** of them can be `1`. Number of unordered pairs = `C(n,2) = n(n−1)/2`. Each contributes at most 1 one → `n(n−1)/2` ones.
- **Total max ones = `n + n(n−1)/2 = n(n+1)/2`.** For `n = 10`: `10 × 11 / 2 = 55`. ✔ *(matches Q9's accepted answer of 55)*

> **General pattern to remember:** `n(n−1)` = max ones with *no self-loops, anti-parallel OK*; `n(n+1)/2` = max ones with *self-loops OK, no anti-parallel*; `n(n−1)/2` would be the count if *both* self-loops were banned *and* anti-parallel edges were banned (a "simple undirected-style" count).

---

## PART 6 — Compiled Quiz (with Full Explanations)

### Q1. State true or false: `log₁₀(nᵏ) = Θ(log₂ n)`
**Answer: True** *(Score: 1)*

**Explanation:** `log₁₀(nᵏ) = k·log₁₀(n)`. Using change of base, `log₁₀(n) = log₂(n) / log₂(10)`. So:
`log₁₀(nᵏ) = [k / log₂(10)] · log₂(n)`.
Since `k` and `log₂(10)` are both positive constants (independent of `n`), the coefficient `k/log₂(10)` is just some constant `c > 0`. This exactly satisfies the `Θ` definition — same growth rate, differing only by a constant multiplier. **→ True.**

---

### Q2. State true or false: `2.99ⁿ = Θ(3ⁿ⁻¹)`
**Answer: False** *(Accepted: False)*

**Explanation:** Rewrite `3ⁿ⁻¹ = 3ⁿ/3`. Take the limit of the ratio:

$$\lim_{n\to\infty} \frac{2.99^n}{3^{n-1}} = \lim_{n\to\infty} 3\cdot\left(\frac{2.99}{3}\right)^n = 3 \cdot 0 = 0 \quad (\text{since } 2.99/3 < 1)$$

The ratio goes to **0**, not a positive constant — this means `2.99ⁿ` grows **strictly slower** than `3ⁿ⁻¹` (i.e., `2.99ⁿ = o(3ⁿ⁻¹)`), not the same order. **Θ requires equal growth rate → False.**

---

### Q3. State true or false: `3ⁿ = O(n!)`
**Answer: True** *(Score: 1)*

**Explanation:** Factorial growth eventually dominates any fixed-base exponential. Formally, `n!/3ⁿ → ∞` as `n → ∞` (since each new factor added to `n!` eventually exceeds 3, while `3ⁿ` only ever multiplies by 3 each step). Since `3ⁿ` grows **no faster** than `n!`, `3ⁿ` qualifies as `O(n!)` (this is an upper bound claim, which only needs `3ⁿ ≤ c·n!` eventually — easily satisfied). **→ True.**

---

### Q4. State true or false: `n! = O(3ⁿ)`
**Answer: False** *(Accepted: False)*

**Explanation:** This is the reverse direction of Q3. Since `n!` grows **strictly faster** than `3ⁿ` (as shown above, the ratio `n!/3ⁿ → ∞`), it is **impossible** to bound `n!` above by any constant multiple of `3ⁿ` for large `n`. So `n!` is **not** `O(3ⁿ)` — in fact `3ⁿ = O(n!)` but not vice-versa. **→ False.**

---

### Q5. Fill in the blank: `1 + 1/2 + 1/3 + ... + 1/n = Θ(____)`
**Answer: `log n`** *(Score: 1)*

**Explanation:** This sum is the **Harmonic Series**, `Hₙ = Σᵢ₌₁ⁿ (1/i)`. A classical result (provable by comparing the sum to the integral `∫₁ⁿ (1/x) dx = ln n`) shows `Hₙ` grows exactly like the natural log:
`ln(n) ≤ Hₙ ≤ ln(n) + 1`.
Since `ln n = log n` up to a constant factor (base change), `Hₙ = Θ(log n)`. **→ `log n`.**

---

### Q6. Which one is correct?
Options: `2ⁿ = Θ(n³)` | `2ⁿ = O(n⁴)` | `2ⁿ = O(n¹⁰⁰⁰⁰)` | **`2ⁿ = Θ(2ⁿ⁻¹)`** ✔

**Answer: `2ⁿ = Θ(2ⁿ⁻¹)`**

**Explanation:**
- `2ⁿ = 2 · 2ⁿ⁻¹` — the ratio `2ⁿ/2ⁿ⁻¹ = 2`, a fixed positive constant → satisfies `Θ` exactly. **True.**
- `2ⁿ = Θ(n³)` is **false** — exponential vs. polynomial growth are never `Θ` of each other (Part 2.6 hierarchy).
- `2ⁿ = O(n⁴)` is **false** — this claims exponential growth is bounded above by a *degree-4 polynomial*, but exponentials eventually outgrow **every** fixed-degree polynomial. It's actually the reverse: `n⁴ = O(2ⁿ)`.
- `2ⁿ = O(n¹⁰⁰⁰⁰)` is **false** for the same reason — no matter how large the fixed polynomial degree, `2ⁿ` eventually overtakes it.

---

### Q7. Which one is FALSE?
Options: `7n²log n + 8n − 9 = O(n²log n)` | `= Θ(n²log n)` | `= O(n³)` | **`= Θ(n³)`** ✘ (false)

**Answer: `7n²log n + 8n − 9 = Θ(n³)` is FALSE**

**Explanation:** Let `f(n) = 7n²log n + 8n − 9`.
- `f(n) = O(n²log n)` — **True**: `f(n)` is bounded above by a constant multiple of `n²log n` (the dominant term matches exactly).
- `f(n) = Θ(n²log n)` — **True**: since `f(n)`'s dominant term *is* `7n²log n`, both an upper and lower bound of the same order hold — the growth rates match exactly.
- `f(n) = O(n³)` — **True but loose**: since `n²log n` grows slower than `n³` (log n is sub-polynomial), `n²log n = O(n³)`, and hence `f(n) = O(n³)` also holds — `O` permits looser bounds (see Part 2.3's subtlety).
- `f(n) = Θ(n³)` — **False**: `Θ` demands the growth rates be *identical*. But `n²log n` and `n³` do **not** have the same growth rate — `n²log n / n³ = log n / n → 0`, meaning `n²log n` is strictly *smaller order* than `n³`. So `f(n)` cannot be `Θ(n³)`. **This is the false statement.**

---

### Q8. Directed graph, 10 vertices, no self-loops. Max number of 1's in adjacency matrix?
**Answer: 90**

**Explanation:** Total matrix cells = `n² = 100`. Remove the `n = 10` diagonal (self-loop) cells → `90` off-diagonal cells remain. Since anti-parallel edges are **not** restricted here, every remaining off-diagonal cell can independently be `1`. **Max ones = `n(n−1) = 10 × 9 = 90`.** (See full derivation in Part 5, Case A.)

---

### Q9. Directed graph, 10 vertices, self-loops allowed but **no anti-parallel edges** (if `(x,y) ∈ E` then `(y,x) ∉ E`). Max number of 1's?
**Answer: 55**

**Explanation:** All `n = 10` diagonal (self-loop) cells can be `1` → `+10`. For every unordered pair `{i,j}`, at most **one** of `(i,j)`/`(j,i)` can be an edge (anti-parallel forbidden) → at most `C(10,2) = 45` off-diagonal ones. **Total = `10 + 45 = 55` = `n(n+1)/2`.** (See full derivation in Part 5, Case B.)

---

## Quick-Reference Cheat Sheet

| Concept | Formula / Rule |
|---|---|
| WCC definition | `WCC(n) = max over I ∈ I_n of IC(I)` |
| Big-O (upper bound) | `f(n) ≤ c·g(n)`, `∀n ≥ n₀`, some `c>0` |
| Theta (tight bound) | `c₁g(n) ≤ f(n) ≤ c₂g(n)`, `∀n ≥ n₀` |
| Growth hierarchy | `log n < n < n log n < n² < n³ < 2ⁿ < n!` |
| Directed: Σ out-degree | `= \|E\|` |
| Directed: Σ in-degree | `= \|E\|` |
| Undirected: Σ degree | `= 2\|E\|` |
| Adjacency matrix size | `n × n` (both directed & undirected) |
| Adjacency list size (directed) | `\|E\|` |
| Adjacency list size (undirected) | `2\|E\|` |
| Max ones, no self-loop, directed | `n(n−1)` |
| Max ones, self-loop OK, no anti-parallel | `n(n+1)/2` |
