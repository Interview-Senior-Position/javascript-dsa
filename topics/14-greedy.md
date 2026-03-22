# Greedy algorithms

## In plain English

**Greedy:** At each step, take what looks **best right now**—**local** best—without redoing past choices.

**Layman:** Hiking for **shortest time** today might **not** be global best if you need a **long climb** later—but **some** problems are designed so **simple local rules** still win globally.

**When it fails:** **0/1 knapsack** (whole items, weight limit)—local “take best value/weight” can miss the optimal combo. Use **DP** instead.

**Classic wins:** **Interval** scheduling (finish **earliest** first), **fractional** knapsack (you can take **fractions** of items).

---

## Activity selection / non-overlapping intervals

**Layman:** Meetings on a timeline—**maximize** how many you attend. **Pick the one that ends soonest** first; then **skip** anything overlapping. Repeat.

**Technical:** Sort by **end time**; greedy count; **O(n log n)** for sort.

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

**Layman:** How far can you **reach** from where you stand? Each step **extends** your reach. If you can’t reach the next index, you’re **stuck**.

**Technical:** Track **max reachable index**; **O(n)**.

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

**Layman:** Balloons are **intervals** on a line. **One arrow** at position `x` pops every balloon **covering** `x`. **Shoot** at the **rightmost** point you can while still covering the current cluster; **move** when a balloon starts **after** that.

**Technical:** Sort by start; merge overlap; **O(n log n)**.

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

**Layman:** You can take **pieces** of gold bars—**not** whole items only. **Take** the **best value per kilo** first until the bag is full.

**Technical:** Sort by **value/weight**; **optimal** for fractional; **wrong** for 0/1 knapsack.

```javascript
function fractionalKnapsack(items, W) {
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

**Layman:** Greedy = **fast** rule when the problem guarantees it. DP = **try** combinations with memory when **today’s choice** affects **tomorrow’s** options.

---

← [Previous: Dynamic programming](13-dynamic-programming.md) · [README](../README.md) · [Next: Bit manipulation →](15-bit-manipulation.md)
