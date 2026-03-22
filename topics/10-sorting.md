# Sorting

**Stability:** A **stable** sort keeps the relative order of equal keys. **Merge sort** is stable; typical **quick sort** in-place is **not** stable.

**Comparison lower bound:** Any comparison-based sort needs **Ω(n log n)** comparisons in the worst case.

---

## Bubble sort — O(n²)

**Idea:** Repeatedly swap adjacent elements if out of order; each pass bubbles the largest unsorted element to the end. Simple but slow—mainly pedagogical.

```javascript
function bubbleSort(arr) {
  const a = [...arr];
  for (let i = 0; i < a.length; i++) {
    for (let j = 0; j < a.length - 1 - i; j++) {
      if (a[j] > a[j + 1]) [a[j], a[j + 1]] = [a[j + 1], a[j]];
    }
  }
  return a;
}
```

---

## Merge sort — O(n log n), O(n) extra space

**Idea:** **Divide** array in half, sort halves **recursively**, **merge** two sorted halves in linear time. Predictable **O(n log n)** worst case; uses **O(n)** auxiliary space for merging.

```javascript
function mergeSort(arr) {
  if (arr.length <= 1) return arr;
  const mid = arr.length >> 1;
  const left = mergeSort(arr.slice(0, mid));
  const right = mergeSort(arr.slice(mid));
  return merge(left, right);
}
function merge(a, b) {
  const out = [];
  let i = 0, j = 0;
  while (i < a.length && j < b.length) {
    out.push(a[i] <= b[j] ? a[i++] : b[j++]);
  }
  return out.concat(a.slice(i), b.slice(j));
}
```

---

## Quick sort (Lomuto partition) — average O(n log n)

**Idea:** Pick a **pivot** (here, last element), partition so smaller elements are left, larger right, then recurse. **Average** **O(n log n)**; **worst** **O(n²)** on bad pivots (mitigated by random pivot in practice).

```javascript
function quickSort(arr, lo = 0, hi = arr.length - 1) {
  if (lo >= hi) return arr;
  const p = partition(arr, lo, hi);
  quickSort(arr, lo, p - 1);
  quickSort(arr, p + 1, hi);
  return arr;
}
function partition(arr, lo, hi) {
  const pivot = arr[hi];
  let i = lo;
  for (let j = lo; j < hi; j++) {
    if (arr[j] <= pivot) {
      [arr[i], arr[j]] = [arr[j], arr[i]];
      i++;
    }
  }
  [arr[i], arr[hi]] = [arr[hi], arr[i]];
  return i;
}
```

---

## Counting sort — O(n + k) when keys in small range

**Idea:** Count how many of each key, then write that many copies in order. **Not comparison-based**; requires **integer keys in a bounded range** `[0, maxVal]`. **Stable** if implemented carefully (often cumulative counts).

```javascript
function countingSort(arr, maxVal) {
  const count = Array(maxVal + 1).fill(0);
  for (const x of arr) count[x]++;
  const out = [];
  for (let i = 0; i <= maxVal; i++) {
    for (let j = 0; j < count[i]; j++) out.push(i);
  }
  return out;
}
```

---

## Built-in sort

**Critical:** Default `sort()` converts elements to strings—**`[10,2,1]` sorts wrong**. Always pass **`(a,b) => a - b`** for numbers.

```javascript
const nums = [10, 2, 1];
nums.sort((a, b) => a - b);
```

---

← [Previous: Graphs](09-graphs.md) · [README](../README.md) · [Next: Searching →](11-searching.md)
