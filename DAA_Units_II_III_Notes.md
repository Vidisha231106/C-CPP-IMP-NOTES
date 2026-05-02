# Design and Analysis of Algorithms
## Units II & III — Complete Study Notes
### Topics: Decrease & Conquer | Transform & Conquer | Space-Time Trade-offs

---

> **How to use these notes:** Read top to bottom for first pass. Before an exam, jump to the **"Exam Snapshot"** boxes at the end of each major section. Every algorithm has code in C++, full complexity analysis, and worked examples.

---

# TABLE OF CONTENTS

1. [Decrease and Conquer — Core Idea](#1-decrease-and-conquer--core-idea)
2. [Insertion Sort](#2-insertion-sort)
3. [Depth First Search (DFS)](#3-depth-first-search-dfs)
4. [Breadth First Search (BFS)](#4-breadth-first-search-bfs)
5. [Topological Sorting](#5-topological-sorting)
6. [Applications of DFS and BFS](#6-applications-of-dfs-and-bfs)
7. [Transform and Conquer — Core Idea](#7-transform-and-conquer--core-idea)
8. [Presorting](#8-presorting)
9. [Heapsort](#9-heapsort)
10. [Problem Reduction](#10-problem-reduction)
11. [Space and Time Trade-offs — Core Idea](#11-space-and-time-trade-offs--core-idea)
12. [Sorting by Counting](#12-sorting-by-counting)
13. [Naive String Matching](#13-naive-string-matching)
14. [Input Enhancement: Horspool's Algorithm](#14-input-enhancement-horspools-algorithm)
15. [Input Enhancement: Boyer-Moore Algorithm](#15-input-enhancement-boyer-moore-algorithm)
16. [Complexity Master Table](#16-complexity-master-table)
17. [LeetCode Practice Problems](#17-leetcode-practice-problems)

---

# 1. Decrease and Conquer — Core Idea

## What Is It?

Decrease and Conquer is an algorithmic paradigm where you **reduce a problem of size n to a problem of size (n-1)** (or some smaller size), solve the smaller problem, and then use that solution to solve the original.

**Key difference from Divide and Conquer:**
- **Divide & Conquer** → splits into multiple subproblems (e.g., Merge Sort splits into 2 halves)
- **Decrease & Conquer** → reduces to ONE smaller subproblem

## Three Variants

| Variant | Description | Example |
|---|---|---|
| Decrease by a constant | Reduce size by 1 each time | Insertion Sort, DFS/BFS |
| Decrease by a constant factor | Reduce by half each time | Binary Search |
| Variable-size decrease | Reduction varies | Euclid's GCD |

## Recursive Structure

```
solve(n):
    if n == base_case:
        return base_answer
    smaller = solve(n - 1)          // Solve smaller instance
    return extend(smaller, element) // Extend solution
```

---

# 2. Insertion Sort

## Conceptual Idea

Think of sorting playing cards. You hold a sorted hand and insert each new card into its correct position.

- The array is divided into a **sorted left part** and **unsorted right part**.
- At each step, take the **first element of the unsorted part** and insert it at its correct position in the sorted part.
- This is "decrease by 1" — after inserting element i, you have a sorted subarray of size i.

## Dry Run Example

Array: `[5, 3, 8, 1, 4]`

```
Pass 1: key=3  → [3, 5, 8, 1, 4]
Pass 2: key=8  → [3, 5, 8, 1, 4]  (no shift needed)
Pass 3: key=1  → [1, 3, 5, 8, 4]
Pass 4: key=4  → [1, 3, 4, 5, 8]
```

## Algorithm (Pseudocode)

```
InsertionSort(A[0..n-1]):
    for i = 1 to n-1:
        key = A[i]
        j = i - 1
        while j >= 0 and A[j] > key:
            A[j+1] = A[j]
            j = j - 1
        A[j+1] = key
```

## C++ Code

```cpp
#include <iostream>
#include <vector>
using namespace std;

void insertionSort(vector<int>& A) {
    int n = A.size();
    for (int i = 1; i < n; i++) {
        int key = A[i];
        int j = i - 1;
        // Shift elements greater than key one position right
        while (j >= 0 && A[j] > key) {
            A[j + 1] = A[j];
            j--;
        }
        A[j + 1] = key;
    }
}

int main() {
    vector<int> arr = {5, 3, 8, 1, 4};
    insertionSort(arr);
    for (int x : arr) cout << x << " ";
    // Output: 1 3 4 5 8
    return 0;
}
```

## Recurrence Relation

Let C(n) = number of comparisons in the worst case.

```
C(n) = C(n-1) + (n-1)         // Insert nth element takes at most (n-1) comparisons
C(1) = 0                       // Base case: 1 element, 0 comparisons

Expanding:
C(n) = C(n-2) + (n-2) + (n-1)
     = C(1) + 1 + 2 + ... + (n-1)
     = n(n-1)/2
```

## Complexity Analysis

| Case | Input | Comparisons | Time |
|---|---|---|---|
| **Best** | Already sorted | n-1 | O(n) |
| **Average** | Random | n²/4 | O(n²) |
| **Worst** | Reverse sorted | n(n-1)/2 | O(n²) |

- **Space Complexity:** O(1) — in-place algorithm
- **Stable:** Yes — equal elements maintain relative order
- **Adaptive:** Yes — runs faster on nearly sorted input

## Key Properties

1. **In-place:** No extra memory needed
2. **Online:** Can sort elements as they arrive
3. **Stable sort:** Does not disturb the order of equal elements
4. **Best for:** Small arrays (n ≤ 20), nearly sorted data, online sorting

## Comparison with Other O(n²) Sorts

| Algorithm | Best | Average | Worst | Stable? | In-place? |
|---|---|---|---|---|---|
| Insertion Sort | O(n) | O(n²) | O(n²) | Yes | Yes |
| Selection Sort | O(n²) | O(n²) | O(n²) | No | Yes |
| Bubble Sort | O(n) | O(n²) | O(n²) | Yes | Yes |

> **Exam tip:** Insertion sort is preferred over bubble sort in practice because it does far fewer writes (swaps). Each element is moved at most once per pass.

---

# 3. Depth First Search (DFS)

## Core Idea

DFS explores a graph by going **as deep as possible** along each branch before backtracking. Think of it as navigating a maze — always take the first available path, and when you hit a dead end, backtrack to the last junction and try the next path.

## Graph Representation

Before DFS/BFS, a graph G = (V, E) is stored as:

**Adjacency Matrix:** `A[i][j] = 1` if edge (i,j) exists. Space = O(V²)

**Adjacency List:** For each vertex, a list of its neighbors. Space = O(V + E)

```
Graph:
    1 -- 2
    |    |
    3 -- 4
    
Adjacency List:
1: [2, 3]
2: [1, 4]
3: [1, 4]
4: [2, 3]
```

## DFS Algorithm (Recursive)

```
DFS(G):
    mark all vertices as unvisited
    for each vertex v in G:
        if v is unvisited:
            DFS_Visit(v)

DFS_Visit(v):
    mark v as visited
    for each neighbor u of v:
        if u is unvisited:
            tree_edge(v, u)   // Record tree edge
            DFS_Visit(u)
    record finish time of v
```

## DFS Algorithm (Iterative using Stack)

```
DFS_Iterative(G, start):
    stack S
    push start onto S
    while S is not empty:
        v = S.pop()
        if v is not visited:
            mark v as visited
            for each neighbor u of v:
                if u is not visited:
                    push u onto S
```

## C++ Code (Recursive)

```cpp
#include <iostream>
#include <vector>
using namespace std;

int n; // number of vertices
vector<int> adj[100];
bool visited[100];
int discovery[100], finish[100], timer_val = 0;

void dfs(int v) {
    visited[v] = true;
    discovery[v] = ++timer_val;
    
    for (int u : adj[v]) {
        if (!visited[u]) {
            cout << "Tree edge: " << v << " -> " << u << endl;
            dfs(u);
        }
    }
    finish[v] = ++timer_val;
}

void DFS(int n) {
    fill(visited, visited + n + 1, false);
    for (int v = 1; v <= n; v++) {
        if (!visited[v]) {
            dfs(v);
        }
    }
}
```

## DFS Tree and Edge Classification

During DFS on a **directed** graph, edges are classified as:

| Edge Type | Description | When? |
|---|---|---|
| **Tree Edge** | Goes to unvisited vertex | u is white (unvisited) |
| **Back Edge** | Goes to an ancestor in DFS tree | u is gray (in current stack) |
| **Forward Edge** | Goes to a descendant (not tree edge) | u is black, d[u] > d[v] |
| **Cross Edge** | Goes to a vertex in different subtree | u is black, d[u] < d[v] |

> **Important:** In an **undirected** graph, DFS only produces Tree Edges and Back Edges — no forward or cross edges.

### Notation
- `d[v]` = discovery time of v
- `f[v]` = finish time of v

**Parenthesis Theorem:** For any two vertices u and v, exactly one of the following holds:
- `[d[u], f[u]]` and `[d[v], f[v]]` are entirely disjoint → cross/no relation
- `[d[u], f[u]]` contains `[d[v], f[v]]` → v is descendant of u
- `[d[v], f[v]]` contains `[d[u], f[u]]` → u is descendant of v

## DFS on Adjacency Matrix vs List

```cpp
// Adjacency Matrix DFS - O(V²)
void dfs_matrix(int v, int adj[][100], bool visited[], int n) {
    visited[v] = true;
    for (int u = 0; u < n; u++) {
        if (adj[v][u] && !visited[u]) {
            dfs_matrix(u, adj, visited, n);
        }
    }
}

// Adjacency List DFS - O(V + E)
void dfs_list(int v, vector<int> adj[], bool visited[]) {
    visited[v] = true;
    for (int u : adj[v]) {
        if (!visited[u]) {
            dfs_list(u, adj, visited);
        }
    }
}
```

## Complexity Analysis

| Representation | Time | Space |
|---|---|---|
| Adjacency Matrix | O(V²) | O(V²) for matrix + O(V) for stack |
| Adjacency List | O(V + E) | O(V + E) for list + O(V) for stack |

- **Stack depth (recursive):** O(V) in worst case (path graph)

## DFS Forest

When DFS is run on a disconnected graph, it produces a **DFS forest** — a collection of DFS trees, one per connected component.

---

# 4. Breadth First Search (BFS)

## Core Idea

BFS explores a graph **level by level**, visiting all neighbors of a vertex before moving to the next level. It uses a **queue** instead of a stack. Think of it as ripples in water — the start vertex is the stone dropped in.

## BFS Algorithm

```
BFS(G, s):
    mark all vertices as unvisited
    create queue Q
    mark s as visited, enqueue s
    while Q is not empty:
        v = dequeue(Q)
        process v
        for each neighbor u of v:
            if u is unvisited:
                mark u as visited
                enqueue u
                record (v, u) as tree edge
                d[u] = d[v] + 1   // BFS distance
```

## C++ Code

```cpp
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

void bfs(int start, vector<int> adj[], int n) {
    vector<bool> visited(n + 1, false);
    vector<int> dist(n + 1, -1);
    queue<int> q;
    
    visited[start] = true;
    dist[start] = 0;
    q.push(start);
    
    while (!q.empty()) {
        int v = q.front();
        q.pop();
        cout << "Visiting: " << v << " (distance " << dist[v] << ")" << endl;
        
        for (int u : adj[v]) {
            if (!visited[u]) {
                visited[u] = true;
                dist[u] = dist[v] + 1;
                q.push(u);
            }
        }
    }
}
```

## BFS Tree and Key Properties

1. **BFS Tree:** The tree formed by tree edges discovered during BFS.
2. **Shortest Path:** `d[v]` gives the shortest path (in terms of number of edges) from source to v.
3. **BFS levels:** All vertices at distance k are processed before distance k+1.

## Dry Run Example

```
Graph (undirected):
    1 - 2 - 5
    |   |
    3 - 4

BFS from vertex 1:
Level 0: Visit 1             Queue: [1]
Level 1: Visit 2, 3         Queue: [2, 3]
Level 2: Visit 5, 4         Queue: [5, 4]

BFS tree:
    1
   / \
  2   3
 / \
5   4

Distances: d[1]=0, d[2]=1, d[3]=1, d[4]=2, d[5]=2
```

## BFS Edge Classification (Undirected Graph)

In an undirected graph, BFS only gives:
- **Tree edges**: When discovering unvisited vertex
- **Cross edges**: Between vertices at same or adjacent levels

**No back edges exist in BFS tree for undirected graphs** (they'd have been tree edges).

## Complexity Analysis

| Representation | Time | Space |
|---|---|---|
| Adjacency Matrix | O(V²) | O(V²) + O(V) for queue |
| Adjacency List | O(V + E) | O(V + E) + O(V) for queue |

## DFS vs BFS: Head-to-Head Comparison

| Feature | DFS | BFS |
|---|---|---|
| Data Structure | Stack (or recursion) | Queue |
| Space Complexity | O(V) | O(V) |
| Finds shortest path? | No | Yes (unweighted) |
| Detects cycles? | Yes (back edges) | Yes (cross edges) |
| Connected components? | Yes | Yes |
| Bipartiteness check? | Yes | Yes (preferred) |
| Topological sort? | Yes | Yes (Kahn's algo) |
| Use for deep graphs | Better | Uses too much memory |
| Use for wide graphs | Better (less memory) | Explores too slowly |
| Complete (finite graph)? | Yes | Yes |

---

# 5. Topological Sorting

## Definition

A **topological order** (or topological sort) of a **Directed Acyclic Graph (DAG)** is a linear ordering of all vertices such that for every directed edge (u → v), vertex u comes **before** v in the ordering.

> **Critical:** Topological sort is only possible on a **DAG** (no cycles). If a cycle exists, no valid ordering exists.

## Real-World Example

- Course prerequisites: CS101 must come before CS201
- Build dependencies: Compile A before B if B depends on A
- Task scheduling with dependencies

## Method 1: DFS-Based Topological Sort

**Key Insight:** In DFS, when a vertex is **finished** (all its descendants visited), add it to the front of the result. The finished order reversed gives topological order.

```
DFS_Topo(G):
    mark all vertices unvisited
    result = empty list
    for each vertex v:
        if v unvisited:
            DFS_Visit_Topo(v, result)
    return reverse of result

DFS_Visit_Topo(v, result):
    mark v as visited
    for each neighbor u of v:
        if u unvisited:
            DFS_Visit_Topo(u, result)
    append v to result  // Add AFTER all descendants are done
```

## C++ Code (DFS Method)

```cpp
#include <iostream>
#include <vector>
#include <stack>
using namespace std;

vector<int> adj[100];
bool visited[100];
stack<int> result;

void dfs_topo(int v) {
    visited[v] = true;
    for (int u : adj[v]) {
        if (!visited[u]) {
            dfs_topo(u);
        }
    }
    result.push(v);  // Push after all descendants are done
}

void topologicalSort(int n) {
    fill(visited, visited + n + 1, false);
    for (int v = 1; v <= n; v++) {
        if (!visited[v]) {
            dfs_topo(v);
        }
    }
    cout << "Topological Order: ";
    while (!result.empty()) {
        cout << result.top() << " ";
        result.pop();
    }
}
```

## Method 2: Kahn's Algorithm (BFS / In-degree Method)

**Key Insight:** A vertex with in-degree 0 has no prerequisites — it can come first. Process it, reduce in-degrees of neighbors. Repeat.

```
Kahn(G):
    Compute in-degree for each vertex
    Enqueue all vertices with in-degree = 0
    count = 0
    while Queue is not empty:
        v = dequeue
        add v to result
        count++
        for each neighbor u of v:
            in_degree[u]--
            if in_degree[u] == 0:
                enqueue u
    if count != |V|:
        print "Cycle exists! Not a DAG"
    else:
        print result
```

## C++ Code (Kahn's Algorithm)

```cpp
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

vector<int> topologicalSortKahn(int n, vector<int> adj[]) {
    vector<int> in_degree(n + 1, 0);
    
    // Compute in-degrees
    for (int v = 1; v <= n; v++)
        for (int u : adj[v])
            in_degree[u]++;
    
    queue<int> q;
    for (int v = 1; v <= n; v++)
        if (in_degree[v] == 0)
            q.push(v);
    
    vector<int> result;
    while (!q.empty()) {
        int v = q.front(); q.pop();
        result.push_back(v);
        for (int u : adj[v]) {
            in_degree[u]--;
            if (in_degree[u] == 0)
                q.push(u);
        }
    }
    
    if ((int)result.size() != n)
        cout << "Cycle detected! Topological sort not possible." << endl;
    
    return result;
}
```

## Dry Run: Kahn's Algorithm

```
DAG:
5 → 0 → 4 → 2
        ↓
6 → 1 → 3 → 2

In-degrees: 0:1, 1:1, 2:2, 3:1, 4:1, 5:0, 6:0

Initial Queue: [5, 6]   (in-degree = 0)

Step 1: Process 5 → result=[5], reduce in_degree[0]=0 → Queue: [6, 0]
Step 2: Process 6 → result=[5,6], reduce in_degree[1]=0 → Queue: [0, 1]
Step 3: Process 0 → result=[5,6,0], reduce in_degree[4]=0 → Queue: [1, 4]
Step 4: Process 1 → result=[5,6,0,1], reduce in_degree[3]=0 → Queue: [4, 3]
Step 5: Process 4 → result=[5,6,0,1,4], reduce in_degree[2]=1 → Queue: [3]
Step 6: Process 3 → result=[5,6,0,1,4,3], reduce in_degree[2]=0 → Queue: [2]
Step 7: Process 2 → result=[5,6,0,1,4,3,2]

Final: 5 6 0 1 4 3 2
```

## Detecting Cycles with Kahn's

If the result has fewer vertices than V, a cycle exists (some vertices were never added because their in-degree never reached 0).

## DFS-Based Cycle Detection in Directed Graph

Use 3 colors:
- **White (0):** Unvisited
- **Gray (1):** In the current DFS path (recursion stack)
- **Black (2):** Fully processed

```cpp
bool hasCycle(int v, vector<int> adj[], int color[]) {
    color[v] = 1;  // Gray — currently processing
    for (int u : adj[v]) {
        if (color[u] == 1) return true;  // Back edge → cycle!
        if (color[u] == 0 && hasCycle(u, adj, color)) return true;
    }
    color[v] = 2;  // Black — done
    return false;
}
```

## Complexity

| Method | Time | Space |
|---|---|---|
| DFS-Based | O(V + E) | O(V) |
| Kahn's | O(V + E) | O(V) |

> Both are optimal — you must examine every vertex and edge at least once.

---

# 6. Applications of DFS and BFS

## 6.1 Finding Connected Components (Undirected Graph)

Each call to DFS/BFS from an unvisited vertex discovers a new component.

```cpp
int countComponents(int n, vector<int> adj[]) {
    bool visited[n + 1];
    fill(visited, visited + n + 1, false);
    int components = 0;
    for (int v = 1; v <= n; v++) {
        if (!visited[v]) {
            dfs(v, adj, visited);  // Or BFS
            components++;
        }
    }
    return components;
}
```

## 6.2 Checking Bipartiteness (BFS Method)

A graph is **bipartite** if we can color vertices with 2 colors such that no two adjacent vertices share the same color. BFS can verify this:

```cpp
bool isBipartite(int start, vector<int> adj[], int n) {
    vector<int> color(n + 1, -1);
    queue<int> q;
    color[start] = 0;
    q.push(start);
    while (!q.empty()) {
        int v = q.front(); q.pop();
        for (int u : adj[v]) {
            if (color[u] == -1) {
                color[u] = 1 - color[v];  // Flip color
                q.push(u);
            } else if (color[u] == color[v]) {
                return false;  // Same color = odd cycle = not bipartite
            }
        }
    }
    return true;
}
```

## 6.3 Shortest Path in Unweighted Graph (BFS)

BFS guarantees shortest path (in edge count) in unweighted graphs.

```cpp
vector<int> shortestPath(int src, int dest, vector<int> adj[], int n) {
    vector<int> dist(n + 1, -1);
    vector<int> parent(n + 1, -1);
    queue<int> q;
    dist[src] = 0;
    q.push(src);
    while (!q.empty()) {
        int v = q.front(); q.pop();
        for (int u : adj[v]) {
            if (dist[u] == -1) {
                dist[u] = dist[v] + 1;
                parent[u] = v;
                q.push(u);
            }
        }
    }
    // Reconstruct path from dest to src
    vector<int> path;
    for (int v = dest; v != -1; v = parent[v])
        path.push_back(v);
    reverse(path.begin(), path.end());
    return path;
}
```

## 6.4 Strongly Connected Components (SCC) — Kosaraju's Algorithm

A **Strongly Connected Component** is a maximal set of vertices such that there is a path from each vertex to every other vertex.

**Kosaraju's Algorithm:**
1. Run DFS on original graph G, push vertices to stack in finish order
2. Transpose the graph (reverse all edges)
3. Pop from stack, run DFS on transposed graph — each DFS tree is an SCC

```cpp
void dfs1(int v, vector<int> adj[], bool visited[], stack<int>& st) {
    visited[v] = true;
    for (int u : adj[v])
        if (!visited[u])
            dfs1(u, adj, visited, st);
    st.push(v);  // Push after processing
}

void dfs2(int v, vector<int> radj[], bool visited[]) {
    visited[v] = true;
    cout << v << " ";
    for (int u : radj[v])
        if (!visited[u])
            dfs2(u, radj, visited);
}

void kosaraju(int n, vector<int> adj[], vector<int> radj[]) {
    bool visited[n + 1];
    fill(visited, visited + n + 1, false);
    stack<int> st;
    for (int v = 1; v <= n; v++)
        if (!visited[v])
            dfs1(v, adj, visited, st);
    
    fill(visited, visited + n + 1, false);
    int scc_count = 0;
    while (!st.empty()) {
        int v = st.top(); st.pop();
        if (!visited[v]) {
            cout << "SCC " << ++scc_count << ": ";
            dfs2(v, radj, visited);
            cout << endl;
        }
    }
}
```

## 6.5 Finding Articulation Points and Bridges (DFS)

An **articulation point** (cut vertex) is a vertex whose removal increases the number of connected components.

A **bridge** is an edge whose removal disconnects the graph.

```cpp
int disc[100], low[100], timer_art = 0;
bool visited_art[100], isAP[100];
int parent_art[100];

void findAP(int v, vector<int> adj[]) {
    visited_art[v] = true;
    disc[v] = low[v] = ++timer_art;
    int children = 0;
    
    for (int u : adj[v]) {
        if (!visited_art[u]) {
            children++;
            parent_art[u] = v;
            findAP(u, adj);
            low[v] = min(low[v], low[u]);
            
            // v is AP if root with 2+ children, or bridge condition
            if (parent_art[v] == -1 && children > 1) isAP[v] = true;
            if (parent_art[v] != -1 && low[u] >= disc[v]) isAP[v] = true;
        } else if (u != parent_art[v]) {
            low[v] = min(low[v], disc[u]);
        }
    }
}
```

## 6.6 Summary Table of DFS/BFS Applications

| Application | Algorithm | Complexity |
|---|---|---|
| Connected components | DFS or BFS | O(V+E) |
| Bipartite check | BFS | O(V+E) |
| Shortest path (unweighted) | BFS | O(V+E) |
| Cycle detection (undirected) | DFS | O(V+E) |
| Cycle detection (directed) | DFS (3-color) | O(V+E) |
| Topological sort | DFS or BFS (Kahn) | O(V+E) |
| Strongly Connected Components | DFS (Kosaraju) | O(V+E) |
| Articulation points/Bridges | DFS | O(V+E) |

---

## ✅ EXAM SNAPSHOT: Decrease & Conquer

| Topic | Key Point |
|---|---|
| Insertion Sort | Decrease by 1; stable; O(n) best, O(n²) worst; in-place |
| DFS | Uses stack; O(V+E) list, O(V²) matrix; finds back edges |
| BFS | Uses queue; O(V+E); finds shortest path in unweighted graph |
| Topological Sort | Only for DAG; DFS finish order reversed; Kahn's uses in-degrees |
| SCC | Kosaraju: 2 DFS passes + transpose |
| Bipartite | BFS with 2-coloring |

---

# 7. Transform and Conquer — Core Idea

## What Is It?

Transform and Conquer solves a problem by first **transforming it into a different (simpler or more suitable) problem**, then solving the transformed problem.

## Three Forms

| Form | Description | Example |
|---|---|---|
| **Instance Simplification** | Transform input into a simpler instance of the same problem | Presorting (sort first, then solve) |
| **Representation Change** | Transform input into a different representation | Heaps, AVL trees |
| **Problem Reduction** | Transform to a completely different (known) problem | LCM → GCD, Matrix mult |

---

# 8. Presorting

## Core Idea

Sort the input first, then solve the problem more efficiently on the sorted data.

**Trade-off:** Sorting costs O(n log n), but can reduce subsequent operations from O(n²) to O(n) or O(log n).

## Application 1: Element Uniqueness

**Brute Force:** Compare all pairs → O(n²)

**With Presorting:**
```
Sort array: O(n log n)
Scan for adjacent duplicates: O(n)
Total: O(n log n) ← much better!
```

```cpp
bool hasAllUnique(vector<int>& A) {
    sort(A.begin(), A.end());
    for (int i = 0; i < (int)A.size() - 1; i++)
        if (A[i] == A[i+1]) return false;
    return true;
}
```

## Application 2: Finding Mode (Most Frequent Element)

**Brute Force:** O(n²)

**With Presorting:** Sort → scan for longest run of equal elements → O(n log n)

```cpp
int findMode(vector<int>& A) {
    sort(A.begin(), A.end());
    int mode = A[0], maxCount = 1, currCount = 1;
    for (int i = 1; i < (int)A.size(); i++) {
        if (A[i] == A[i-1]) currCount++;
        else currCount = 1;
        if (currCount > maxCount) {
            maxCount = currCount;
            mode = A[i];
        }
    }
    return mode;
}
```

## Application 3: Searching

**Linear Search (unsorted):** O(n) per query

**Binary Search (sorted):** O(log n) per query, after O(n log n) sorting

For k queries:
- Without sorting: O(k·n)
- With sorting: O(n log n + k·log n) — better when k is large

```cpp
bool binarySearch(vector<int>& A, int target) {
    int lo = 0, hi = A.size() - 1;
    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;
        if (A[mid] == target) return true;
        else if (A[mid] < target) lo = mid + 1;
        else hi = mid - 1;
    }
    return false;
}
```

## Complexity Summary for Presorting

| Problem | Without Presorting | With Presorting |
|---|---|---|
| Uniqueness check | O(n²) | O(n log n) |
| Find mode | O(n²) | O(n log n) |
| k searches | O(k·n) | O(n log n + k·log n) |
| Closest pair (1D) | O(n²) | O(n log n) → O(n) scan |

---

# 9. Heapsort

## What Is a Heap?

A **Max-Heap** is a complete binary tree where:
- The root has the **maximum** value
- Every parent is **≥** its children

Stored in array: for node at index `i` (0-indexed):
- Left child: `2i + 1`
- Right child: `2i + 2`
- Parent: `(i - 1) / 2`

```
Array: [16, 14, 10, 8, 7, 9, 3, 2, 4, 1]

Tree:
            16
          /    \
        14      10
       /  \    /  \
      8    7  9    3
     / \ /
    2  4 1
```

## The Heapify (Sift-Down) Operation

Restores heap property by pushing element down.

```cpp
void heapify(vector<int>& A, int n, int i) {
    int largest = i;
    int left = 2 * i + 1;
    int right = 2 * i + 2;
    
    if (left < n && A[left] > A[largest])
        largest = left;
    if (right < n && A[right] > A[largest])
        largest = right;
    
    if (largest != i) {
        swap(A[i], A[largest]);
        heapify(A, n, largest);  // Recursively fix the affected subtree
    }
}
```

**Complexity of heapify:** O(log n) — height of the tree

## Build Heap (Heapify All Non-Leaf Nodes)

```cpp
void buildHeap(vector<int>& A, int n) {
    // Start from last non-leaf and go up
    for (int i = n / 2 - 1; i >= 0; i--) {
        heapify(A, n, i);
    }
}
```

**Complexity of Build Heap:** O(n) — NOT O(n log n)!

> **Why O(n)?** Nodes near the leaves (majority) do very little work. The mathematical sum is O(n).

## Heapsort Algorithm

```
HeapSort(A[0..n-1]):
    1. Build Max-Heap from A         // O(n)
    2. for i = n-1 down to 1:
           swap A[0] with A[i]       // Max element to end
           heapify(A, i, 0)          // Restore heap for reduced array
```

## Complete C++ Code

```cpp
#include <iostream>
#include <vector>
using namespace std;

void heapify(vector<int>& A, int n, int i) {
    int largest = i, l = 2*i+1, r = 2*i+2;
    if (l < n && A[l] > A[largest]) largest = l;
    if (r < n && A[r] > A[largest]) largest = r;
    if (largest != i) {
        swap(A[i], A[largest]);
        heapify(A, n, largest);
    }
}

void heapSort(vector<int>& A) {
    int n = A.size();
    // Build Max Heap
    for (int i = n/2 - 1; i >= 0; i--)
        heapify(A, n, i);
    
    // Extract elements one by one
    for (int i = n - 1; i > 0; i--) {
        swap(A[0], A[i]);     // Move current root (max) to end
        heapify(A, i, 0);     // Heapify the reduced heap
    }
}

int main() {
    vector<int> arr = {12, 11, 13, 5, 6, 7};
    heapSort(arr);
    for (int x : arr) cout << x << " ";
    // Output: 5 6 7 11 12 13
}
```

## Dry Run

```
Input: [4, 10, 3, 5, 1]

Step 1 - Build Heap:
Start from i=1 (n/2-1 = 1)
  heapify at 1: [4, 10, 3, 5, 1]  → 10 > 5, already max
  heapify at 0: 10 > 4, swap → [10, 4, 3, 5, 1]
                heapify at 1: 5 > 4, swap → [10, 5, 3, 4, 1]
Heap: [10, 5, 3, 4, 1]

Step 2 - Extract:
i=4: swap A[0],A[4] → [1,5,3,4,10], heapify(4,0) → [5,4,3,1,10]
i=3: swap A[0],A[3] → [1,4,3,5,10], heapify(3,0) → [4,1,3,5,10]
i=2: swap A[0],A[2] → [3,1,4,5,10], heapify(2,0) → [3,1,4,5,10]
i=1: swap A[0],A[1] → [1,3,4,5,10], heapify(1,0) → [1,3,4,5,10]

Sorted: [1, 3, 4, 5, 10] ✓
```

## Complexity Analysis

| Operation | Time |
|---|---|
| Build Heap | O(n) |
| Each Extract + Heapify | O(log n) |
| Total (n extractions) | O(n log n) |

| Case | Time | Space |
|---|---|---|
| Best | O(n log n) | O(1) |
| Average | O(n log n) | O(1) |
| Worst | O(n log n) | O(1) |

> **Key advantages:** Guaranteed O(n log n) in ALL cases, in-place (O(1) space)
> **Disadvantage:** Not stable; poor cache performance compared to Quicksort

## Heapsort vs Merge Sort vs Quicksort

| Feature | Heapsort | Merge Sort | Quicksort |
|---|---|---|---|
| Best | O(n log n) | O(n log n) | O(n log n) |
| Average | O(n log n) | O(n log n) | O(n log n) |
| Worst | O(n log n) | O(n log n) | O(n²) |
| Space | O(1) | O(n) | O(log n) |
| Stable | No | Yes | No |
| Cache | Poor | Good | Good |

---

# 10. Problem Reduction

## Core Idea

**Reduce problem A to a known problem B.** If we can transform an instance of A into an instance of B, solve B, and convert B's answer back to A's answer, we can solve A using B's algorithm.

```
Problem A → Transform → Problem B → Solve B → Back-transform → Answer to A
           O(f(n))              O(g(n))      O(h(n))
Total: O(f(n) + g(n) + h(n))
```

## Example 1: LCM Reduction to GCD

**LCM(m, n) = m × n / GCD(m, n)**

Instead of computing LCM directly, reduce it to a GCD call.

```cpp
int gcd(int a, int b) { return b == 0 ? a : gcd(b, a % b); }
int lcm(int a, int b) { return a / gcd(a, b) * b; }  // Avoid overflow
```

## Example 2: Counting Paths via Matrix Exponentiation

Number of paths of length k from u to v = `(A^k)[u][v]` where A is the adjacency matrix.

Instead of BFS k times, **reduce to matrix multiplication**.

## Example 3: Linear Programming Reduction

Many optimization problems can be reduced to Linear Programming and solved with Simplex or Interior Point methods.

## Example 4: Sorting to Element Uniqueness

```cpp
// Reduce element uniqueness to sorting
bool isUnique(vector<int> A) {
    sort(A.begin(), A.end());  // Transform: sort
    for (int i = 0; i + 1 < A.size(); i++)
        if (A[i] == A[i+1]) return false;
    return true;
}
```

## NP-Completeness and Reductions

Reductions are foundational in complexity theory:
- If A reduces to B in polynomial time, and B ∈ P, then A ∈ P
- If A reduces to B, and A is NP-hard, then B is also NP-hard

---

## ✅ EXAM SNAPSHOT: Transform & Conquer

| Topic | Key Point |
|---|---|
| Presorting | Sort first O(n log n), then solve; useful for repeated queries |
| Heap | Complete binary tree; parent ≥ children; stored in array |
| Build Heap | O(n) (not O(n log n)); start from n/2-1 downward |
| Heapsort | O(n log n) always; in-place; not stable |
| Problem Reduction | Transform A→B, solve B, transform answer back |

---

# 11. Space and Time Trade-offs — Core Idea

## The Fundamental Trade-off

Use **extra memory (space)** to **speed up computation (time)**.

Two main forms:
1. **Input Enhancement:** Preprocess input and store extra info → use it to speed up processing
2. **Precomputed Results:** Store results of common subproblems (memoization, DP)

---

# 12. Sorting by Counting

## Overview

Counting-based sorts avoid comparisons and exploit the distribution of input values. They achieve O(n) time under certain conditions.

## 12.1 Counting Sort

**Assumption:** All elements are integers in the range [0, k]

### Algorithm

```
CountingSort(A[0..n-1], k):
    1. Create count array C[0..k], initialized to 0
    2. For each element A[i]: C[A[i]]++       // Count occurrences
    3. For j = 1 to k: C[j] += C[j-1]        // Cumulative sum (positions)
    4. For i = n-1 down to 0:                 // Build output (reverse for stability)
           B[C[A[i]] - 1] = A[i]
           C[A[i]]--
```

### C++ Code

```cpp
#include <iostream>
#include <vector>
using namespace std;

vector<int> countingSort(vector<int>& A, int k) {
    vector<int> count(k + 1, 0);
    
    // Step 1: Count frequencies
    for (int x : A) count[x]++;
    
    // Step 2: Prefix sum (positions)
    for (int i = 1; i <= k; i++) count[i] += count[i-1];
    
    // Step 3: Build output (right to left for stability)
    int n = A.size();
    vector<int> output(n);
    for (int i = n - 1; i >= 0; i--) {
        output[count[A[i]] - 1] = A[i];
        count[A[i]]--;
    }
    return output;
}

int main() {
    vector<int> arr = {4, 2, 2, 8, 3, 3, 1};
    auto sorted = countingSort(arr, 8);
    for (int x : sorted) cout << x << " ";
    // Output: 1 2 2 3 3 4 8
}
```

### Dry Run

```
A = [4, 2, 2, 8, 3, 3, 1], k = 8

Count:  [0, 1, 2, 1, 1, 0, 0, 0, 1]
         0  1  2  3  4  5  6  7  8

Prefix: [0, 1, 3, 4, 5, 5, 5, 5, 6]

Building output (right to left):
i=6: A[6]=1, pos=prefix[1]-1=0, output[0]=1, prefix[1]=0
i=5: A[5]=3, pos=prefix[3]-1=3, output[3]=3, prefix[3]=3
i=4: A[4]=3, pos=prefix[3]-1=2, output[2]=3, prefix[3]=2  (Wait! updated)
...

Final: [1, 2, 2, 3, 3, 4, 8]
```

### Complexity

| | Counting Sort |
|---|---|
| Time | O(n + k) |
| Space | O(n + k) |
| Stable | Yes |
| In-place | No |
| Suitable when | k = O(n) |

> **When k >> n**, Counting Sort becomes inefficient. For example, sorting 5 numbers in range [1, 10^9] would waste O(10^9) space.

## 12.2 Radix Sort

Sort by digit, from **Least Significant Digit (LSD)** to Most Significant Digit.

```
RadixSort(A, d):              // d = number of digits
    for i = 1 to d:
        Use a stable sort to sort A by digit i
```

```cpp
void countingSortByDigit(vector<int>& A, int exp) {
    int n = A.size();
    vector<int> output(n), count(10, 0);
    
    for (int x : A) count[(x / exp) % 10]++;
    for (int i = 1; i < 10; i++) count[i] += count[i-1];
    
    for (int i = n - 1; i >= 0; i--) {
        output[count[(A[i] / exp) % 10] - 1] = A[i];
        count[(A[i] / exp) % 10]--;
    }
    A = output;
}

void radixSort(vector<int>& A) {
    int maxVal = *max_element(A.begin(), A.end());
    for (int exp = 1; maxVal / exp > 0; exp *= 10)
        countingSortByDigit(A, exp);
}
```

| | Radix Sort |
|---|---|
| Time | O(d · (n + k)) = O(n) when d is constant |
| Space | O(n + k) |
| Stable | Yes (requires stable inner sort) |

---

# 13. Naive String Matching

## Problem Definition

Given:
- **Text T** of length n
- **Pattern P** of length m (m ≤ n)

Find all positions in T where P occurs.

## Algorithm

Slide pattern P over text T, one position at a time. At each position, check if P matches.

```
NaiveSearch(T[0..n-1], P[0..m-1]):
    for s = 0 to n - m:          // s = shift (starting position)
        match = true
        for j = 0 to m - 1:
            if T[s + j] ≠ P[j]:
                match = false
                break
        if match:
            print "Pattern found at index", s
```

## C++ Code

```cpp
#include <iostream>
#include <string>
using namespace std;

void naiveSearch(const string& T, const string& P) {
    int n = T.size(), m = P.size();
    for (int s = 0; s <= n - m; s++) {
        bool match = true;
        for (int j = 0; j < m; j++) {
            if (T[s + j] != P[j]) {
                match = false;
                break;
            }
        }
        if (match)
            cout << "Pattern found at index " << s << endl;
    }
}
```

## Dry Run

```
T = "AABAACAADAABAAABAA"
P = "AABA"

s=0: T[0..3] = "AABA" == P? Yes! → Found at 0
s=1: T[1..4] = "ABAA" == P? A=A, B≠A → No
s=2: T[2..5] = "BAAC" == P? B≠A → No
...
```

## Complexity

| Case | Comparisons | Time |
|---|---|---|
| Best | n - m + 1 | O(n) |
| Average | O(n) | O(n) |
| Worst | m(n - m + 1) | O(nm) |

**Worst case:** T = "AAAAAAA...A" (n times), P = "AAAB" (m times)
Every position requires m comparisons before mismatch.

---

# 14. Input Enhancement: Horspool's Algorithm

## Motivation

Naive matching moves the pattern by just 1 position after a mismatch. Can we **skip more positions**?

Horspool's Algorithm (a simplification of Boyer-Moore) uses a **Bad Character Shift Table** to skip multiple positions based on the **mismatched character in the text**.

## Key Idea

- Compare pattern **right to left**
- When mismatch occurs at `T[i + j]` with character `c = T[i + m - 1]`:
  - If `c` is not in pattern: shift by `m` (entire pattern length)
  - If `c` is in pattern: shift so the **rightmost occurrence** of `c` in P aligns with `T[i + m - 1]`

## Bad Character Shift Table (BCShift)

For each character `c` in the alphabet:

```
BCShift[c] = m                              if c not in P[0..m-2]
BCShift[c] = m - 1 - (last occurrence of c in P[0..m-2])
```

> **Important:** We only look at `P[0..m-2]` (not the last character of pattern), because we always align based on rightmost non-last occurrence.

## Computing the Shift Table

```
ComputeShiftTable(P[0..m-1]):
    for each character c in alphabet:
        Table[c] = m                   // Initialize to m (not found)
    for j = 0 to m - 2:              // Scan all but last character
        Table[P[j]] = m - 1 - j       // Distance from end
    return Table
```

## C++ Code

```cpp
#include <iostream>
#include <string>
#include <unordered_map>
using namespace std;

unordered_map<char, int> buildShiftTable(const string& P) {
    int m = P.size();
    unordered_map<char, int> table;
    // Default: any char not in pattern shifts by m
    // For chars in pattern (except last): shift = m - 1 - j
    for (int j = 0; j < m - 1; j++) {
        table[P[j]] = m - 1 - j;
    }
    // Last char of pattern: only set if not already set
    if (table.find(P[m-1]) == table.end())
        table[P[m-1]] = m;
    return table;
}

void horspoolSearch(const string& T, const string& P) {
    int n = T.size(), m = P.size();
    auto shift = buildShiftTable(P);
    
    int i = m - 1;  // i points to current rightmost position of pattern in text
    
    while (i <= n - 1) {
        int k = 0;  // Number of matched characters
        while (k < m && P[m - 1 - k] == T[i - k]) k++;
        
        if (k == m) {
            cout << "Pattern found at index " << (i - m + 1) << endl;
        }
        // Shift based on the character at the rightmost pattern position
        char c = T[i];
        int s = (shift.count(c)) ? shift[c] : m;
        i += s;
    }
}
```

## Dry Run (Horspool's)

```
T = "BARD LOVED BANANAS"  (simplified: "BANABANANA")
P = "BAN"  (m = 3)

Build Shift Table for "BAN":
  Scan P[0..1] = "BA"
  B: 3 - 1 - 0 = 2
  A: 3 - 1 - 1 = 1
  N (last char, not in table yet): shift = 3
  All others: 3

Table: B→2, A→1, N→3, *→3

i starts at m-1 = 2

i=2: Check T[2]=N, T[1]=A, T[0]=B against P[2]=N, P[1]=A, P[0]=B
     k=0: P[2]=N==T[2]=N ✓
     k=1: P[1]=A==T[1]=A ✓
     k=2: P[0]=B==T[0]=B ✓  → MATCH at index 0!
     Shift by T[2]=N → shift[N]=3 → i = 2+3 = 5

i=5: Check from T[5], compare from right
     ...continue
```

## Complexity

| Case | Time |
|---|---|
| Best | O(n/m) — skip entire pattern each time |
| Average | O(n) — typically sublinear |
| Worst | O(nm) — similar to naive (rare in practice) |

---

# 15. Input Enhancement: Boyer-Moore Algorithm

## Overview

Boyer-Moore is one of the most efficient string matching algorithms in practice. It uses TWO heuristic rules:

1. **Bad Character Rule (BCR)**
2. **Good Suffix Rule (GSR)**

The algorithm shifts by the **maximum** of the two rules.

## Rule 1: Bad Character Rule (BCR)

When mismatch at pattern position `j` with text character `c`:
- If `c` doesn't appear in P: shift pattern past this position (shift by `j + 1`)
- If `c` appears in P at position `k < j`: shift to align `P[k]` with `T[i+j]` (shift by `j - k`)

```
BCR Shift = j - (last occurrence of T[i+j] in P before position j)
          = j + 1   if T[i+j] not in P[0..j-1]
```

## Rule 2: Good Suffix Rule (GSR)

When a suffix `t = P[j+1..m-1]` has been matched, but P[j] mismatches T[i+j]:

Find the rightmost occurrence of `t` in P that is preceded by a character other than `P[j]`. Shift to align this occurrence with the matched text.

**Three sub-cases:**
1. If `t` occurs elsewhere in P (not at position j+1): align that occurrence
2. If no full occurrence, but a proper prefix of P matches a proper suffix of `t`: shift to align
3. Otherwise: shift by `m` (full pattern length)

## Preprocessing for GSR

```cpp
// Compute good suffix shift table
void computeGoodSuffix(const string& P, vector<int>& shift, vector<int>& border) {
    int m = P.size();
    shift.resize(m + 1);
    border.resize(m + 1);
    
    // Phase 1: find borders of each suffix
    int i = m, j = m + 1;
    border[i] = j;
    while (i > 0) {
        while (j <= m && P[i-1] != P[j-1]) {
            if (shift[j] == 0) shift[j] = j - i;
            j = border[j];
        }
        i--; j--;
        border[i] = j;
    }
    
    // Phase 2: fill remaining shifts
    j = border[0];
    for (i = 0; i <= m; i++) {
        if (shift[i] == 0) shift[i] = j;
        if (i == j) j = border[j];
    }
}
```

## Full Boyer-Moore (Simplified)

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
using namespace std;

// Bad Character Table
vector<int> badCharTable(const string& P) {
    int m = P.size();
    vector<int> table(256, -1);  // -1 = not in pattern
    for (int i = 0; i < m; i++)
        table[(unsigned char)P[i]] = i;  // Last occurrence
    return table;
}

void boyerMoore(const string& T, const string& P) {
    int n = T.size(), m = P.size();
    auto bc = badCharTable(P);
    
    int s = 0;  // Shift of pattern relative to text
    while (s <= n - m) {
        int j = m - 1;  // Start matching from right
        
        while (j >= 0 && P[j] == T[s + j])
            j--;
        
        if (j < 0) {
            cout << "Pattern found at index " << s << endl;
            s++;  // Simplified: move by 1 (full BM would use GSR here)
        } else {
            // Bad Character shift
            int shift = j - bc[(unsigned char)T[s + j]];
            s += max(1, shift);
        }
    }
}
```

## Comparison: BM vs Horspool vs Naive

| Feature | Naive | Horspool | Boyer-Moore |
|---|---|---|---|
| Direction | Left to Right | Right to Left (comparison) | Right to Left |
| Rules used | None | Bad Character only | Bad Char + Good Suffix |
| Best Case | O(n) | O(n/m) | O(n/m) |
| Worst Case | O(nm) | O(nm) | O(n) with GSR |
| Preprocessing | None | O(m + σ) | O(m + σ) |
| Practice | Slow | Fast in practice | Fastest in practice |

Where σ = alphabet size.

## Preprocessing Complexity

| Algorithm | Preprocessing | Searching |
|---|---|---|
| Naive | O(0) | O(nm) |
| Horspool | O(m + σ) | O(nm) worst, O(n/m) best |
| Boyer-Moore | O(m + σ) | O(nm) without GSR, O(n) with GSR |

---

## ✅ EXAM SNAPSHOT: Space-Time Trade-offs & String Matching

| Topic | Key Point |
|---|---|
| Counting Sort | O(n+k); needs k = O(n); stable; not in-place |
| Radix Sort | O(d·(n+k)); d digits; stable |
| Naive String Match | O(nm) worst; simple; no preprocessing |
| Horspool | Uses bad char table; shift by T[rightmost pos]; O(n/m) best |
| Boyer-Moore | BCR + GSR; shift = max(BCR, GSR); O(n) worst with GSR |

---

# 16. Complexity Master Table

| Algorithm | Best | Average | Worst | Space | Stable | In-place |
|---|---|---|---|---|---|---|
| **Insertion Sort** | O(n) | O(n²) | O(n²) | O(1) | Yes | Yes |
| **Heapsort** | O(n log n) | O(n log n) | O(n log n) | O(1) | No | Yes |
| **Counting Sort** | O(n+k) | O(n+k) | O(n+k) | O(n+k) | Yes | No |
| **Radix Sort** | O(d(n+k)) | O(d(n+k)) | O(d(n+k)) | O(n+k) | Yes | No |
| **DFS** | — | O(V+E) | O(V+E) | O(V) | — | — |
| **BFS** | — | O(V+E) | O(V+E) | O(V) | — | — |
| **Topo Sort (DFS)** | — | O(V+E) | O(V+E) | O(V) | — | — |
| **Topo Sort (Kahn)** | — | O(V+E) | O(V+E) | O(V) | — | — |
| **Naive String Match** | O(n) | O(n) | O(nm) | O(1) | — | — |
| **Horspool's** | O(n/m) | O(n) | O(nm) | O(σ) | — | — |
| **Boyer-Moore** | O(n/m) | O(n) | O(n) * | O(m+σ) | — | — |

*With Good Suffix Rule implemented fully

---

# 17. LeetCode Practice Problems

These are **medium-difficulty** problems that directly reinforce the topics from Units II and III.

## Insertion Sort

| # | Problem | Key Concept |
|---|---|---|
| 147 | Insertion Sort List | Insertion Sort on linked list |
| 148 | Sort List | Compare insertion sort vs merge sort |

## DFS

| # | Problem | Key Concept |
|---|---|---|
| 200 | Number of Islands | DFS on 2D grid (connected components) |
| 695 | Max Area of Island | DFS + counting |
| 130 | Surrounded Regions | DFS from boundary |
| 133 | Clone Graph | DFS with hashmap |
| 547 | Number of Provinces | DFS connected components |
| 841 | Keys and Rooms | DFS reachability |

## BFS

| # | Problem | Key Concept |
|---|---|---|
| 994 | Rotting Oranges | Multi-source BFS |
| 1926 | Nearest Exit from Entrance | BFS shortest path |
| 542 | 01 Matrix | BFS shortest distance |
| 1162 | As Far from Land as Possible | BFS level traversal |
| 286 | Walls and Gates | Multi-source BFS |

## Topological Sort

| # | Problem | Key Concept |
|---|---|---|
| 207 | Course Schedule | Cycle detection in DAG (Kahn's / DFS) |
| 210 | Course Schedule II | Topological sort (output order) |
| 802 | Find Eventual Safe States | Reverse topo sort |
| 1059 | All Paths from Source Lead to Dest | DFS + topo concepts |
| 310 | Minimum Height Trees | Iterative leaf pruning (like Kahn's) |

## DFS + BFS Mixed / Graph Applications

| # | Problem | Key Concept |
|---|---|---|
| 684 | Redundant Connection | DFS / Union-Find |
| 785 | Is Graph Bipartite? | BFS 2-coloring |
| 797 | All Paths from Source to Target | DFS in DAG |
| 1020 | Number of Enclaves | DFS from boundary |

## Heapsort / Heap Applications

| # | Problem | Key Concept |
|---|---|---|
| 215 | Kth Largest Element | Heap / Quickselect |
| 347 | Top K Frequent Elements | Heap / Bucket sort |
| 451 | Sort Characters by Frequency | Heap + counting |
| 692 | Top K Frequent Words | Heap |
| 703 | Kth Largest Element in a Stream | Min-heap |

## Counting Sort / Sorting

| # | Problem | Key Concept |
|---|---|---|
| 912 | Sort an Array | Implement various sorts |
| 75 | Sort Colors | Counting sort (3 values) |
| 274 | H-Index | Counting sort idea |
| 1051 | Height Checker | Counting sort |
| 1122 | Relative Sort Array | Counting sort |

## String Matching / Pattern Problems

| # | Problem | Key Concept |
|---|---|---|
| 28 | Find the Index of the First Occurrence in a String | Naive / KMP |
| 686 | Repeated String Match | String matching |
| 459 | Repeated Substring Pattern | Pattern matching concept |
| 796 | Rotate String | String containment |
| 1408 | String Matching in an Array | String matching |

---

## Quick Revision Tips

1. **DFS vs BFS decision:** If problem asks shortest path → BFS. If problem asks connectivity, cycle, DFS order → DFS.
2. **Topo sort trigger:** Any problem with "prerequisites" or "ordering with dependencies" → Kahn's or DFS topo.
3. **Heap trigger:** "Top K", "Kth largest/smallest" → Heap.
4. **Counting Sort trigger:** Elements are integers in a small range → O(n+k) is better than O(n log n).
5. **String matching:** If pattern is long and text is much longer → prefer Boyer-Moore/Horspool over naive.

---

*End of Notes | Design and Analysis of Algorithms — Units II & III*
*Topics: Decrease & Conquer, Transform & Conquer, Space-Time Trade-offs*
