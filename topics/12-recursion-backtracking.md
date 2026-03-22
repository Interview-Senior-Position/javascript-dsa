# Recursion & backtracking

**Recursion:** A function calls itself on a **smaller** subproblem until a **base case** stops. Costs **call stack** space proportional to **depth** (often **O(n)** or **O(log n)**).

**Backtracking:** Build a solution step by step (**candidates**). **Choose** an option, recurse, then **undo** (backtrack) to try the next option. Prune when constraints fail early.

---

## Factorial / Fibonacci (recursive)

**Fibonacci:** Naive recursion recomputes the same subproblems exponentially—**O(2ⁿ)** time. **Iterative** or **memoization** gives **O(n)**.

```javascript
function fib(n) {
  if (n <= 1) return n;
  return fib(n - 1) + fib(n - 2);
}

// Better: iterative O(n) time, O(1) space
function fibIter(n) {
  let a = 0, b = 1;
  for (let i = 0; i < n; i++) [a, b] = [b, a + b];
  return a;
}
```

---

## Subsets (backtracking)

**Idea:** At each index, either **include** or **exclude** elements (here: include with increasing index to avoid duplicates). Snapshot `path` when recording answers.

```javascript
function subsets(nums) {
  const res = [];
  function backtrack(start, path) {
    res.push([...path]);
    for (let i = start; i < nums.length; i++) {
      path.push(nums[i]);
      backtrack(i + 1, path);
      path.pop();
    }
  }
  backtrack(0, []);
  return res;
}
```

---

## Permutations

**Idea:** Fill positions one by one; track **used** indices so each element appears once. **O(n · n!)** time roughly—factorial search space.

```javascript
function permute(nums) {
  const res = [];
  function backtrack(path, used) {
    if (path.length === nums.length) {
      res.push([...path]);
      return;
    }
    for (let i = 0; i < nums.length; i++) {
      if (used[i]) continue;
      used[i] = true;
      path.push(nums[i]);
      backtrack(path, used);
      path.pop();
      used[i] = false;
    }
  }
  backtrack([], Array(nums.length).fill(false));
  return res;
}
```

---

## N-Queens (pruning)

**Idea:** Place queens row by row. **Column** and both **diagonals** (`r - c` and `r + c`) must be unique. If a position conflicts, **skip** early—don’t explore doomed branches.

```javascript
function solveNQueens(n) {
  const res = [];
  const cols = new Set();
  const diag1 = new Set(), diag2 = new Set();
  const board = Array.from({ length: n }, () => Array(n).fill("."));
  function dfs(r) {
    if (r === n) {
      res.push(board.map((row) => row.join("")));
      return;
    }
    for (let c = 0; c < n; c++) {
      if (cols.has(c) || diag1.has(r - c) || diag2.has(r + c)) continue;
      cols.add(c); diag1.add(r - c); diag2.add(r + c);
      board[r][c] = "Q";
      dfs(r + 1);
      board[r][c] = ".";
      cols.delete(c); diag1.delete(r - c); diag2.delete(r + c);
    }
  }
  dfs(0);
  return res;
}
```

---

## Word search (grid DFS)

**Idea:** From each cell, **DFS** along matching characters. **Mark** visited cells (e.g. with `#`) to avoid reusing the same cell in one path; **restore** after recursion.

```javascript
function exist(board, word) {
  const rows = board.length, cols = board[0].length;
  function dfs(r, c, i) {
    if (i === word.length) return true;
    if (r < 0 || c < 0 || r >= rows || c >= cols) return false;
    if (board[r][c] !== word[i]) return false;
    const tmp = board[r][c];
    board[r][c] = "#";
    const ok =
      dfs(r + 1, c, i + 1) ||
      dfs(r - 1, c, i + 1) ||
      dfs(r, c + 1, i + 1) ||
      dfs(r, c - 1, i + 1);
    board[r][c] = tmp;
    return ok;
  }
  for (let r = 0; r < rows; r++) {
    for (let c = 0; c < cols; c++) {
      if (dfs(r, c, 0)) return true;
    }
  }
  return false;
}
```

---

← [Previous: Searching](11-searching.md) · [README](../README.md) · [Next: Dynamic programming →](13-dynamic-programming.md)
