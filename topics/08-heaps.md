# Heaps & priority queues

**Heap:** A **complete** binary tree stored in an array with the **heap property**: in a min-heap, each parent is ≤ its children. The **minimum** is always at index **0**.

**Operations:** **Peek** min is **O(1)**. **Insert** and **extract-min** are **O(log n)** via **sift-up** / **sift-down**.

**Priority queue:** Abstractly “process highest/lowest priority first”; a binary heap is the standard implementation. JavaScript has **no built-in heap**—implement one or use a library for interview-grade problems.

---

## Min-heap class (array storage, 0-based)

**Indexing:** Parent of `i` is `(i-1)>>1`. Children are `2*i+1` and `2*i+2`. **Sift up** after append; **sift down** after replacing root with last element.

```javascript
class MinHeap {
  constructor() {
    this.a = [];
  }
  size() {
    return this.a.length;
  }
  peek() {
    return this.a[0];
  }
  push(x) {
    this.a.push(x);
    this._siftUp(this.a.length - 1);
  }
  pop() {
    const a = this.a;
    if (!a.length) return undefined;
    const top = a[0];
    const last = a.pop();
    if (a.length) {
      a[0] = last;
      this._siftDown(0);
    }
    return top;
  }
  _parent(i) { return (i - 1) >> 1; }
  _left(i) { return (i << 1) + 1; }
  _right(i) { return (i << 1) + 2; }
  _siftUp(i) {
    const a = this.a;
    while (i > 0) {
      const p = this._parent(i);
      if (a[p] <= a[i]) break;
      [a[p], a[i]] = [a[i], a[p]];
      i = p;
    }
  }
  _siftDown(i) {
    const a = this.a;
    const n = a.length;
    while (true) {
      let smallest = i;
      const l = this._left(i), r = this._right(i);
      if (l < n && a[l] < a[smallest]) smallest = l;
      if (r < n && a[r] < a[smallest]) smallest = r;
      if (smallest === i) break;
      [a[i], a[smallest]] = [a[smallest], a[i]];
      i = smallest;
    }
  }
}
```

---

## Top K frequent elements

**Idea:** Count frequencies, then keep the **k** largest by count. A **min-heap of size k** on pairs `[count, value]` gives **O(n log k)**; sorting all unique entries is **O(u log u)** where `u` is unique count—often acceptable in interviews.

Below: sort by frequency (simplest to read). For a real heap solution, store `[count, num]` and compare by `count` in sift operations.

```javascript
function topKFrequent(nums, k) {
  const freq = new Map();
  for (const x of nums) freq.set(x, (freq.get(x) ?? 0) + 1);
  return [...freq.entries()]
    .sort((a, b) => b[1] - a[1])
    .slice(0, k)
    .map((e) => e[0]);
}
```

---

## Merge K sorted lists (heap idea)

**Ideal approach:** Min-heap of size **k** storing current head of each list (or divide-and-conquer merge pairs). **O(N log k)** for total nodes `N`.

**Below:** Collecting all values and sorting is **O(N log N)**—only for illustration when lists are small.

```javascript
function mergeKLists(lists) {
  const all = [];
  for (const head of lists) {
    let h = head;
    while (h) {
      all.push(h.val);
      h = h.next;
    }
  }
  all.sort((a, b) => a - b);
  return all;
}
```

---

## Note: `sort` with comparator

**Interview tip:** Saying “sort by X” is fine if you state **O(n log n)**. Mention a **heap** when `k` is small vs `n` or data is **streaming**.

---

← [Previous: Trees & tries](07-trees.md) · [README](../README.md) · [Next: Graphs →](09-graphs.md)
