# Searching & binary search

**Binary search:** On a **sorted** array, compare the middle to the target and eliminate half the range each step—**O(log n)**. The same pattern finds **boundaries**: first position where `f(i)` is true, insertion point, etc.

**Generalization:** “Binary search on the **answer**”—search an integer range `[lo, hi]` where `feasible(mid)` is **monotone** (if `mid` works, larger might too, or the opposite). Many optimization problems use this.

---

## Classic binary search

**Invariant:** Maintain `lo..hi` so the target, if present, stays inside. Exit when `lo > hi` or when found.

```javascript
function binarySearch(arr, target) {
  let lo = 0, hi = arr.length - 1;
  while (lo <= hi) {
    const mid = (lo + hi) >>> 1;
    if (arr[mid] === target) return mid;
    if (arr[mid] < target) lo = mid + 1;
    else hi = mid - 1;
  }
  return -1;
}
```

---

## Lower bound (first index ≥ target)

**Idea:** Search for the **leftmost** position where `arr[pos] >= target`. Loop invariant: answer in `[lo, hi)`; shrink half-open range. Returns `arr.length` if all elements are smaller.

```javascript
function lowerBound(arr, target) {
  let lo = 0, hi = arr.length;
  while (lo < hi) {
    const mid = (lo + hi) >>> 1;
    if (arr[mid] < target) lo = mid + 1;
    else hi = mid;
  }
  return lo;
}
```

---

## Binary search on answer (minimize k such that feasible(k))

**Example:** Ship packages with weights `weights` in `days` days—minimize ship **capacity**. If capacity `C` works, any larger capacity also works (**monotone**). Search the smallest feasible `C`.

```javascript
function minShipCapacity(weights, days) {
  let lo = Math.max(...weights);
  let hi = weights.reduce((a, b) => a + b, 0);
  function can(cap) {
    let need = 1, cur = 0;
    for (const w of weights) {
      if (cur + w > cap) {
        need++;
        cur = w;
      } else cur += w;
    }
    return need <= days;
  }
  while (lo < hi) {
    const mid = (lo + hi) >>> 1;
    if (can(mid)) hi = mid;
    else lo = mid + 1;
  }
  return lo;
}
```

---

## Search in rotated sorted array

**Idea:** One half of `[lo, hi]` is always **normally sorted**. Compare `nums[mid]` with ends to decide which half is sorted, then check whether `target` lies in that half’s range.

```javascript
function searchRotated(nums, target) {
  let lo = 0, hi = nums.length - 1;
  while (lo <= hi) {
    const mid = (lo + hi) >>> 1;
    if (nums[mid] === target) return mid;
    if (nums[lo] <= nums[mid]) {
      if (nums[lo] <= target && target < nums[mid]) hi = mid - 1;
      else lo = mid + 1;
    } else {
      if (nums[mid] < target && target <= nums[hi]) lo = mid + 1;
      else hi = mid - 1;
    }
  }
  return -1;
}
```

---

← [Previous: Sorting](10-sorting.md) · [README](../README.md) · [Next: Recursion & backtracking →](12-recursion-backtracking.md)
