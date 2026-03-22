# Bit manipulation

## In plain English

Computers store numbers in **binary** (0s and 1s). **Bit tricks** answer questions like: **odd or even?** **power of two?** **which small set of items is selected?**—using **fast** integer operations.

**JavaScript note:** `& | ^ <<` work on **32-bit** chunks for normal numbers. Huge integers need **`BigInt`**.

---

## Basics

**Layman:** Think of a row of **on/off switches**. **AND** = both on; **OR** = at least one on; **XOR** = **different** (pairs cancel). **Shift** = move all bits left/right (like multiplying/dividing by 2 in fixed width).

**Technical:** `x & (x-1)` drops **lowest** 1 bit; useful for counting bits.

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

**Layman:** Powers of two have **exactly one** `1` in binary—**4** is `100`, **8** is `1000`. So `n` and `n-1` **don’t overlap** in bits.

**Technical:** `n > 0 && (n & (n-1)) === 0`.

```javascript
function isPowerOfTwo(n) {
  return n > 0 && (n & (n - 1)) === 0;
}
```

---

## Count set bits (Hamming weight)

**Layman:** Count **on** bits by repeatedly **turning off** the lowest `1` until zero.

**Technical:** Brian Kernighan style; **O(count of 1s)**.

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

**Layman:** **XOR** is like **toggle**: same number twice = **0**. XOR **everything** in the list—pairs **vanish**, lone number **remains**.

**Technical:** XOR associative/commutative; **O(n)**.

```javascript
function singleNumber(nums) {
  return nums.reduce((a, b) => a ^ b, 0);
}
```

---

## Subset enumeration with bitmask

**Layman:** `n` items → **2ⁿ** subsets. Loop a number from **0** to `2ⁿ-1`; **bit i** = “item i **in** or **out**.”

**Technical:** `mask & (1 << i)` tests bit `i`.

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

**Layman:** `>>>` fills with **zeros** on the left (good for **unsigned** view). `>>` keeps **sign** for negatives.

**Technical:** `>>> 0` casts to **unsigned** 32-bit.

```javascript
(-1 >>> 0) === 0xffffffff;
```

---

← [Previous: Greedy](14-greedy.md) · [README](../README.md) · [Next: Math & misc →](16-math-and-misc.md)
