# Bit manipulation

**Uses:** Encode **small sets** as integers (bitmask), fast **parity** checks, **powers of two**, competitive-programming tricks, and low-level optimizations.

**JavaScript caveat:** Numbers are **IEEE-754 doubles**. Bitwise operators (`& | ^ ~ << >> >>>`) coerce operands to **32-bit signed integers** (except `>>>` which yields **unsigned** 32-bit). For larger integers use **`BigInt`**.

---

## Basics

**AND (`&`):** both bits 1 → 1. **OR (`|`):** either 1 → 1. **XOR (`^`):** bits differ → 1 (also “toggle” / “cancel pairs”). **Shifts:** multiply/divide by powers of two (with fixed width).

**Tricks:** `x & (x-1)` clears the **lowest set bit** (useful in counting bits). `x & -x` isolates lowest set bit (two’s complement).

```javascript
const x = 5; // 0b101
x & 1;       // lowest bit (odd/even)
x | (1 << i); // set bit i
x & ~(1 << i); // clear bit i
x ^ (1 << i); // flip bit i
(x & (x - 1)) // clear lowest set bit
```

---

## Check power of two

**Idea:** Powers of two have exactly **one** bit set, so `n & (n-1)` is **0** for `n > 0`.

```javascript
function isPowerOfTwo(n) {
  return n > 0 && (n & (n - 1)) === 0;
}
```

---

## Count set bits (Hamming weight)

**Idea:** Repeatedly clear lowest set bit; each iteration counts one **1**. **O(number of set bits)**.

```javascript
function hammingWeight(n) {
  n = n >>> 0; // unsigned 32-bit
  let c = 0;
  while (n) {
    n &= n - 1;
    c++;
  }
  return c;
}
```

---

## Single number (others appear twice)

**Idea:** XOR of a number with itself is **0**. XOR all elements; pairs cancel, the unique value remains.

```javascript
function singleNumber(nums) {
  return nums.reduce((a, b) => a ^ b, 0);
}
```

---

## Subset enumeration with bitmask

**Idea:** For `n` items, there are `2^n` subsets. Loop `mask` from `0` to `2^n - 1`; bit `i` means “include item `i`.”

```javascript
function subsetsBits(n) {
  const res = [];
  for (let mask = 0; mask < 1 << n; mask++) {
    const cur = [];
    for (let i = 0; i < n; i++) {
      if (mask & (1 << i)) cur.push(i);
    }
    res.push(cur);
  }
  return res;
}
```

---

## Note on `>>>` vs `>>`

**`>>`:** arithmetic right shift (sign extends for negative numbers). **`>>>`:** logical right shift—fills with **zeros**; use with `>>> 0` to get **unsigned** 32-bit representation.

```javascript
(-1 >>> 0) === 0xffffffff;
```

---

← [Previous: Greedy](14-greedy.md) · [README](../README.md) · [Next: Math & misc →](16-math-and-misc.md)
