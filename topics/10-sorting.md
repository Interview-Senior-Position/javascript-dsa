# Sorting

## In plain English

**Sorting** = put things in **order** (smallest to largest, or A–Z).

**Stable sort:** If two items **tie**, their **original order** is preserved—like two people named “Alex” keeping their line order.

**Why not always bubble sort?** For big lists, **smarter** methods do far less work. Comparison sorts can’t beat **n log n** in the worst case (roughly)—that’s a **fundamental** limit.

---

## Bubble sort — O(n²)

**Layman:** Walk the line repeatedly; **swap neighbors** if they’re wrong. Big values “**bubble**” to the end. Simple to explain, **slow** on large data—teaching only.

**Technical:** **O(n²)** time; in-place.

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

**Layman:** **Split** the pile in half until **tiny** piles, **sort** each half, **merge** two sorted piles like **zippering** two lines of people by height.

**Technical:** Divide-and-conquer; **O(n log n)** worst case; **O(n)** extra for merged output.

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

**Layman:** Pick a **pivot** (reference height). **Partition:** smaller left, larger right. **Recurse** on left and right. Bad pivot choices can make it **slow**; random pivot helps in practice.

**Technical:** Average **O(n log n)**; worst **O(n²)**; in-place.

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

**Layman:** If values are only **small integers** (e.g. grades 0–100), **count** how many of each, then **print** that many copies in order—no comparing pairs.

**Technical:** **O(n + k)** where `k` is value range; not comparison-based.

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

**Layman:** JS **sort** treats numbers as **text** by default—**10** comes before **2**. Always pass a **compare function** for numbers.

**Technical:** Timsort-style in modern engines—typically **O(n log n)**.

```javascript
const nums = [10, 2, 1];
nums.sort((a, b) => a - b);
```

---

← [Previous: Graphs](09-graphs.md) · [README](../README.md) · [Next: Searching →](11-searching.md)
