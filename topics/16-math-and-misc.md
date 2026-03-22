# Math, number theory & misc

**Common themes:** **Divisibility** (GCD/LCM), **primes** (sieve), **modular arithmetic** (avoid overflow, fast pow), **combinatorics** (binomial coefficients). In JS, watch **Number.MAX_SAFE_INTEGER**; use **`BigInt`** for exact large integers.

---

## GCD & LCM (Euclidean algorithm)

**GCD:** Greatest common divisor of `a` and `b`. **Euclidean algorithm:** `gcd(a,b) = gcd(b, a mod b)` until `b = 0`. **LCM:** `lcm(a,b) = |a*b| / gcd(a,b)` (use division after GCD to reduce overflow risk).

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

**Idea:** Mark multiples of each prime starting at `p²` (smaller multiples already handled). **O(n log log n)** time, **O(n)** space.

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

**Idea:** Write exponent in binary. Square the base and multiply result when the current bit is 1—**O(log exp)** multiplications. Essential for large powers under a modulus.

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

**Idea:** Pascal’s rule `C(n,k) = C(n-1,k-1) + C(n-1,k)`. Build a table up to `maxN`. Use modulo if answers must fit in a fixed range.

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

**Safe integers in JS:** `±(2^53 - 1)`. Beyond that, `Number` loses precision. **`BigInt`** literals end with `n`; use for exact arithmetic at the cost of no mixing with `Number` without conversion.

```javascript
const x = 9007199254740993n; // larger than Number.MAX_SAFE_INTEGER
const y = x * 2n;
```

---

← [Previous: Bit manipulation](15-bit-manipulation.md) · [README](../README.md) · [Back to index](../README.md)
