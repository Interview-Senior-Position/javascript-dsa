# Hash maps & sets

## In plain English

A **hash map** (dictionary) is a **label → drawer** system: “student ID → backpack.” You don’t scan every drawer—**average** lookup is **instant** (like knowing the shelf number from a code).

A **set** is “**have I seen this before?**” with no duplicates—like a **guest list** at the door.

**Layman:** Collisions (two labels hashing to the same slot) are handled inside the engine; **rare** with good hashing. Worst cases can be slow but **usually** you ignore that in interviews.

**Technical:** Average **O(1)** insert/lookup/delete; `Map` in JS keeps insertion order and supports any key type.

---

## Map & Set basics

**Layman:** Counting votes, remembering “last time we saw this number,” or “does this exist?”

**Technical:** `Map` for key–value; `Set` for membership.

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

**Layman:** First pass: tally how many of each letter. Second pass: walk the string in order and return the **first** letter with count **1**.

**Technical:** **O(n)** time, **O(alphabet)** space.

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

**Layman:** Walking the list, you need **target − current number**. If that **partner** was already seen, you found the pair. **Notebook:** “number → position I saw it.”

**Technical:** Hash map **value → index**; **O(n)** one pass.

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

**Layman:** Sort the letters of each word—**listen** and **silent** become the same **signature**. Bucket words with the same signature.

**Technical:** Key = sorted letters string; **O(n · k log k)** for `n` words length `k` with sort.

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

**Layman:** **LRU** = when the room is full, throw out whatever you **used longest ago**. **Touch** something → move it to “most recently used” side (in JS `Map`, re-inserting updates order).

**Technical:** `get` delete+set; `put` evict **first** key when over capacity.

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
