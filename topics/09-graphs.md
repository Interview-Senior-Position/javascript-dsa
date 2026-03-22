# Graphs

**Graph:** Vertices (nodes) and **edges** (connections). **Directed** edges go one way; **undirected** edges are two-way.

**Representations:**

- **Adjacency list:** `g[u] = [neighbors of u]` — sparse graphs, fast iteration of neighbors.
- **Adjacency matrix:** `M[u][v]` edge weight or 0/1 — dense graphs; **O(V²)** space.
- **Edge list:** list of `(u,v)` — simple, sometimes combined with other structures.

**Traversals:** **BFS** explores layer-by-layer (shortest path in **unweighted** graphs). **DFS** goes deep first (stack or recursion)—good for connectivity, cycles, topological sorting on DAGs.

---

## Adjacency list & DFS

**DFS:** From a node, visit an unvisited neighbor, recurse or stack. **Time** **O(V + E)** with adjacency lists.

```javascript
function buildGraph(n, edges) {
  const g = Array.from({ length: n }, () => []);
  for (const [u, v] of edges) {
    g[u].push(v);
    g[v].push(u); // undirected
  }
  return g;
}

function dfsRecursive(g, start, visited = new Set()) {
  visited.add(start);
  const out = [start];
  for (const nei of g[start]) {
    if (!visited.has(nei)) out.push(...dfsRecursive(g, nei, visited));
  }
  return out;
}

function dfsIterative(g, start) {
  const visited = new Set();
  const st = [start];
  const out = [];
  while (st.length) {
    const u = st.pop();
    if (visited.has(u)) continue;
    visited.add(u);
    out.push(u);
    for (const v of g[u]) if (!visited.has(v)) st.push(v);
  }
  return out;
}
```

---

## BFS (shortest path, unweighted)

**Idea:** Queue nodes by **increasing distance** from start. First time you reach `v`, you’ve used the **minimum number of edges**. **O(V + E)**.

```javascript
function bfsShortestPath(g, start, target) {
  const q = [start];
  const dist = new Map([[start, 0]]);
  while (q.length) {
    const u = q.shift();
    if (u === target) return dist.get(u);
    for (const v of g[u]) {
      if (!dist.has(v)) {
        dist.set(v, dist.get(u) + 1);
        q.push(v);
      }
    }
  }
  return -1;
}
```

---

## Detect cycle (undirected, DFS)

**Idea:** In an undirected graph, a DFS edge to an already **visited** node that is **not** your parent implies a **cycle**. Check every connected component.

```javascript
function hasCycleUndirected(g) {
  const n = g.length;
  const visited = new Set();
  function dfs(u, parent) {
    visited.add(u);
    for (const v of g[u]) {
      if (v === parent) continue;
      if (visited.has(v)) return true;
      if (dfs(v, u)) return true;
    }
    return false;
  }
  for (let i = 0; i < n; i++) {
    if (!visited.has(i) && dfs(i, -1)) return true;
  }
  return false;
}
```

---

## Topological sort (Kahn’s algorithm, DAG)

**Topological order:** Linear ordering of vertices so every directed edge `u → v` has `u` before `v`. **Only exists** for a **DAG** (no directed cycles).

**Kahn:** Repeatedly remove nodes with **in-degree 0** (sources). If you process all `n` nodes, you have a valid order; if not, there is a **cycle**.

```javascript
function topologicalSort(n, edges) {
  const indeg = Array(n).fill(0);
  const g = Array.from({ length: n }, () => []);
  for (const [u, v] of edges) {
    g[u].push(v);
    indeg[v]++;
  }
  const q = [];
  for (let i = 0; i < n; i++) if (indeg[i] === 0) q.push(i);
  const order = [];
  while (q.length) {
    const u = q.shift();
    order.push(u);
    for (const v of g[u]) {
      if (--indeg[v] === 0) q.push(v);
    }
  }
  return order.length === n ? order : []; // empty if cycle
}
```

---

## Union–Find (disjoint set)

**Use case:** Dynamic connectivity—“are u and v in the same component?” and “merge components.” **Path compression** in `find` and **union by rank/size** make operations nearly **O(1)** amortized (inverse Ackermann).

```javascript
class UnionFind {
  constructor(n) {
    this.p = Array.from({ length: n }, (_, i) => i);
    this.r = Array(n).fill(0);
  }
  find(x) {
    if (this.p[x] !== x) this.p[x] = this.find(this.p[x]);
    return this.p[x];
  }
  union(a, b) {
    let ra = this.find(a), rb = this.find(b);
    if (ra === rb) return false;
    if (this.r[ra] < this.r[rb]) [ra, rb] = [rb, ra];
    this.p[rb] = ra;
    if (this.r[ra] === this.r[rb]) this.r[ra]++;
    return true;
  }
}
```

---

← [Previous: Heaps](08-heaps.md) · [README](../README.md) · [Next: Sorting →](10-sorting.md)
