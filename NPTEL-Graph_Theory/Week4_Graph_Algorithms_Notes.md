# Graph Algorithms — Advanced Study Notes
### Heaps · Dijkstra's Algorithm · Bellman-Ford Algorithm

---

## Table of Contents
1. [Binary Min-Heaps](#1-binary-min-heaps)
2. [Dijkstra's Algorithm](#2-dijkstras-algorithm)
3. [Bellman-Ford Algorithm](#3-bellman-ford-algorithm)
4. [How Dijkstra & Bellman-Ford Relate](#4-how-dijkstra--bellman-ford-relate)
5. [Quiz — Compiled Questions with Explanations](#5-quiz--compiled-questions-with-explanations)

---

## 1. Binary Min-Heaps

Heaps are the data structure that makes Dijkstra's algorithm fast, so we start here.

### 1.1 Structural definition
A **binary heap** with $n$ nodes is a complete binary tree — every level is full except possibly the last, which fills left to right. This structural constraint is what guarantees:

$$\text{height} = O(\log n)$$

because a complete binary tree of height $h$ has at most $2^{h+1}-1$ nodes.

### 1.2 Heap-order property
For a **min-heap**: every parent's key is $\le$ both of its children's keys.
$$\text{key}(\text{parent}) \le \text{key}(\text{child})$$
This does **not** impose any left-right ordering between siblings — only the parent-child relationship matters. The minimum element is always at the root.

### 1.3 Array representation
A heap is stored implicitly in an array (1-indexed):
- `parent(i) = i / 2` (floor)
- `left(i) = 2i`
- `right(i) = 2i + 1`

No pointers are needed — index arithmetic reconstructs the tree.

### 1.4 Core operations & complexity

| Operation | Idea | Time |
|---|---|---|
| `Find-Min` | Read root | O(1) |
| `Delete-Min` | Move last element to root, sift down (heapify) | O(log n) |
| `Insert(x)` | Place at next free leaf, **sift/bubble up** | O(log n) |
| `Decrease-Key(v, newval)` | Lower the value in place, **sift/bubble up** | O(log n) |

**Why insert and decrease-key are both O(log n):** both only ever need to fix violations *along a single root-to-leaf path* (upwards), and that path has length $O(\log n)$.

**Insert algorithm (bubble-up):**
1. Place new element at index $n+1$ (next free slot — keeps tree complete).
2. While it is smaller than its parent: swap it with the parent.
3. Stop when it reaches the root or is $\ge$ its parent.

**Decrease-Key algorithm:**
1. Overwrite the value at node $v$ with the smaller value.
2. Bubble it up exactly like insert (a decreased key can only violate the heap property *upward*, never downward, since children are unaffected and were already $\ge$ old value $\ge$ new value).

### 1.5 Worked Example (from your quiz)

Heap $H$ (7 nodes), index outside / value inside:

```
Index:   1   2   3   4   5   6   7
Value:   5   10  15  25  20  35  30
```
Tree shape: 1(5) → 2(10), 3(15); 2 → 4(25), 5(20); 3 → 6(35), 7(30).

**Operation: `Insert(3)`**

Step 1 — place 3 at index 8 (next free leaf, child of index 4):
```
idx: 1  2  3  4  5  6  7  8
val: 5 10 15 25 20 35 30  3
```
Step 2 — bubble up:
- Compare idx8(3) with parent idx4(25): 3 < 25 → **swap** → idx4=3, idx8=25
- Compare idx4(3) with parent idx2(10): 3 < 10 → **swap** → idx2=3, idx4=10
- Compare idx2(3) with parent idx1(5): 3 < 5 → **swap** → idx1=3, idx2=5
- idx1 is the root → stop.

**Resulting heap $H'$:**
```
idx: 1  2  3  4  5  6  7  8
val: 3  5 15 10 20 35 30 25
```
- ✅ Value at **index 8** = **25** (the value displaced all the way down as 3 rose up).
- ✅ The original root value **5** ends up at **index 2**.

*(Intuition: insertion is really "the new small value climbs to where it belongs, and every value it passes gets pushed one level down its old path.")*

**Operation: `Decrease-Key` on node with index 6 (value 35 → new value 2)**

Starting from $H'$ above (idx6 = 35, untouched by the earlier insert since it wasn't on the bubble-up path):

- Set idx6 = 2.
- Compare with parent idx3(15): 2 < 15 → swap → idx3=2, idx6=15
- Compare with parent idx1(3): 2 < 3 → swap → idx1=2, idx3=3
- idx1 is root → stop.

**Final heap:**
```
idx: 1  2  3  4  5  6  7  8
val: 2  5  3 10 20 15 30 25
```
- ✅ Value at **the root (index 1)** = **2**
- ✅ Value at **index 6** = **15**

### 1.6 Why this matters for Dijkstra
Dijkstra repeatedly needs "the vertex in $V-S$ with the smallest tentative distance" (`Find-Min`/`Delete-Min`) and "lower a vertex's tentative distance when a shorter path is found" (`Decrease-Key`). A heap gives both in $O(\log n)$, which is the entire reason the heap-based implementation beats the naive array-based one on sparse graphs (see §2.5).

---

## 2. Dijkstra's Algorithm

### 2.1 Problem setup
Given $G = (V, E, w, s)$ — a **directed, weighted graph with $w(e) > 0$ for every edge**, and a source $s$ — find $\delta(v)$, the shortest-path weight from $s$ to every other vertex.

> ⚠️ **Critical restriction:** Dijkstra's algorithm is only correct when **all edge weights are strictly positive**. Negative weights (even a single one) can break the greedy argument in §2.3 — that's exactly why Bellman-Ford exists (§3).

### 2.2 The key structural insight: sorted shortest paths
Because $w(e) > 0$, if you walk along the shortest path tree (SPT) from $s$: $s \to t \to \omega \to u \to v$, the $\delta$ values are **strictly increasing** along the path:
$$\delta(s) < \delta(t) < \delta(\omega) < \delta(u) < \delta(v)$$
This means: **if we discover the $\delta$ values in increasing order, each new vertex's shortest path only ever depends on vertices we've already finalized.** This is the greedy insight that makes Dijkstra work.

### 2.3 The "special path" argument (correctness proof)
Partition $V$ into:
- $S$ = vertices for which $\delta(\cdot)$ is **already known/finalized**
- $V-S$ = vertices not yet finalized

Initially $S = \{s\}$, $\delta(s) = 0$.

**Definition — special path:** a path from $s$ to $v \in V-S$ is a *special path* if **every vertex on it except $v$ itself lies in $S$.**

For each $v \in V-S$, define:
$$d[v] = \text{weight of the shortest special path from } s \text{ to } v$$

**Claim:** if $v$ is the vertex in $V-S$ with the **minimum** $d[\cdot]$ value, then $d[v] = \delta(v)$ (the special-path distance IS the true shortest-path distance), so $v$ can safely be moved into $S$.

**Proof (by contradiction):**
Let $P$ be the special path achieving $d[v]$. Suppose $P$ is *not* the true shortest path — let $Q$ be an actual shortest path from $s$ to $v$, so $w(Q) < w(P) = d[v]$.

Since $v \notin S$ but $s \in S$, $Q$ must cross the $S / (V-S)$ boundary at least once. Let $(u', u)$ be the **first crossing edge** on $Q$ (i.e., $u' \in S$, and everything before $u'$ on $Q$ is in $S$, so $Q[s,u]$ is itself a special path to $u$).

Then:
$$w(Q) = w(Q[s,u]) + w(Q[u,v])$$
Since all remaining edge weights are $> 0$ (this is where positivity is used!):
$$w(Q[s,v]) = w(Q[s,u]) + w(Q[u,v]) > w(Q[s,u]) \ge d[u] \ge d[v] = w(P)$$
(The middle step uses $w(Q[s,u]) \ge d[u]$ because $Q[s,u]$ *is* a special path to $u$, so it can't beat the shortest special path to $u$; the last step uses $d[u] \ge d[v]$ because $v$ was chosen as the vertex with *minimum* $d$ value.)

So $w(Q) > w(P)$ — **contradicting** our assumption that $w(Q) < w(P)$. Hence no such shorter $Q$ can exist. $P$ **is** the true shortest path, $d[v] = \delta(v)$. $\blacksquare$

This is exactly why Dijkstra is a **greedy algorithm**: at each step, greedily finalizing the minimum-$d$ vertex is provably optimal — no future information can ever produce a shorter path to it (again, only true because weights are non-negative).

### 2.4 Algorithm

**State maintained per vertex $u \in V-S$:**
- $d[u]$ = weight of the shortest *known* special path to $u$ so far
- $p[u]$ = predecessor of $u$ on that path (the **Bellman edge** $(p[u], u)$)

**Initialization:**
$$d[v] = \begin{cases} 0 & v = s \\ w(s,v) & v \in \text{Adj}(s) \\ \infty & \text{otherwise}\end{cases} \qquad p[v] = \begin{cases}\perp & v = s\\ s & v \in \text{Adj}(s)\\ \perp & \text{otherwise}\end{cases}$$
$S$ and $V-S$ are tracked with a boolean array `In-S[·]` (In-S[s]=1, else 0).

**Main loop — `DIJKSTRA(G)`:**
```
1. Initialize (S, V-S), d[], p[]
2. while (S ≠ V):
   2a. Find vertex v in V-S such that d[v] ≤ d[u] for all u in V-S   // Find-Min
   2b. Move v to S                                                   // In-S[v] = 1
   2c. for every u in Adj(v):
         if (u ∈ V-S) and d[u] > d[v] + w(v,u):
             d[u] = d[v] + w(v,u)      // relaxation / decrease-key
             p[u] = v
3. return d[v], p[v] for all v ∈ V
   // d[v] = δ(v);  (p[v], v) is the Bellman edge on the shortest path
```

This runs for exactly $n-1$ iterations (one vertex moved into $S$ per iteration, $s$ already there).

### 2.5 Complexity analysis

**(a) Naive array-based implementation:**
- Step 2a (`Find-Min` by linear scan) costs $(n-1)+(n-2)+\cdots+1 = O(n^2)$ **total** across all iterations.
- Step 2b (`Move to S`) is $O(1)$ per iteration → $O(n)$ total.
- Step 2c (relax neighbors): summed across *all* iterations, each edge is examined exactly once (from its tail vertex's turn) → $\sum_{v} \text{outdeg}(v) = O(m)$ total.

$$\boxed{T(n) = O(n^2 + m)}$$

**(b) Heap-based implementation:**
Maintain $\{d[u] \mid u \in V-S\}$ in a **min-heap**.
- `Find-Min` + `Delete-Min`: $O(\log n)$ each, done $n$ times → $O(n \log n)$
- Every relaxation that actually **decreases** a $d[u]$ value is a `Decrease-Key` call, $O(\log n)$ each, and there are at most $m$ relaxations (one per edge) → $O(m \log n)$

$$T(n) = O(n\log n + m \log n)$$

**(c) Comparing the two — when to use which:**

| Graph density | Best implementation | Why |
|---|---|---|
| **Sparse** ($m = O(n)$) | Heap-based | $O(n^2+m) \to O(n^2)$ vs. $O(n\log n + m\log n) \to O(n\log n)$ — heap wins by a big margin |
| **Dense** ($m = O(n^2)$) | Array-based | $O(n^2+m)\to O(n^2)$ vs. $O(n\log n + m\log n)\to O(n^2\log n)$ — array wins (heap's extra $\log n$ factor on $m$ hurts) |

**(d) Fibonacci-heap implementation (the theoretically optimal one):**
A Fibonacci heap supports `Decrease-Key` in **amortized $O(1)$** (only `Delete-Min` costs $O(\log n)$). Since there are $n$ Delete-Mins and up to $m$ Decrease-Keys:
$$\boxed{T(n) = O(n\log n + m)}$$
This dominates both prior bounds for **all** graph densities — it is never worse, and strictly better for dense graphs — which is why Fibonacci heaps are the "textbook optimal" choice for Dijkstra, even though they're rarely implemented in practice due to large constant factors.

### 2.6 Summary of all three complexities
$$O(n^2+m) \;\;(\text{array}) \qquad O(n\log n + m\log n)\;\;(\text{binary heap}) \qquad O(n\log n+m)\;\;(\text{Fibonacci heap})$$

---

## 3. Bellman-Ford Algorithm

### 3.1 Why we need it
Dijkstra's greedy proof (§2.3) *requires* $w(e) > 0$. If negative edges exist, a "finalized" vertex might later be improved by a path through an edge we haven't seen yet — greediness breaks. **Bellman-Ford solves the Single-Source Shortest Path (SSSP) problem for graphs with negative edge weights** (as long as there's no negative-weight cycle reachable from the source), using a completely different strategy: **the method of iterated improvements**, rather than greedy finalization.

### 3.2 Core idea — relaxation / the "improvement step"
For every edge $(u,v)$ with weight $w(u,v)$, define the **relax / improvement step**:
$$\text{if } d[v] > d[u] + w(u,v):\quad d[v] \leftarrow d[u] + w(u,v),\quad p[v] \leftarrow u$$
This says: *"if going through $u$ gives a shorter walk to $v$ than what we currently know, update it."* Unlike Dijkstra, we don't need $u$ to be "finalized" — relaxation is always safe, it can never make $d[v]$ incorrect, only pull it closer to the truth (this is the key invariant: $d[v]$ is always the weight of *some* real walk from $s$ to $v$, and relaxation never increases it).

A **Bellman edge** for $v$ is simply $(p[v], v)$ — the last edge on the current best-known walk to $v$.

### 3.3 The Bellman-Ford Equation
Let $\delta_k(v)$ = the weight of the shortest walk from $s$ to $v$ using **at most $k$ edges**. Then:
$$\delta_k(v) = \min\Big(\delta_{k-1}(v),\;\; \min_{(u,v)\in E}\big[\delta_{k-1}(u) + w(u,v)\big]\Big)$$
with base case $\delta_0(s) = 0$, $\delta_0(v) = \infty$ for $v \ne s$.

**Why $n-1$ iterations suffice:** in a graph with no negative cycle, any *simple* shortest path visits at most $n-1$ edges (since a simple path on $n$ vertices has at most $n-1$ edges). So after $n-1$ full rounds of relaxing every edge, $d[v] = \delta_{n-1}(v) = \delta(v)$ for all $v$.

### 3.4 Algorithm — "Method of Iterated Improvements"
```
BELLMAN-FORD(G = (V, E, w, s)):
1. Initialize d[s] = 0, d[v] = ∞ for v ≠ s;  p[v] = ⊥ for all v
2. repeat (n − 1) times:
      for every edge (u, v) in E (in a fixed order):
          if d[v] > d[u] + w(u, v):
              d[v] = d[u] + w(u, v)
              p[v] = u
3. // optional (n-th pass) — negative cycle check:
   for every edge (u, v) in E:
       if d[v] > d[u] + w(u, v):
           report "negative-weight cycle reachable from s"
4. return d[], p[]
```
Each **outer pass** is called a *stage*; the array after stage $k$ is conventionally denoted $d_k[\cdot]$ (so $d_0$ is the initialization, $d_1$ after one full pass over all edges, etc.).

### 3.5 Complexity
Each stage relaxes every edge once: $O(m)$ per stage, $n-1$ stages:
$$\boxed{T(n) = O(n \cdot m)}$$
(Compare: Dijkstra is $O(n^2+m)$ or better — Bellman-Ford is slower, but it tolerates negative weights, which Dijkstra cannot.)

### 3.6 Negative-weight cycles ("zero cycle")
If the graph contains a cycle whose total edge weight is **negative**, then shortest paths through vertices on that cycle are not well-defined — you can keep going around the cycle to make the "path" arbitrarily cheap (arbitrarily negative). This is why Bellman-Ford includes step 3: run **one extra ($n$-th) relaxation pass** — if *any* edge can still be relaxed after $n-1$ passes have already converged for a cycle-free graph, that proves a negative cycle exists reachable from $s$.

A **zero-weight cycle** (total weight exactly 0) is not a correctness problem (paths don't get cheaper by looping through it), but it does mean multiple distinct shortest paths of equal weight can exist.

### 3.7 Worked Example (from your quiz graph)

**Graph:** vertices $1,\dots,8$, source $=1$. Edges relaxed in this **fixed order**:
$$(1,2),\ (2,3),\ (3,4),\ (4,5),\ (5,6),\ (6,7),\ (7,5),\ (6,8)$$
Vertices $5,6,7$ sit on a small triangle with weight $-1$ edges — this triangle forms a **negative-weight cycle** ($5 \to 6 \to 7 \to 5$), while the "backbone" $1\to2\to3\to4\to5$ and the tail $6 \to 8$ carry weight $+1$ edges.

**$d_0[\;]$ (before any relaxation — initialization):**
$$[\,0,\ \infty,\ \infty,\ \infty,\ \infty,\ \infty,\ \infty,\ \infty\,]$$
Only the source is $0$; every edge relaxation is still to come.

**$d_1[\;]$ (after 1 full pass over all 8 edges, in the fixed order):**
$$[\,0,\ 1,\ 2,\ 3,\ 1,\ 3,\ 2,\ 4\,]$$
Walking through the pass: relaxing $(1,2),(2,3),(3,4)$ propagates $0\to1\to2\to3$ along the backbone exactly like Dijkstra would. Then $(4,5),(5,6),(6,7)$ push a value out along the triangle, and — crucially — because $(7,5)$ is relaxed **later in the same pass**, it immediately pulls $d[5]$ *down* using the just-computed (already improved, negative-influenced) value at $7$. This single-pass "domino" effect is only possible because Bellman-Ford relaxes **all** edges every round, not just edges out of "finalized" vertices.

**$d_2[\;]$ (after 2 passes) — accepted quiz answer:**
$$[\,0,\ 1,\ 2,\ 3,\ -2,\ 0,\ -1,\ 1\,]$$
Notice $d[5], d[6], d[7]$ have all dropped further (by exactly $-3$ each, i.e. the weight of one full trip around the negative triangle $5\to6\to7\to5 = -1-1-1$... conceptually, the cycle continues shaving weight off every extra pass).

**$d_3[\;]$ (after 3 passes) — accepted quiz answer:**
$$[\,0,\ 1,\ 2,\ 3,\ -3,\ -1,\ -2,\ 0\,]$$

**What this demonstrates:** because vertices $5,6,7$ lie on a reachable negative-weight cycle, their $d[\cdot]$ values **never converge** — they keep decreasing with every extra pass, forever. This is *exactly* the signature Bellman-Ford's step-3 check is designed to catch: if relaxation is still possible after $n-1=7$ passes, a negative cycle reachable from the source exists, and "shortest path" is undefined for the vertices on/downstream of it.

---

## 4. How Dijkstra & Bellman-Ford Relate

| | **Dijkstra** | **Bellman-Ford** |
|---|---|---|
| Edge weights | Must be $w(e) > 0$ | Can be negative (no negative cycle) |
| Strategy | Greedy — finalize min-$d$ vertex each round | Iterated improvement — relax *all* edges, repeatedly |
| Core structure | $S$ / $V-S$ partition + special path | $\delta_k(v)$: shortest walk using $\le k$ edges |
| Rounds | $n-1$ (finalize 1 vertex/round) | $n-1$ full edge-relaxation passes (+1 to detect neg. cycles) |
| Complexity | $O(n^2+m)$ / $O(n\log n+m\log n)$ / $O(n\log n+m)$ (Fib. heap) | $O(nm)$ |
| Fails when | Negative edge present | Negative cycle reachable from source (no valid shortest path) |
| Common output notion | $(p[v], v)$ = **Bellman edge**, $d[v]=\delta(v)$ | Same notion of Bellman edge / relaxation, generalized |

Both algorithms compute the **same thing** (SSSP distances $\delta(v)$ and a shortest-path tree via predecessor pointers $p[v]$) using the identical *relaxation* primitive — they differ only in **which edges they're allowed to relax, and in what order**, which is precisely what negative weights force you to change.

---

## 5. Quiz — Compiled Questions with Explanations

### Heap Questions

**Q1. Insert(3) into $H$ (7-node heap: 1(5), 2(10), 3(15), 4(25), 5(20), 6(35), 7(30)). What is the value at the node with index 8?**

> **Answer: 25**
> Insert places 3 at index 8 (leaf), then bubbles up: swaps with idx4(25) → idx8=25; swaps with idx2(10) → idx4=10; swaps with idx1(5) → idx2=5. Index 8 ends up holding the value that was originally displaced from index 4, which is **25**. See full working in §1.5.

**Q2. After Insert(3) in $H$, at which index does the original root value (5) end up?**

> **Answer: index 2**
> As the new value 3 bubbles from leaf to root, each ancestor on that path gets pushed one level down: idx4(25)→idx8, idx2(10)→idx4, idx1(5)→idx2. So the old root value 5 lands at **index 2**.

**Q3. On $H'$ (the heap after Insert(3)), Decrease-Key is done at node index 6 (35 → 2). What is the value at node index 6 after this operation?**

> **Answer: 15**
> idx6 is set to 2, then bubbles up: swaps with parent idx3(15) → idx6 becomes 15, idx3 becomes 2; then idx3(2) swaps with parent idx1(3) → idx3=3, idx1=2. So after the operation, index 6 holds the value that was displaced from index 3, which is **15**. Full working in §1.5.

**Q4. After the same Decrease-Key operation, what value is at the root?**

> **Answer: 2**
> The decreased value (2) is smaller than everything on its bubble-up path (15, then 3), so it rises all the way to the root. **Root = 2.**

---

### Dijkstra's Algorithm Questions

**Q5. What are the two "quantities" every vertex $v \in V-S$ maintains during Dijkstra, and what do they represent?**

> **Answer:** $d[v]$ — the weight of the shortest **special path** (a path from $s$ to $v$ where every vertex except $v$ lies in $S$) found *so far*; and $p[v]$ — the predecessor vertex on that path, giving the **Bellman edge** $(p[v], v)$. When $v$ is finally moved into $S$, $d[v]$ is proven equal to $\delta(v)$, the true shortest-path distance (§2.3).

**Q6. Why does Dijkstra's correctness proof fail if some edge has negative weight?**

> **Answer:** The proof (§2.3) relies on the step $w(Q[s,v]) = w(Q[s,u]) + w(Q[u,v]) > w(Q[s,u])$ — this strict inequality only holds because $w(Q[u,v]) > 0$. If a later edge on the true shortest path could have negative weight, a path that looks "longer so far" when it crosses into $V-S$ could still end up cheaper overall, so the greedy "always finalize the current minimum" rule can pick the wrong vertex.

**Q7. State the property of the min-$d$ vertex $v$ chosen at each round of Dijkstra, in terms of the "special path" set-up.**

> **Answer:** $v$ is the vertex in $V-S$ with $d[v] \le d[u]$ for every $u \in V-S$ (property of $v$, used directly in the contradiction proof — it's what lets us conclude $d[u] \ge d[v]$ for the crossing vertex $u$ on any competing path $Q$).

**Q8. Give the complexity of Dijkstra's algorithm using (a) an array, (b) a binary heap, (c) a Fibonacci heap.**

> **Answer:**
> (a) Array-based: $O(n^2 + m)$
> (b) Binary-heap-based: $O(n\log n + m\log n)$
> (c) Fibonacci-heap-based: $O(n\log n + m)$ — best overall, since Decrease-Key is amortized $O(1)$.

**Q9. For a sparse graph ($m = O(n)$), which implementation of Dijkstra is faster — array or heap? What about a dense graph ($m = O(n^2)$)?**

> **Answer:** Sparse → **heap-based** is faster ($O(n\log n)$ vs $O(n^2)$). Dense → **array-based** is faster ($O(n^2)$ vs $O(n^2\log n)$), because the extra $\log n$ multiplying $m$ in the heap bound hurts once $m$ is already $\Theta(n^2)$.

---

### Bellman-Ford Algorithm Questions

**Q10. Execute Bellman-Ford on the given graph (source = vertex 1), relaxing edges in the fixed order $(1,2),(2,3),(3,4),(4,5),(5,6),(6,7),(7,5),(6,8)$. What is $d_0[\;]$, the array before any relaxation?**

> **Answer:** $[0, \infty, \infty, \infty, \infty, \infty, \infty, \infty]$ — only the source starts at 0; nothing else has been reached yet.

**Q11. What is $d_1[\;]$, the array after one full pass over all edges (in the fixed order above)?**

> **Answer:** $[0, 1, 2, 3, 1, 3, 2, 4]$. The backbone $1\to2\to3\to4\to5$ propagates distances $0,1,2,3$ normally; then the triangle edges $(5,6),(6,7),(7,5)$ get relaxed *within the same pass*, and because $(7,5)$ comes after $(6,7)$ in the fixed order, $d[5]$ gets an immediate second (lower) update using the newly-computed $d[7]$. See §3.7 for the full walkthrough.

**Q12. What is $d_2[\;]$ and $d_3[\;]$? What do you observe, and what does it indicate about the graph?**

> **Answer:** $d_2 = [0,1,2,3,-2,0,-1,1]$, $d_3 = [0,1,2,3,-3,-1,-2,0]$. The values at vertices $5, 6, 7$ keep **decreasing without bound** across successive passes. This is the signature of a **negative-weight cycle** reachable from the source — here, the triangle $5 \to 6 \to 7 \to 5$ has total weight $-1$ (or similar net-negative weight), so shortest-path distances through it are undefined; every extra lap around the cycle makes the "path" cheaper. This is exactly what Bellman-Ford's optional $n$-th relaxation pass is designed to detect.

**Q13. What is the time complexity of the Bellman-Ford algorithm, and why?**

> **Answer:** $O(nm)$. Each of the $n-1$ stages relaxes every one of the $m$ edges once, giving $O(n\cdot m)$; an optional $n$-th pass for negative-cycle detection doesn't change the asymptotic bound.

**Q14. Why does Bellman-Ford need at most $n-1$ stages (in the absence of a negative cycle)?**

> **Answer:** In a graph with no negative cycle, any shortest path is simple and therefore uses at most $n-1$ edges (a simple path on $n$ vertices has $\le n-1$ edges). By the Bellman-Ford equation $\delta_k(v) = \min(\delta_{k-1}(v), \min_{(u,v)\in E}[\delta_{k-1}(u)+w(u,v)])$, after $k$ passes $d[\cdot]$ equals $\delta_k(\cdot)$; once $k = n-1$, this equals the true $\delta(\cdot)$ for every vertex.

---

## Quick-Reference Cheat Sheet

| Concept | One-liner |
|---|---|
| Heap insert | New leaf, bubble up — $O(\log n)$ |
| Heap decrease-key | Lower value in place, bubble up — $O(\log n)$ |
| Dijkstra requires | $w(e) > 0$ for **all** edges |
| Dijkstra's greedy step | Move min-$d$ vertex from $V-S$ to $S$ |
| Special path | Path $s\to v$ where every vertex but $v$ is in $S$ |
| Bellman edge | $(p[v], v)$ — last edge on current best path to $v$ |
| Dijkstra complexity (best) | $O(n\log n + m)$ with Fibonacci heap |
| Bellman-Ford handles | Negative edges (not negative **cycles**) |
| Bellman-Ford relaxation | $d[v] > d[u]+w(u,v) \Rightarrow$ update |
| Bellman-Ford stages | $n-1$ (+1 optional, for negative-cycle detection) |
| Bellman-Ford complexity | $O(nm)$ |
| Negative cycle signature | $d[\cdot]$ keeps shrinking past $n-1$ passes |
