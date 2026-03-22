# Strings

## In plain English

Text is a **sequence of characters**. In JavaScript, **you cannot edit a string in place**—any “change” builds a **new** string. **Glueing many tiny pieces** in a loop (`s += ch`) can copy the whole thing over and over (**slow**). **Build a list of parts, then `join` once**—like writing on scrap paper and stapling at the end.

---

## Frequency maps

**Layman:** Count how many of each letter—like tally marks for a spelling quiz. **Anagrams** are two words with the **same letter counts** (order doesn’t matter).

**Technical:** `Map` or fixed array for alphabet; **O(n)** time. Pairwise cancel counts with +1 / -1 in one pass.

```javascript
function charFrequency(s) {
  const map = new Map();
  for (const ch of s) {
    map.set(ch, (map.get(ch) ?? 0) + 1);
  }
  return map;
}

// Layman: same letters, same counts → anagram; O(n)
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

**Layman:** A **palindrome** reads the same forwards and backwards (“racecar”). After stripping junk and lowercasing, **compare outside-in**—like closing a book from both covers toward the middle.

**Technical:** Normalize, then two pointers **O(n)** time; extra **O(n)** if you build a cleaned string.

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

**Layman:** Start with the first word as your guess for “prefix everyone shares.” For each next word, **chop letters from the end** of the guess until it matches the start of that word.

**Technical:** **O(total characters)** worst case across all strings.

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

**Layman:** Turn a chunk of text into a **fingerprint** (number). Slide the window: subtract the contribution of the outgoing character, add the incoming one. If fingerprints match, **double-check** the actual text (fingerprints can collide).

**Technical:** Rolling hash **O(n)** for substring search; **not** for cryptography.

```javascript
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

**Layman:** Don’t tape a **growing** poster every time you add one letter—collect pieces, **join once**.

**Technical:** Repeated `+=` on strings may **O(n²)**; `array.join` is **O(n)**.

```javascript
function buildString(parts) {
  return parts.join("");
}
```

---

← [Previous: Arrays](02-arrays.md) · [README](../README.md) · [Next: Linked lists →](04-linked-lists.md)
