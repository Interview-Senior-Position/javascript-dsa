# Hash maps & sets

**Hash table:** Maps keys to values using a hash function and buckets. **Average** insert, lookup, and delete are **O(1)**; **worst case** (many collisions) can degrade to **O(n)**—rare with a good implementation.

**In JavaScript:** `Map` keeps **insertion order**, accepts **any** key type, and avoids prototype pitfalls of plain objects. `Object` is fine for string keys. `Set` stores **unique** values with similar complexity.

---

## Map & Set basics

**When to use:** Counting frequencies, memoization, deduplication, O(1) membership tests, mapping values to indices (e.g. two-sum with one pass).

```javascript
const freq = new Map();
for (const x of [1, 2, 2, 3]) {
  freq.set(x, (freq.get(x) ?? 0) + 1);
}

const seen = new Set();
seen.add(5);
seen.has(5); // true
```

---

## First unique character

**Idea:** First pass counts occurrences; second pass finds the **first** index whose count is 1. **O(n)** time, **O(alphabet)** space.

```javascript
function firstUniqChar(s) {
  const count = new Map();
  for (const ch of s) count.set(ch, (count.get(ch) ?? 0) + 1);
  for (let i = 0; i < s.length; i++) {
    if (count.get(s[i]) === 1) return i;
  }
  return -1;
}
```

---

## Two sum (value → index)

**Idea:** For each `nums[i]`, you need **`target - nums[i]`** seen earlier. Store **value → index** as you walk; if `need` is in the map, return both indices. **O(n)** time, **O(n)** space.

```javascript
function twoSum(nums, target) {
  const map = new Map();
  for (let i = 0; i < nums.length; i++) {
    const need = target - nums[i];
    if (map.has(need)) return [map.get(need), i];
    map.set(nums[i], i);
  }
  return null;
}
```

---

## Group anagrams

**Idea:** Anagrams share the same **sorted letters** (or same frequency signature). Use that string as a **hash key** to bucket words. Sorting each word costs **O(k log k)** per word of length `k`; total depends on total characters.

```javascript
function groupAnagrams(strs) {
  const map = new Map();
  for (const w of strs) {
    const key = [...w].sort().join("");
    if (!map.has(key)) map.set(key, []);
    map.get(key).push(w);
  }
  return [...map.values()];
}
```

---

## LRU cache (concept: Map as ordered + capacity)

**LRU (least recently used):** When full, evict the item **not used for the longest time**.

**Trick in JS:** `Map` iteration order follows insertion. On **get**, **delete** and **re-insert** to mark as most recently used. On **put**, if over capacity, delete the **first** key (`map.keys().next().value`) as LRU.

```javascript
class LRUCache {
  constructor(capacity) {
    this.cap = capacity;
    this.map = new Map();
  }
  get(key) {
    if (!this.map.has(key)) return -1;
    const val = this.map.get(key);
    this.map.delete(key);
    this.map.set(key, val);
    return val;
  }
  put(key, value) {
    if (this.map.has(key)) this.map.delete(key);
    this.map.set(key, value);
    if (this.map.size > this.cap) {
      const oldest = this.map.keys().next().value;
      this.map.delete(oldest);
    }
  }
}
```

---

← [Previous: Stacks & queues](05-stacks-queues.md) · [README](../README.md) · [Next: Trees & tries →](07-trees.md)
