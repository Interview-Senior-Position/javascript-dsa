# Graphs

## In plain English

A **graph** is **dots** (people, cities, tasks) and **lines** (friendships, roads, dependencies). **Undirected** = two-way street; **directed** = one-way arrow.

**How to store it:**  
**Adjacency list** = for each dot, **list who it touches** (common, compact).  
**Matrix** = big table “does every pair connect?” (dense graphs).

**Walking the graph:**  
**BFS** = **ripple** outward in layers (shortest path if every edge counts as 1).  
**DFS** = **follow one path** until dead end, then backtrack—like maze exploration.

---

## Adjacency list & DFS

**Layman:** From a starting dot, **visit neighbors**; mark **visited** so you don’t spin forever. **Recursive** DFS = “let the call stack do the backtracking.” **Iterative** DFS = explicit **stack**.

**Technical:** Time **O(V + E)** with adjacency lists.

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

**Layman:** **Wave** by wave: first your friends (distance 1), then their friends (2), etc. **First time** you reach a node is the **shortest** hop count (unweighted).

**Technical:** Queue + distance map; **O(V + E)**.

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

**Layman:** If you walk and meet someone **already visited** who isn’t **just the person you came from**, you found a **loop** in the road network.

**Technical:** DFS with **parent** pointer; check all components.

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

**Layman:** **Dependencies** (A before B). Repeatedly do tasks **with no remaining prerequisites** (no incoming arrows). If you can’t finish all tasks, there’s a **dependency loop**.

**Technical:** Kahn’s algorithm; **in-degree** counts; **DAG** only.

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

**Layman:** **Groups** merge over time: “Are **A** and **B** in the same group?” **Union** merges groups. **Path compression** = “everyone remembers the boss directly.”

**Technical:** `find`/`union` nearly **O(1)** amortized with rank and path compression.

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
