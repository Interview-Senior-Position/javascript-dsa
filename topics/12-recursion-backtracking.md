# Recursion & backtracking

## In plain English

**Recursion:** A task **calls itself** on a **smaller** version until a **stopping rule** (base case)—like Russian dolls or “countdown to zero.”

**Backtracking:** Try a **choice**, go deeper; if it **fails**, **undo** (backtrack) and try the next choice—like solving a maze: go forward, hit wall, **step back**, try another turn.

**Layman cost:** Each recursive call uses **stack space** (memory). Very deep chains can **overflow** the stack.

---

## Factorial / Fibonacci (recursive)

**Layman:** Naive Fibonacci recomputes the **same** subproblems again and again—like recomputing “ways to step 3” from scratch every time. **Iterative** = remember **last two** numbers only.

**Technical:** Naive fib **O(2ⁿ)** time; iterative **O(n)**.

```javascript
function fib(n) {
  if (n <= 1) return n;
  return fib(n - 1) + fib(n - 2);
}

function fibIter(n) {
  let a = 0, b = 1;
  for (let i = 0; i < n; i++) [a, b] = [b, a + b];
  return a;
}
```

---

## Subsets (backtracking)

**Layman:** Build **every combination** of items: at each step, **take** an item and recurse, then **put it back** and try the next. Snapshot the bag at each point.

**Technical:** **O(2ⁿ)** subsets.

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

**Layman:** **Orderings** of all items: fill slots one by one; mark **used** so you don’t pick the same card twice.

**Technical:** **O(n!)** arrangements.

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

**Layman:** Place queens **row by row**. If a column or diagonal **already** has a queen, **don’t** go there—**prune** bad branches early.

**Technical:** Sets for column and diagonals `r-c`, `r+c`.

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

**Layman:** From a cell, spell the word **letter by letter**, walking **up/down/left/right**. **Temporarily** mark visited cells so you don’t reuse them in **one** path; **erase** mark when backing up.

**Technical:** DFS + backtrack on grid.

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
