# Bellman Equations for Shortest Paths — Advanced Notes

*(Compiled from NPTEL lecture notes + quiz problem set)*

---

## 1. Setup and Notation

We work with a weighted directed graph:

```
G = (V, E, W, s)
```

- `V` — set of vertices
- `E` — set of directed edges
- `W` — weight function, `w(u,v)` = weight of edge `(u,v)`
- `s` — a fixed **source vertex**

**δ(v)** := weight of the shortest path (or shortest walk, once we clarify the distinction below) from `s` to `v`.

**Standing assumption (initially):** `G` has **no negative-weight cycles** reachable from `s`. Without this assumption, "shortest path" is not even well-defined (you could go around a negative cycle forever and make the "distance" `-∞`).

### 1.1 Walks vs. Paths vs. Trails vs. Closed Walks

This distinction matters a lot once we talk about cycles, so nail it down precisely:

| Object | Vertices repeat? | Edges repeat? | Starts = Ends? |
|---|---|---|---|
| **Walk** | Allowed | Allowed | Not required |
| **Trail** | Allowed | Not allowed | Not required |
| **Path** | Not allowed | Not allowed | Not required |
| **Closed walk** | Allowed | Allowed | **Yes** |
| **Cycle** | Not allowed (except start=end) | Not allowed | **Yes** |

- A **walk** is simply an alternating sequence `v0, e1, v1, e2, v2, ..., ek, vk` where each `ei = (v_{i-1}, v_i) ∈ E`. Nothing stops you from visiting the same vertex or traversing the same edge twice.
- A **path** is a walk with all distinct vertices (hence all distinct edges too).
- The **weight of a walk/path** is the sum of the weights of its edges.
- A **closed walk** is a walk where `v0 = vk` (you return to where you started). A cycle is the "simple" (non-repeating) special case of a closed walk.

### 1.2 Why the Walk/Path Distinction Matters

`δ(v)` is officially defined as the shortest-**path** weight. But the Bellman recursion (below) naturally computes something more like a shortest-**walk** weight. The entire theory of "when do these two coincide?" boils down to what kind of cycles exist in `G`:

- **If `G` has no negative cycles and no zero-weight cycles:** shortest walk = shortest path, always. (Any repetition of a vertex only adds cost, so the optimal walk never needs to repeat a vertex — it is automatically a simple path.)
- **If `G` has a zero cycle:** the shortest *walk* can loop around the zero cycle any number of times without changing its weight → **infinitely many walks achieve the same optimal value**, but the shortest **path** is still unique. This is exactly why the Bellman Equations can have **infinitely many solutions** when a zero cycle exists (see §4).
- **If `G` has a negative cycle:** the shortest walk weight is `-∞` for every vertex reachable from that cycle (you can loop forever, reducing weight without bound) — so no shortest path/walk value exists at all. This is why the Bellman Equations have **no solution** in this case (see §3).

This is the conceptual bridge that connects "shortest walk," "closed walk," "negative cycle," and "zero cycle" into one coherent story.

---

## 2. The Bellman Equations (BE)

For each vertex `v ∈ V`, introduce a variable `x_v`. The **Bellman Equations** are:

```
x_s = 0
x_v = min_{u ≠ v} { x_u + w(u,v) }      (minimum taken over all edges (u,v) ∈ E)
```

**Key structural fact:** The number of terms inside the `min{...}` for vertex `v` equals the **in-degree of `v`** — one term per incoming edge. If there are **parallel edges** (multiple edges from the same `u` to the same `v`, possibly with different weights), each parallel edge contributes its *own* term to the min — you don't collapse them into one term for "vertex `u`"; you take the min over **edges**, not over source-vertices. This distinction is exactly what Quiz Questions 4–7 test (see §7).

If `x_v = δ(v)` for every `v`, we say `{x_v}` is **the solution** of the Bellman Equations (assuming this solution is meaningful/unique — see §4).

### 2.1 Worked Example (from lecture)

Graph (4 vertices `s, a, b, c` with directed edges, including a diagonal structure):

```
Edges & weights:
s → a : 1
s → b : 4      (via c, see below — read directly off diagram)
a → b : 2
c → a : 5
b → c : 3
c → ...
```

The Bellman Equations set up as:

```
x_s = 0
x_a = min{ x_s + 1,  x_c + 5 }
x_b = min{ 6, x_a + 2 }
x_c = x_b + 3
```

**Substituting step by step (eliminating variables):**

```
x_c = x_b + 3
⇒ x_a = min{1, x_c + 5} = min{1, x_b + 3 + 5} = min{1, x_b + 8}
```

Now try assuming `x_a = x_b + 8` (i.e., the second term is active):

