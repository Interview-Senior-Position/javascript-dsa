# Dynamic programming

**When it applies:** The problem has **optimal substructure** (optimal solution built from optimal subsolutions) and **overlapping subproblems** (same subproblem solved many times naively).

**Methods:** **Memoization** (recursive + cache), **tabulation** (fill table bottom-up). Often you can reduce space to **previous row** or a few variables.

---

## Climbing stairs

**Model:** Reach step `n` from `n-1` or `n-2`. Ways(n) = Ways(n-1) + Ways(n-2)—Fibonacci. **O(n)** time, **O(1)** space with two variables.

```javascript
function climbStairs(n) {
  if (n <= 2) return n;
  let a = 1, b = 2;
  for (let i = 3; i <= n; i++) [a, b] = [b, a + b];
  return b;
}
```

---

## House robber (linear)

**Idea:** For each house, either **skip** it (take previous best) or **rob** it (add value to best from two steps back). `dp[i] = max(dp[i-1], dp[i-2] + nums[i])`.

```javascript
function rob(nums) {
  let prev2 = 0, prev1 = 0;
  for (const x of nums) {
    const cur = Math.max(prev1, prev2 + x);
    prev2 = prev1;
    prev1 = cur;
  }
  return prev1;
}
```

---

## Longest increasing subsequence — O(n log n) with patience sorting

**Idea:** Maintain **`tails[k]`** = smallest ending value of an increasing subsequence of length `k+1`. For each `x`, **binary search** where to place `x` (replace first `≥ x`). Length of `tails` is the LIS length.

```javascript
function lengthOfLIS(nums) {
  const tails = [];
  for (const x of nums) {
    let lo = 0, hi = tails.length;
    while (lo < hi) {
      const mid = (lo + hi) >>> 1;
      if (tails[mid] < x) lo = mid + 1;
      else hi = mid;
    }
    if (lo === tails.length) tails.push(x);
    else tails[lo] = x;
  }
  return tails.length;
}
```

---

## 0/1 Knapsack (tabulation)

**Idea:** Each item **once**—either take (add value, subtract capacity) or skip. `dp[i][w]` = max value using first `i` items with capacity `w`.

```javascript
function knapsack(weights, values, W) {
  const n = weights.length;
  const dp = Array.from({ length: n + 1 }, () => Array(W + 1).fill(0));
  for (let i = 1; i <= n; i++) {
    for (let w = 0; w <= W; w++) {
      dp[i][w] = dp[i - 1][w];
      if (w >= weights[i - 1]) {
        dp[i][w] = Math.max(
          dp[i][w],
          dp[i - 1][w - weights[i - 1]] + values[i - 1]
        );
      }
    }
  }
  return dp[n][W];
}
```

---

## Edit distance

**Idea:** `dp[i][j]` = min operations to convert first `i` chars of `word1` to first `j` chars of `word2`. Match: no cost; else **1 + min**(delete, insert, replace).

```javascript
function minDistance(word1, word2) {
  const m = word1.length, n = word2.length;
  const dp = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0));
  for (let i = 0; i <= m; i++) dp[i][0] = i;
  for (let j = 0; j <= n; j++) dp[0][j] = j;
  for (let i = 1; i <= m; i++) {
    for (let j = 1; j <= n; j++) {
      if (word1[i - 1] === word2[j - 1]) {
        dp[i][j] = dp[i - 1][j - 1];
      } else {
        dp[i][j] = 1 + Math.min(dp[i - 1][j], dp[i][j - 1], dp[i - 1][j - 1]);
      }
    }
  }
  return dp[m][n];
}
```

---

## Coin change (unbounded)

**Idea:** Each coin used **unlimited** times. `dp[a]` = min coins for amount `a`; try every coin `c ≤ a`.

```javascript
function coinChange(coins, amount) {
  const dp = Array(amount + 1).fill(Infinity);
  dp[0] = 0;
  for (let a = 1; a <= amount; a++) {
    for (const c of coins) {
      if (a >= c) dp[a] = Math.min(dp[a], dp[a - c] + 1);
    }
  }
  return dp[amount] === Infinity ? -1 : dp[amount];
}
```

---

← [Previous: Recursion & backtracking](12-recursion-backtracking.md) · [README](../README.md) · [Next: Greedy →](14-greedy.md)
