# Searching & binary search

## In plain English

**Binary search** = **phone book** strategy: open to the **middle**; if your name is **before** that page, throw away the **right** half; else throw away the **left** half. Repeat. **Sorted** data required.

**Search the answer:** Sometimes you don’t have a sorted array—you have a **number** (capacity, speed, time) and can ask “does **this** value **work**?” If “works” flips from **no** to **yes** as you increase the value, you can **binary search** that number.

---

## Classic binary search

**Layman:** Halve the remaining **range** until you find the target or run out.

**Technical:** **O(log n)** on sorted array.

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

**Layman:** Find the **first** slot where the value is **≥ target**—like “where would I **insert** this to keep order?”

**Technical:** Half-open interval `[lo, hi)` invariant; returns `length` if all smaller.

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

## Binary search on answer (minimize k such as feasible(k))

**Layman:** Ship packages in **`days`** days—trucks have a **capacity**. Bigger capacity → **fewer** trips. Find the **smallest** capacity that still fits in `days` by asking “does this capacity work?” and halving the search range.

**Technical:** Monotone `can(mid)`; binary search **O(n log sum)** for checking `mid`.

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

**Layman:** A sorted list was **rotated** at some pivot—half of any range is still **normally sorted**. Compare **mid** to ends to find which half is sorted, then check if **target** sits in that half’s range.

**Technical:** **O(log n)** with careful cases.

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