```
x_b = min{6, x_a + 2} = min{6, x_b + 8 + 2} = min{6, x_b + 10}
```

This is a self-referential equation in `x_b` alone — solving it consistently:

- If `x_b = 6`: then `x_a = x_b + 8 = 14`. But then check: `x_a = min{1, x_b+8} = min{1,14}` should equal `1`, not `14`. **Contradiction.**
- So `x_a` must actually equal `1` (the *other* branch of its min is the active one).
- Then `x_b = min{6, x_a+2} = min{6,3} = 3`
- Then `x_c = x_b + 3 = 6`

**Final unique solution:**

```
x_s = 0,  x_a = 1,  x_b = 3,  x_c = 6
δ(s)=0,   δ(a)=1,   δ(b)=3,   δ(c)=6
```

This matches shortest-path distances computed directly from the graph — confirming the BE gives the correct answer **when the graph has no negative or zero cycles**.

---

## 3. Negative Cycles → Bellman Equations Have NO Solution

Modify the same graph so that the cycle `a → b → c → a` becomes a **negative cycle** (e.g., weights `1, 2, 3, -7` around the loop instead of `1,2,3,5`, making the total loop weight negative).

Setting up the equations the same way:

```
x_a = min{1, x_b - 4}
x_b = min{6, x_a + 2}
```

Try `x_a = 1`:
```
1 ≤ x_b - 4  ⇒  x_b ≥ 5
but x_b = min{6, x_a+2} = min{6,3} = 3
```
**Contradiction** (`3 ≥ 5` is false).

Try the other branch, `x_a = x_b - 4`:
```
x_b = min{6, x_a+2} = min{6, x_b-4+2} = min{6, x_b-2}
```
This forces `x_b = 6`... but plugging back in gives `x_a = 2`, which again fails the consistency check against the original min.

**No assignment of values satisfies all equations simultaneously.** This is the general theorem:

> **If `G` contains a negative-weight cycle (reachable from `s`), the Bellman Equations have NO solution.**

*Intuition:* around a negative cycle, `δ(v) = -∞` for every vertex on/after the cycle — but `-∞` is not a real number that can appear in a legitimate solution, so the system of equations is simply inconsistent.

---

## 4. Zero Cycles → Bellman Equations Have INFINITELY MANY Solutions (Non-Unique)

Now suppose the cycle `a → b → c → a` has total weight **exactly 0** (a "zero cycle") — e.g., weights `1, 2, 3, -6`.

The equations turn out to admit **multiple, genuinely different** solution sets:

```
Solution 1:  x_s=0, x_a=1,  x_b=3, x_c=6
Solution 2:  x_s=0, x_a=0,  x_b=2, x_c=5
Solution 3:  x_s=0, x_a=-2, x_b=0, x_c=3
Solution 4:  x_s=0, x_a=-7, x_b=-5,x_c=-2
... (infinitely many, differing by a constant shift around the zero cycle)
```

