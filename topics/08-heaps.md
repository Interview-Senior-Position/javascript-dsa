# Heaps & priority queues

## In plain English

A **min-heap** is a **complete binary tree** stored in an array where **every parent is smaller than its children**. The **smallest** item is always at the **top** (index 0).

**Heap vs sorted list:** You don’t fully sort everything—only enough structure to **always get the min** next. **Insert** and **remove-min** are **fast** (logarithmic in size).

**Priority queue:** Serve the **highest-priority** item next (sometimes “smallest number first”). A heap is the usual engine.

**Layman:** JS has **no** built-in heap—you implement one or use a library for interview-style problems.

---

## Min-heap class (array storage, 0-based)

**Layman:** Store the tree **left-to-right** in an array. **Bubble up** after adding at the end; **sink down** after removing the top (replace with last item).

**Technical:** Parent `i` has children `2i+1`, `2i+2`; sift-up / sift-down **O(log n)**.

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

**Layman:** Count how often each number appears, then pick the **k** most common—like top **k** vote getters.

**Technical:** Sorting unique entries is **O(u log u)**; **min-heap of size k** can be **O(n log k)**; below uses sort for readability.

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

**Layman:** **Ideal:** keep the **smallest current head** among `k` lists in a heap—always **advance** the list that won. **Below:** dump all values and sort—simple but not **O(N log k)**.

**Technical:** Full sort merge is **O(N log N)**.

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

**Layman:** Saying “sort by frequency” is fine; mention **n log n** time. Mention **heap** when `k` is tiny vs `n`.

---

← [Previous: Trees & tries](07-trees.md) · [README](../README.md) · [Next: Graphs →](09-graphs.md)
