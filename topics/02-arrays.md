# Arrays & common techniques

**What arrays are good at:** Random access by index in **O(1)** and sequential scans in **O(n)**.

**JavaScript specifics:** Arrays are **dynamic** and act as lists. Index access is fast, but **inserting or deleting in the middle** shifts elements and costs **O(n)** in the worst case. For frequent head/tail operations, consider structures from the [stacks & queues](05-stacks-queues.md) topic.

---

## Two pointers

**Idea:** Place two indices (`lo`/`hi` or `slow`/`fast`) and move them according to a rule. Often **O(n)** with **O(1)** extra space instead of nested loops or extra data structures.

**Sorted two-sum:** If the array is sorted, compare `arr[lo] + arr[hi]` to the target: if the sum is too small, increase `lo`; if too large, decrease `hi`. That works because moving `lo` up increases the sum, moving `hi` down decreases it.

```javascript
// Sorted array: pair with target sum — O(n), O(1) space
function twoSumSorted(arr, target) {
  let lo = 0, hi = arr.length - 1;
  while (lo < hi) {
    const sum = arr[lo] + arr[hi];
    if (sum === target) return [lo, hi];
    if (sum < target) lo++;
    else hi--;
  }
  return null;
}

// Reverse in place — swap symmetric elements until pointers meet
function reverseInPlace(arr) {
  let i = 0, j = arr.length - 1;
  while (i < j) {
    [arr[i], arr[j]] = [arr[j], arr[i]];
    i++; j--;
  }
  return arr;
}
```

---

## Sliding window (fixed / variable)

**Idea:** Maintain a **window** `[lo, hi]` of elements that satisfies a condition. Slide `hi` forward to grow, and adjust `lo` to restore the condition. Avoid recomputing from scratch each time—update sums/counts **incrementally**.

**Fixed window:** After the first `k` elements, each step adds one new element and removes the one that left the window—**O(n)** total.

**Variable window:** Expand `hi` until invalid, then shrink `lo` until valid again. Works well for “longest subarray with property X” when the property is **monotone** as you extend the window.

```javascript
// Max sum of k consecutive elements — O(n)
function maxSumSubarrayK(arr, k) {
  if (arr.length < k) return 0;
  let sum = 0;
  for (let i = 0; i < k; i++) sum += arr[i];
  let best = sum;
  for (let i = k; i < arr.length; i++) {
    sum += arr[i] - arr[i - k];
    best = Math.max(best, sum);
  }
  return best;
}

// Longest subarray with sum ≤ target (non-negative values) — variable window
function longestSubarrayAtMost(nums, target) {
  let lo = 0, sum = 0, best = 0;
  for (let hi = 0; hi < nums.length; hi++) {
    sum += nums[hi];
    while (sum > target && lo <= hi) {
      sum -= nums[lo++];
    }
    best = Math.max(best, hi - lo + 1);
  }
  return best;
}
```

---

## Prefix sum

**Idea:** Precompute cumulative sums so any **range sum** `nums[l] + … + nums[r]` becomes **O(1)** after **O(n)** preprocessing.

With `prefix[i] = sum(nums[0..i-1])`, the sum on `[l, r]` is `prefix[r+1] - prefix[l]`. A leading `0` simplifies indexing.

```javascript
function buildPrefix(nums) {
  const p = [0];
  for (const x of nums) p.push(p[p.length - 1] + x);
  return p; // p[i] = sum(nums[0..i-1])
}

// Sum of nums[l..r] inclusive = prefix[r+1] - prefix[l]
function rangeSum(prefix, l, r) {
  return prefix[r + 1] - prefix[l];
}
```

---

## Kadane’s algorithm (max subarray sum)

**Idea:** For each position, the best subarray **ending here** either starts fresh at `x` or extends the previous best. Track the global maximum. **O(n)** time, **O(1)** space.

```javascript
function maxSubarraySum(nums) {
  let best = -Infinity, cur = 0;
  for (const x of nums) {
    cur = Math.max(x, cur + x);
    best = Math.max(best, cur);
  }
  return best;
}
```

---

## Rotate / partition patterns

**Dutch national flag (0, 1, 2):** Three regions—`[0..low-1]` are 0s, `[low..mid-1]` are 1s, unknown in the middle, `[high+1..end]` are 2s. Swap `mid` with `low` or `high` and shrink the unknown region. **O(n)** one pass.

```javascript
function sortColors(nums) {
  let low = 0, mid = 0, high = nums.length - 1;
  while (mid <= high) {
    if (nums[mid] === 0) {
      [nums[low], nums[mid]] = [nums[mid], nums[low]];
      low++; mid++;
    } else if (nums[mid] === 1) {
      mid++;
    } else {
      [nums[mid], nums[high]] = [nums[high], nums[mid]];
      high--;
    }
  }
}
```

---

← [Previous: Complexity](01-complexity.md) · [README](../README.md) · [Next: Strings →](03-strings.md)
