# Arrays & common techniques

## In plain English

An **array** is a numbered row of slots: “give me item 5” is instant. **Walking the whole row once** scales with how long the row is (**linear**). **Sticking something in the middle** means scooting everyone over—slow on big arrays.

**Layman:** Think of a concert line with fixed positions printed on the ground: jumping to seat 5 is easy; inserting a person in the middle makes everyone behind take one step (**slow**).

**Technical:** Index access **O(1)**; full scan **O(n)**; insert/delete middle **O(n)** worst case. JS arrays are dynamic and implemented as contiguous storage with fast tail ops.

---

## Two pointers

**Layman:** Put one finger at the **start** and one at the **end** (or two speeds moving along). You move them according to a simple rule instead of checking every pair with nested loops—like squeezing a hose until the pressure is right.

**Technical:** Often **O(n)** time, **O(1)** extra space. On a **sorted** array, two-sum compares `lo + hi` to the target; shrinking `hi` lowers the sum, raising `lo` raises it.

```javascript
// Layman: squeeze from both ends until two numbers add to target — O(n)
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

// Layman: swap first with last, then inward, until fingers meet — reverse in place
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

**Layman:** Imagine a **window** sliding along the street. You only care about **who’s inside the window right now**—add the new person at the front, drop the one who left the back, instead of recounting everyone from scratch each time.

**Technical:** Fixed-size window: **O(n)** one pass. Variable window: grow `hi`, shrink `lo` until a condition holds (monotone sum for non-negative numbers is typical).

```javascript
// Layman: best total for exactly k houses in a row — slide one step at a time
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

// Layman: stretch window until sum too big, then shrink from left — longest “good” stretch
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

**Layman:** Precompute a **running total** once. Then “how much from house 3 to house 7?” is **end total minus start total**—one subtraction instead of re-adding.

**Technical:** `prefix[i] = sum(nums[0..i-1])`; range `[l,r]` sum is `prefix[r+1] - prefix[l]`. Build **O(n)**, query **O(1)**.

```javascript
function buildPrefix(nums) {
  const p = [0];
  for (const x of nums) p.push(p[p.length - 1] + x);
  return p; // p[i] = sum(nums[0..i-1])
}

function rangeSum(prefix, l, r) {
  return prefix[r + 1] - prefix[l];
}
```

---

## Kadane’s algorithm (max subarray sum)

**Layman:** At each position ask: “Is it better to **extend** the streak I already have, or **start fresh** here?” Track the best streak you’ve seen anywhere.

**Technical:** `cur = max(x, cur + x)`; `best = max(best, cur)`. **O(n)** time, **O(1)** space.

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

**Layman (Dutch flag):** Sort only **red, white, blue** in one pass. Keep three zones: “reds done,” “whites unknown,” “blues from the right.” Swap the unknown middle into the correct zone.

**Technical:** Three pointers `low`, `mid`, `high`; **O(n)** one pass.

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
