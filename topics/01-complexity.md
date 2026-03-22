# Complexity & Big O

This page explains **how long** a program takes and **how much memory** it needs as your data grows—first in **everyday language**, then with the usual **Big O** names so you can read any tutorial or interview question.

---

## In plain English: what are we even measuring?

Imagine your input is a **list of `n` items** (people in a queue, numbers in an array, lines in a file).

- **Time (speed):** If you double `n`, does the program roughly take **twice** as long? **Four times**? **Way more**? That relationship is what we call **time complexity**.
- **Space (memory):** Besides storing the input itself, do you need **extra** scratch space—like a notebook, a checklist, or a stack of sticky notes? That’s **space complexity**.

We usually care about **very large** inputs. We don’t fuss about “2× faster” constant tweaks when `n` is huge—we care whether work **scales like a straight line, a curve, or an explosion**.

**`n`** just means “how big the problem is” (often: length of an array, number of nodes, etc.).

---

## The “growth” idea (layman)

Ask: **If I make the input 10× bigger, what happens to the work?**

| You might say… | Rough meaning | Technical name |
|----------------|----------------|----------------|
| “Work stays the same—I only peek at one place.” | Does not depend on size | **O(1)** — *constant* |
| “I cut the problem in half each time—like guessing a number in a phone book.” | Doubling size only adds **one more** halving step | **O(log n)** — *logarithmic* |
| “I look at each item once.” | Work grows **in step** with size | **O(n)** — *linear* |
| “Sorting the good way—each item ‘pays’ a bit more than one pass.” | A bit worse than linear, still very practical | **O(n log n)** |
| “Every item talks to every other item (nested loops).” | Work grows like **size × size** | **O(n²)** — *quadratic* |
| “I try every combination of yes/no choices.” | Doubling size can **double the digits** of work | **O(2ⁿ)** — *exponential* |

**Memory analogy:** **O(1)** extra space is like using **one notepad** no matter how long the line is. **O(n)** extra is like writing **one note per person** in line.

---

## Big O intuition (same table, slightly tighter)

Big O is a **simple label** for “how fast the work grows,” ignoring small constants (we don’t say “exactly 3n steps,” we say “on the order of n”).

| Notation | Plain words | Example |
|----------|-------------|---------|
| **O(1)** | Same effort whether you have 10 or 10 million items (for that step) | Grab item by index in an array |
| **O(log n)** | Each step throws away about **half** the possibilities | Binary search in a sorted list |
| **O(n)** | One full walk through the data | Sum all numbers in an array |
| **O(n log n)** | Common “good” sorting / divide-and-conquer patterns | Merge sort, efficient sorts |
| **O(n²)** | Nested “for each… for each…” on the same data | All pairs from a list |
| **O(2ⁿ)** | Try all subsets / brute-force choices | Naive “try every combination” |

**Simple rules (still accurate):**

- **Drop constants:** “50n steps” and “5n steps” both grow **like n** → we write **O(n)**.
- **Keep the biggest term:** `n² + 100n` for huge `n` is dominated by **n²** → **O(n²)**.
- **Nested loops** often **multiply** cost; **one after another** is more like “take the heavier leg.”

---

## Code: what the computer is “doing” in simple terms

**One loop through the list** → “I touch each item once” → **O(n)**.

**Loop inside a loop on the same list** → “everyone shakes hands with everyone” → about **O(n²)**.

**Repeatedly cut the search range in half** → “phone book search” → **O(log n)**.

```javascript
// Layman: walk the line once and add numbers — effort grows with line length → O(n)
function sumArray(arr) {
  let s = 0;
  for (let i = 0; i < arr.length; i++) s += arr[i];
  return s;
}

// Layman: list every pair — outer and inner both go up to n → O(n²)
function allPairs(arr) {
  const out = [];
  for (let i = 0; i < arr.length; i++) {
    for (let j = i + 1; j < arr.length; j++) {
      out.push([arr[i], arr[j]]);
    }
  }
  return out;
}

// Layman: open to middle, discard wrong half, repeat — doubles of data only add steps → O(log n)
function binarySearch(sorted, target) {
  let lo = 0, hi = sorted.length - 1;
  while (lo <= hi) {
    const mid = (lo + hi) >>> 1;
    if (sorted[mid] === target) return mid;
    if (sorted[mid] < target) lo = mid + 1;
    else hi = mid - 1;
  }
  return -1;
}
```

`>>>` on the index math is a safe way to pick the middle index in JavaScript for nonnegative ranges.

---

## Space: “extra desk space,” not always the homework pile

- **The input** (the array they gave you) usually **doesn’t count** as “your invention”—you have to hold it anyway.
- **Extra space** means: new lists, hash tables, recursion **call stack** depth, etc.

**Recursion in plain words:** Each function call waits for the next one—like nested errands. A **deep** chain of calls uses more **stack** (memory). Shallow splitting (balanced divide-and-conquer) uses less depth than a long chain.

```javascript
// New array same size as input — “output” space; often mentioned separately from “scratch” space
function duplicate(arr) {
  return arr.map((x) => x);
}

// A Set remembering up to n things — extra memory grows with n → O(n) auxiliary
function usesExtraSet(n) {
  const seen = new Set();
  for (let i = 0; i < n; i++) seen.add(i);
  return seen.size;
}
```

---

## Amortized: “usually cheap, occasionally expensive”

**Layman:** Most days you buy a cheap coffee; once in a while you replace the whole machine. **On average per day**, it’s still cheap.

**Example:** Adding to a dynamic array is usually one write. Sometimes the array **runs out of room** and must **copy everything** to a bigger block—that’s expensive, but rare enough that **average** cost per add stays **low** (often treated like **O(1)** per add in the long run).

---

## Technical one-liner (for when you read other docs)

**Time complexity:** how step count **scales** with `n`. **Space complexity:** how **extra** memory scales. **Big O** describes that growth in the **worst case** unless stated otherwise (some problems discuss average case).

---

← [README](../README.md) · [Next: Arrays →](02-arrays.md)