Each of these independently satisfies every Bellman Equation exactly — plug any one in and every equation checks out. Notice: consecutive solutions differ by a **constant offset that is consistent with looping around the zero cycle** (adding/subtracting 0 doesn't change total weight, but *where* you "bank" that zero can shift each individual `x_v`).

> **If `G` contains a zero-weight cycle, the Bellman Equations may have infinitely many solutions, and the solution is NOT unique.**

This connects directly back to §1.2: a zero cycle means you can insert extra loops into a walk "for free," so many different **walks** achieve the same minimum weight — hence many different consistent variable-assignments satisfy the recursive equations, even though the *actual shortest simple path* distances are still well-defined and unique.

---

## 5. Uniqueness Theorem (The Main Result)

Putting §3 and §4 together, we get the central theorem of this topic:

> **Assume `G` has no negative cycles and no zero cycles** (i.e., every cycle in `G` has strictly positive weight). Then:
> 1. The Bellman Equations have a solution.
> 2. That solution is **unique**.
> 3. The unique solution is exactly `x_v = δ(v)` for every `v ∈ V` — i.e., it recovers the true shortest-path distances.

**Why uniqueness fails only in these two cases:** Every other possible configuration (only positive cycles, or no cycles at all / a DAG) forces the system of equations into a single consistent assignment, by the same substitution/contradiction technique shown in §2.1 and §3.

### Summary Table

| Cycle type in G | Solution of Bellman Equations |
|---|---|
| No negative or zero cycles (only positive cycles / DAG) | **Unique** solution, equals `δ(v)` |
| Contains a negative cycle | **No** solution exists |
| Contains a zero cycle (no negative cycle) | **Infinitely many** solutions; not unique |

This is why, going forward, we always work under the blanket assumption:

```
G has no negative cycles, AND no zero cycles.
```

so that the Single-Source Shortest Path (SSSP) problem is well-posed and the Bellman Equations characterize it exactly.

---

## 6. The Catch: No Direct Method to *Solve* the Bellman Equations

Even once we know the solution is unique (given the no-negative/no-zero-cycle assumption), the Bellman Equations themselves are just a *characterization* — a system of `min` recurrences — not an algorithm. There is **no direct closed-form method** to solve this system as written (notice in §2.1 we had to do manual case-splitting/substitution — that doesn't scale to large graphs).

This motivates the next step in the course: an **iterative/relaxation-based algorithm** (Bellman–Ford) that computes the solution constructively rather than by solving simultaneous equations. That leads into the idea of **bounded walks**, covered next.

---

## 7. Bellman Equations for Bounded Walks (k-Edge Shortest Walks)

To *algorithmically* solve the Bellman Equations, we introduce a **bounded** (or "iterated") version of `x_v`, indexed by the maximum number of edges allowed in the walk:

```
x_v^(k) := weight of the shortest walk from s to v using AT MOST k edges
```

**Base case (k = 0):**
```
x_s^(0) = 0
x_v^(0) = +∞     for all v ≠ s   (no walk of 0 edges reaches any other vertex)
```

**Recursive case (k ≥ 1):**
```
x_v^(k) = min{  x_v^(k-1),                      -- don't use the k-th edge at all
                min_{(u,v)∈E} { x_u^(k-1) + w(u,v) }  }   -- use one more edge into v
```

i.e. `x_v^(k)` is the minimum over:
- the best walk found so far with fewer edges (`x_v^(k-1)`), and
- extending some `(k-1)`-edge walk into a neighbor `u` by one more edge `(u,v)`.

### 7.1 Why This Terminates and Matches δ(v)

- Since `G` has `|V|` vertices and no negative cycles, **any shortest path has at most `|V|-1` edges** (a simple path can't repeat a vertex, so it uses at most `|V|-1` edges).
- Therefore: `x_v^(|V|-1) = δ(v)` for every vertex `v` — the bounded-walk recursion **converges to the true shortest-path distance** after at most `|V|-1` rounds.
- This iterative recursion **is** the Bellman–Ford algorithm in disguise: each round `k` is one "relaxation pass" over all edges.

### 7.2 Bounded Walks and Closed Walks — Why "Walk" and Not "Path" in the Recursion

Note carefully: `x_v^(k)` is defined as the shortest **walk** (not necessarily simple path) using at most `k` edges — this is a deliberate relaxation that makes the DP well-defined even before we know whether zero/negative cycles exist. Key consequences:

- If `G` has **no negative cycles**, extending a walk around any cycle (closed walk) never decreases its weight below the best simple path, so for `k ≥ |V|-1`, the shortest bounded walk **coincides** with the shortest path — `x_v^(k)` stabilizes and stops changing for larger `k`.
- If `G` **has a negative cycle** reachable from `s`, then `x_v^(k) → -∞` as `k → ∞`, since you can always take one more trip around the negative closed walk to strictly decrease the total weight. This is the standard basis for **detecting negative cycles**: if values are still decreasing after `|V|-1` rounds, a negative cycle exists.
- This is precisely the walk-vs-path subtlety from §1.2, now made computational: the bounded-walk recursion is allowed to consider closed walks as sub-structures, and whether that helps or hurts (or does nothing) tells you which of the three cycle regimes (positive / zero / negative) you're in.

---

## 8. Multi-Edges in the Bellman Equation (Parallel Edges)

A subtlety that is easy to get wrong: if there are **multiple distinct edges** from `u` to `v` (e.g., a "direct" edge `s → a` of weight 5 *and* a separate direct edge `s → a` of weight 1), the Bellman equation for `x_a` must include **one term per edge**, not one term per distinct source vertex:

```
x_a = min{ x_s + 5,    -- via the first s→a edge
           x_s + 1 }   -- via the second s→a edge
    = min{5, 1} = 1
```

You always keep the cheapest of the parallel options — this is just the ordinary min-relaxation, but it's worth stating explicitly because it's exactly what several quiz questions below are testing.

---

## 9. Quiz Questions with Full Explanations

### Group A — Straight-Line Graph: `s -5→ a -10→ b -(-5)→ c -(-10)→ d`

All three questions below use the same underlying path graph:

```
s --5--> a --10--> b --(-5)--> c --(-10)--> d
```

**Q1. Write the Bellman equation and solution for `x_a`.**
- Since `a` has exactly one incoming edge (from `s`, weight 5):
  `x_a = x_s + 5`
- `x_s = 0` ⇒ `x_a = 0 + 5 = 5`
- ✅ **Correct answer: `(x_s + 5)` and `5`**

**Q2. Write the Bellman equation and solution for `x_b`.**
- `b` has one incoming edge from `a`, weight 10:
  `x_b = x_a + 10`
- `x_a = 5` (from Q1) ⇒ `x_b = 5 + 10 = 15`
- ✅ **Correct answer: `(x_a + 10)` and `15`**

**Q3. Write the Bellman equation and solution for `x_c`.**
- `c` has one incoming edge from `b`, weight **−5**:
  `x_c = x_b − 5`
- `x_b = 15` (from Q2) ⇒ `x_c = 15 − 5 = 10`
- ✅ **Correct answer: `(x_b − 5)` and `10`**

*(Extending the pattern for completeness, though not asked: `x_d = x_c − 10 = 10 − 10 = 0`.)*

**Takeaway for Group A:** on a simple directed path (no branching, no cycles), the Bellman equation at each vertex has exactly **one term** (in-degree 1), and solving it is just a straightforward chain of additions — this is the simplest possible case of the general recursion in §2.

---

### Group B — Path Graph *Plus* Direct Multi-Edges from `s` (Q4–Q7)

Underlying graph for all four questions:

```
Main path:      s --5--> a --10--> b --5--> c --10--> d
Extra edges:    s --1--> a   (direct)
                s --1--> b   (direct)
                s --1--> c   (direct)
                s --1--> d   (direct)
```

Each vertex `a, b, c, d` therefore has **two incoming edges**: one along the main path, and one direct "shortcut" edge straight from `s`, each shortcut weighing exactly `1`. This is a direct application of §8 (multi-source min, taken over **edges**, not vertices) — except here it's really "two different paths into the same vertex," which is the same min-over-incoming-edges logic.

**Q4. Solution of the Bellman equation for `x_a`.**
```
x_a = min{ x_s + 5,   -- via main path edge s→a
           x_s + 1 }  -- via direct shortcut s→a
    = min{0+5, 0+1} = min{5, 1} = 1
```
✅ **Correct answer: `1`**

**Q5. Solution of the Bellman equation for `x_b`.**
```
x_b = min{ x_a + 10,  -- via main path edge a→b
           x_s + 1 }  -- via direct shortcut s→b
    = min{1+10, 0+1} = min{11, 1} = 1
```
(using `x_a = 1` from Q4)
✅ **Correct answer: `1`**

**Q6. Solution of the Bellman equation for `x_c`.**
```
x_c = min{ x_b + 5,   -- via main path edge b→c
           x_s + 1 }  -- via direct shortcut s→c
    = min{1+5, 0+1} = min{6, 1} = 1
```
(using `x_b = 1` from Q5)
✅ **Correct answer: `1`**

**Q7. Solution of the Bellman equation for `x_d`.**
```
x_d = min{ x_c + 10,  -- via main path edge c→d
           x_s + 1 }  -- via direct shortcut s→d
    = min{1+10, 0+1} = min{11, 1} = 1
```
(using `x_c = 1` from Q6)
✅ **Correct answer: `1`**

**Takeaway for Group B:** every single vertex ends up with `x_v = 1`, because the direct shortcut edge (weight 1) always beats the accumulated main-path cost once the main path has any positive edges in it. This exercise is specifically designed to test whether you correctly (a) identify **all** incoming edges into a vertex (in-degree = number of terms in the min, per §2), and (b) correctly take the **minimum**, not the sum or the main-path-only value — a common mistake is to forget the direct shortcut term entirely and only propagate along the "obvious" path.

---

## 10. Quick-Reference Cheat Sheet

```
Bellman Equations:
    x_s = 0
    x_v = min_{(u,v)∈E} { x_u + w(u,v) }     [one term per INCOMING EDGE, not per vertex]

Uniqueness / Existence:
    No negative cycle, no zero cycle  →  UNIQUE solution = δ(v)  ✅
    Negative cycle present            →  NO solution              ❌
    Zero cycle present (no neg.)      →  INFINITELY MANY solutions ⚠️ (not unique)

Bounded-walk recursion (constructive algorithm, → Bellman-Ford):
    x_v^(0) = 0 if v=s, else +∞
    x_v^(k) = min{ x_v^(k-1),  min_{(u,v)∈E}{ x_u^(k-1) + w(u,v) } }
    x_v^(|V|-1) = δ(v)   (guaranteed, if no negative cycle)
    Negative cycle detection: values keep decreasing past round |V|-1

Walks vs Paths:
    Walk  = vertices/edges may repeat
    Path  = no repeats
    Closed walk = walk that returns to start (repeats allowed)
    Shortest walk = shortest path  ⇔  no negative or zero cycles
```
