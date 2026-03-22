# Strings

**Immutability:** In JavaScript, string values cannot be changed in place. Operations like `slice` or `replace` return **new** strings. For many small concatenations in a loop, repeated copying can become **O(n²)** total time; prefer **`join`** on an array of parts or a single `Array` buffer.

**When to use what:** Frequency maps solve “same multiset of characters” problems; two pointers solve palindromes and many O(n) scans on normalized text.

---

## Frequency maps

**Idea:** Count how often each character appears. A `Map` (or array for limited alphabet) gives **O(n)** counting and **O(1)** expected lookups.

**Anagrams:** Two strings are anagrams if every character count matches. You can increment for one string and decrement for the other in one pass; if all counts end at zero, they match.

```javascript
function charFrequency(s) {
  const map = new Map();
  for (const ch of s) {
    map.set(ch, (map.get(ch) ?? 0) + 1);
  }
  return map;
}

// Anagram check — O(n) time, O(alphabet) space
function isAnagram(a, b) {
  if (a.length !== b.length) return false;
  const count = new Map();
  for (let i = 0; i < a.length; i++) {
    count.set(a[i], (count.get(a[i]) ?? 0) + 1);
    count.set(b[i], (count.get(b[i]) ?? 0) - 1);
  }
  for (const v of count.values()) if (v !== 0) return false;
  return true;
}
```

---

## Palindrome / two pointers on string

**Idea:** After **normalizing** (lowercase, strip non-alphanumeric if required), compare characters from both ends inward. **O(n)** time, **O(n)** extra space if you build a new string; two pointers on the normalized string only need **O(1)** extra if you index the original carefully.

```javascript
function isPalindrome(s) {
  const t = s.toLowerCase().replace(/[^a-z0-9]/g, "");
  let i = 0, j = t.length - 1;
  while (i < j) {
    if (t[i++] !== t[j--]) return false;
  }
  return true;
}
```

---

## Longest common prefix (array of strings)

**Idea:** Take the first string as the candidate prefix. For each other string, shrink the prefix until it is a prefix of that string (or empty). **O(S)** where `S` is total characters across all strings in the worst case.

```javascript
function longestCommonPrefix(strs) {
  if (!strs.length) return "";
  let prefix = strs[0];
  for (let i = 1; i < strs.length; i++) {
    while (strs[i].indexOf(prefix) !== 0) {
      prefix = prefix.slice(0, -1);
      if (!prefix) return "";
    }
  }
  return prefix;
}
```

---

## Rabin–Karp style rolling hash (concept)

**Idea:** Treat a substring as a number in base 256 (or similar), hash it, and **slide** the window: subtract the contribution of the outgoing character and add the incoming one. Hash collisions are possible, so compare the actual substring when hashes match. Not for cryptography; fine for competitive / interview patterns.

```javascript
// Simple numeric hash for substring compare (not cryptographically secure)
function strStr(haystack, needle) {
  if (!needle.length) return 0;
  const BASE = 256;
  const MOD = 1e9 + 7;
  let h = 0, n = 0, pow = 1;
  for (let i = 0; i < needle.length; i++) {
    n = (n * BASE + needle.charCodeAt(i)) % MOD;
    if (i) pow = (pow * BASE) % MOD;
  }
  for (let i = 0; i < needle.length; i++) {
    h = (h * BASE + haystack.charCodeAt(i)) % MOD;
  }
  if (h === n && haystack.slice(0, needle.length) === needle) return 0;
  for (let i = needle.length; i < haystack.length; i++) {
    h = (h - haystack.charCodeAt(i - needle.length) * pow % MOD + MOD) % MOD;
    h = (h * BASE + haystack.charCodeAt(i)) % MOD;
    if (h === n && haystack.slice(i - needle.length + 1, i + 1) === needle) {
      return i - needle.length + 1;
    }
  }
  return -1;
}
```

---

## Builder pattern (avoid O(n²) concatenation)

**Why:** `s += ch` in a loop may copy the growing string each time. Collect parts in an array and **`join`** once, or use a single preallocated strategy if the size is known.

```javascript
// Bad in a loop: s += ch (many reallocations)
// Good:
function buildString(parts) {
  return parts.join("");
}
```

---

← [Previous: Arrays](02-arrays.md) · [README](../README.md) · [Next: Linked lists →](04-linked-lists.md)
