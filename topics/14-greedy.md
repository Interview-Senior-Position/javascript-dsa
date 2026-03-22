# Greedy algorithms

**Greedy:** At each step, take the **locally best** choice hoping it leads to a global optimum.

**When it works:** The problem has **greedy choice property** (a globally optimal solution can be built from locally optimal choices) and often **optimal substructure**. If unsure, compare with **DP** or try to **prove** exchange argument / stay-ahead proof.

**Classic patterns:** Interval scheduling (earliest finish time), minimum arrows on overlapping intervals, fractional knapsack by **value/weight** ratio.

---

## Activity selection / non-overlapping intervals

**Idea:** Sort by **end time**. Greedily keep the interval with the **earliest end** that doesn’t overlap the last chosen—maximizes room for future intervals.

```javascript
function eraseOverlapIntervals(intervals) {
  intervals.sort((a, b) => a[1] - b[1]);
  let end = -Infinity, removed = 0;
  for (const [s, e] of intervals) {
    if (s >= end) end = e;
    else removed++;
  }
  return removed;
}
```

---

## Jump game (can reach last index?)

**Idea:** Track the **farthest index** reachable from `[0..i]`. If you ever land past the last index’s reach, you can’t proceed.

```javascript
function canJump(nums) {
  let reach = 0;
  for (let i = 0; i < nums.length; i++) {
    if (i > reach) return false;
    reach = Math.max(reach, i + nums[i]);
  }
  return true;
}
```

---

## Minimum arrows to burst balloons (intervals)

**Idea:** Sort by **start**. One arrow at position `end` bursts all overlapping balloons; when a balloon starts **after** `end`, shoot a new arrow and update `end` to that balloon’s minimum end.

```javascript
function findMinArrowShots(points) {
  if (!points.length) return 0;
  points.sort((a, b) => a[0] - b[0]);
  let arrows = 1;
  let end = points[0][1];
  for (let i = 1; i < points.length; i++) {
    if (points[i][0] > end) {
      arrows++;
      end = points[i][1];
    } else {
      end = Math.min(end, points[i][1]);
    }
  }
  return arrows;
}
```

---

## Fractional knapsack (concept — use value/weight ratio)

**Idea:** You may take **fractions** of items. Sort by **value per weight** descending; take full items while capacity remains, then fill remainder of best ratio item. **Optimal** for fractional; **not** for 0/1 knapsack.

```javascript
function fractionalKnapsack(items, W) {
  // items: { weight, value }
  items.sort((a, b) => b.value / b.weight - a.value / a.weight);
  let profit = 0, w = 0;
  for (const it of items) {
    if (w + it.weight <= W) {
      w += it.weight;
      profit += it.value;
    } else {
      const remain = W - w;
      profit += (it.value / it.weight) * remain;
      break;
    }
  }
  return profit;
}
```

---

## Greedy vs DP

**Rule of thumb:** Greedy is **faster** when valid; DP handles **constraints** where local choices interact (e.g. **0/1 knapsack**). If a counterexample breaks your greedy, switch to DP or exhaustive search with pruning.

---

← [Previous: Dynamic programming](13-dynamic-programming.md) · [README](../README.md) · [Next: Bit manipulation →](15-bit-manipulation.md)
