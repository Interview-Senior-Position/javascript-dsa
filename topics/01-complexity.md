# Complexity & Big O

**Why it matters:** Before you optimize, you need a shared language for how running time and memory grow as input size `n` grows. **Asymptotic analysis** (Big O) describes that growth **in the limit**—we care about dominant behavior for large `n`, not constant factors or small inputs.

Understanding **time** and **space** cost helps you pick structures and algorithms and explain tradeoffs in interviews.

---

## What Big O measures

- **Time complexity:** how the number of **primitive steps** (comparisons, assignments, etc.) scales with `n`.
- **Space complexity:** how much **extra** memory your algorithm needs beyond storing the input (unless the problem asks for output size).

Big O is an **upper bound** in the worst case unless stated otherwise (e.g. “average case”).

---

## Big O intuition

| Notation | Name | Example |
|----------|------|---------|
| O(1) | constant | array index, hash map get |
| O(log n) | logarithmic | binary search, balanced tree ops |
| O(n) | linear | single pass, BFS layer in sparse graph |
| O(n log n) | linearithmic | efficient sorts (merge, heap) |
| O(n²) | quadratic | nested loops on same data |
| O(2ⁿ) | exponential | naive subset generation |

**Reading the table:** Constant means work does not grow with `n`. Logarithmic usually means you **halve** the problem each step. Linear is one pass per element. Quadratic often means comparing or nesting over all pairs. Exponential means the work **multiplies** by a constant factor for each extra element (e.g. brute-force choices).

**Rules of thumb**

- Drop constants: `O(2n)` → `O(n)` (constants don’t change growth class).
- Keep dominant term: `O(n² + n)` → `O(n²)` (lower-order terms vanish for large `n`).
- Nested loops often **multiply** complexity; sequential steps **add** (take the max of independent phases).

---

## Code: estimating loops

**Single loop over `n`:** typically **O(n)**. **Nested loops** where both indices go up to `n`:** often **O(n²)**. **Halving** the search range each step:** **O(log n)**.

```javascript
// O(n) — single pass: work proportional to array length
function sumArray(arr) {
  let s = 0;
  for (let i = 0; i < arr.length; i++) s += arr[i];
  return s;
}

// O(n²) — nested loops on length n: roughly n(n-1)/2 pairs
function allPairs(arr) {
  const out = [];
  for (let i = 0; i < arr.length; i++) {
    for (let j = i + 1; j < arr.length; j++) {
      out.push([arr[i], arr[j]]);
    }
  }
  return out;
}

// O(log n) — each iteration cuts the range in half
function binarySearch(sorted, target) {
  let lo = 0, hi = sorted.length - 1;
  while (lo <= hi) {
    const mid = (lo + hi) >>> 1;
    if (sorted[mid] === target) return mid;
    if (sorted[mid] < target) lo = mid + 1;
    else hi = mid - 1;
  }
  return -1;
}
```

`>>>` on the midpoint avoids overflow quirks in other languages; in JS it is still a safe idiom for nonnegative indices.

---

## Space complexity

**Auxiliary space** is extra memory beyond the input (e.g. a `Set` for visited nodes). The **output** itself (e.g. a new array of size `n`) is sometimes analyzed separately.

- **Input space** is usually given; you don’t “optimize away” storing the input unless the problem allows streaming.
- **Recursion** uses **call stack** space: depth can be **O(n)** (e.g. a chain of recursive calls) or **O(log n)** for balanced divide-and-conquer.

```javascript
// Produces a new array of length n — often discussed as output space
function duplicate(arr) {
  return arr.map((x) => x);
}

// O(n) auxiliary: a Set holding up to n entries
function usesExtraSet(n) {
  const seen = new Set();
  for (let i = 0; i < n; i++) seen.add(i);
  return seen.size;
}
```

---

## Amortized analysis

Some structures occasionally do an **expensive** operation, but if that cost can be “spread” over many cheap operations, the **average cost per operation** stays low.

**Example:** Appending to a dynamic array may trigger a rare **resize** (copy everything), but resizing is infrequent enough that **amortized** cost per `push` is still **O(1)** for typical implementations.

---

← [README](../README.md) · [Next: Arrays →](02-arrays.md)
