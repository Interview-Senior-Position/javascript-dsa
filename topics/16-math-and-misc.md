# Math, number theory & misc

## In plain English

These are **number tricks** for **divisibility**, **primes**, **big powers mod something**, and **combinations** (how many ways to choose). **Interviews** and **contests** use them often.

**JavaScript:** Normal numbers **lose precision** past **~9 × 10¹⁵**; use **`BigInt`** for exact huge integers.

---

## GCD & LCM (Euclidean algorithm)

**Layman:** **GCD** = largest number that **divides both** (tiles on a floor?). **Euclidean trick:** `gcd(a,b)` = `gcd(b, remainder of a÷b)` until remainder **0**. **LCM** = “how often both cycles align”—product divided by GCD.

**Technical:** Euclidean algorithm **O(log min(a,b))**.

```javascript
function gcd(a, b) {
  a = Math.abs(a);
  b = Math.abs(b);
  while (b) [a, b] = [b, a % b];
  return a || 0;
}

function lcm(a, b) {
  if (!a || !b) return 0;
  return (a / gcd(a, b)) * b;
}
```

---

## Sieve of Eratosthenes (primes up to n)

**Layman:** List **2..n**. Cross out **multiples** of 2, then 3, then 5… **Skip** primes already crossed. What’s left = **primes**.

**Technical:** **O(n log log n)** time, **O(n)** space.

```javascript
function sieve(n) {
  const isPrime = Array(n + 1).fill(true);
  isPrime[0] = isPrime[1] = false;
  for (let i = 2; i * i <= n; i++) {
    if (isPrime[i]) {
      for (let j = i * i; j <= n; j += i) isPrime[j] = false;
    }
  }
  const primes = [];
  for (let i = 2; i <= n; i++) if (isPrime[i]) primes.push(i);
  return primes;
}
```

---

## Fast exponentiation (mod)

**Layman:** Computing `base^exp` by multiplying **exp times** is slow. Write **exp** in binary: **square** each step and **multiply** when the bit is 1—like **Russian peasant** multiplication.

**Technical:** **O(log exp)**; **mod** keeps numbers small.

```javascript
function modPow(base, exp, mod) {
  let res = 1 % mod;
  base %= mod;
  while (exp > 0) {
    if (exp & 1) res = (res * base) % mod;
    base = (base * base) % mod;
    exp >>= 1;
  }
  return res;
}
```

---

## Combinations (n choose k) with mod — small table (Pascal)

**Layman:** “**n choose k**” = ways to pick **k** people from **n** without order. **Pascal’s triangle:** each cell = **sum of two above**—build triangle row by row.

**Technical:** Pascal’s rule; table **O(n²)**.

```javascript
function binomialTable(maxN, mod) {
  const C = Array.from({ length: maxN + 1 }, () => Array(maxN + 1).fill(0));
  for (let n = 0; n <= maxN; n++) {
    C[n][0] = C[n][n] = 1;
    for (let k = 1; k < n; k++) {
      C[n][k] = (C[n - 1][k - 1] + C[n - 1][k]) % mod;
    }
  }
  return C;
}
```

---

## BigInt (when integers exceed safe integer)

**Layman:** JS **safe** integers stop around **9 quadrillion**. Bigger whole numbers? **`BigInt`**—no decimals, **exact** arithmetic.

**Technical:** `suffix n` on literals; don’t mix with `Number` without conversion.

```javascript
const x = 9007199254740993n; // larger than Number.MAX_SAFE_INTEGER
const y = x * 2n;
```

---

← [Previous: Bit manipulation](15-bit-manipulation.md) · [README](../README.md) · [Back to index](../README.md)
