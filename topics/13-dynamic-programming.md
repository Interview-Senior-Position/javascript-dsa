# Dynamic programming

## In plain English

**Dynamic programming (DP):** You **reuse answers** to smaller problems instead of recomputing them. Like a **memo** on a test: if you already solved “ways to reach step 5,” **look it up** instead of recounting from scratch.

**When it fits:** The big answer is built from **smaller** answers (**optimal substructure**), and the same small question appears **many times** (**overlapping subproblems**).

**Two styles:** **Top-down** (recurse + cache) vs **bottom-up** (fill a table **row by row**).

---

## Climbing stairs

**Layman:** Reach step `n` by taking **1 or 2** steps at a time. Ways to reach `n` = ways to `(n-1)` + ways to `(n-2)`—same idea as **Fibonacci**.

**Technical:** **O(n)** time, **O(1)** space with two variables.

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

**Layman:** Houses in a line; **can’t rob two neighbors**. At each house: **skip** it (take best so far) or **rob** it (add money + best from **two** houses back).

**Technical:** `dp[i] = max(dp[i-1], dp[i-2] + nums[i])`; rolling variables.

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

**Layman:** Keep **piles** of cards (patience): for each number, **place** it on the **leftmost** pile whose top is **≥** it. The number of piles = **LIS length**. Binary search finds the pile.

**Technical:** `tails` array + binary search; **O(n log n)**.

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

**Layman:** **One copy** of each item; bag has **weight limit**. Table: **best value** using first `i` items with capacity `w`—either **skip** item `i` or **take** it (if it fits).

**Technical:** 2D DP **O(n·W)**.

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

**Layman:** Turn **word1** into **word2** with **insert**, **delete**, **replace**—minimum **moves**. Table cell = cost to match **prefixes** of both words.

**Technical:** Match → free; else **1 + min** of three neighbors.

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

**Layman:** **Infinite** coins of each type; **fewest** coins to make `amount`. For each amount, try **last coin** being each denomination.

**Technical:** 1D DP **O(amount · coins)**.

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
