# Stacks & queues

**Stack (LIFO):** Last item pushed is first popped—like a stack of plates. **Queue (FIFO):** First item in is first out—like a line.

**JavaScript note:** `array.push`/`pop` at the end give stack behavior in **O(1)** amortized. `array.shift()` from the front is **O(n)** on a plain array because elements shift. For a queue, use **two stacks**, a **deque** pattern, or a proper queue structure.

---

## Stack (array)

**Typical use:** Matching brackets, DFS, monotonic stack for “next greater element,” undo operations.

```javascript
const stack = [];
stack.push(1);
stack.push(2);
const top = stack[stack.length - 1];
const x = stack.pop();
```

---

## Queue with two stacks (amortized O(1))

**Idea:** **Enqueue** pushes onto `in`. **Dequeue** pops from `out`; if `out` is empty, **pour** all elements from `in` to `out` (reversing order so oldest ends up on top). Each element moves at most twice—**amortized** O(1) per operation.

```javascript
class QueueTwoStacks {
  constructor() {
    this.in = [];
    this.out = [];
  }
  _pour() {
    if (this.out.length === 0) {
      while (this.in.length) this.out.push(this.in.pop());
    }
  }
  enqueue(x) {
    this.in.push(x);
  }
  dequeue() {
    this._pour();
    return this.out.pop();
  }
  peek() {
    this._pour();
    return this.out[this.out.length - 1];
  }
}
```

---

## Monotonic stack (next greater element)

**Idea:** Keep indices in **decreasing** value order. When you see a larger value, it is the “next greater” for everything smaller you pop. Left-to-right scan yields **next greater to the right** variants; you can adapt for previous greater, next smaller, etc.

```javascript
function nextGreaterElements(nums) {
  const n = nums.length;
  const ans = Array(n).fill(-1);
  const st = []; // indices, decreasing values
  for (let i = 0; i < n; i++) {
    while (st.length && nums[st[st.length - 1]] < nums[i]) {
      ans[st.pop()] = nums[i];
    }
    st.push(i);
  }
  return ans;
}
```

---

## Deque pattern with circular buffer (fixed max size optional)

**Idea:** Allow efficient push/pop at both ends. This version uses a **lo** pointer so `popFront` can advance without shifting the whole array; **compact** occasionally to reclaim space.

```javascript
class Deque {
  constructor() {
    this.a = [];
    this.lo = 0;
  }
  pushFront(x) {
    this.lo--;
    this.a[this.lo] = x;
  }
  pushBack(x) {
    this.a.push(x);
  }
  popFront() {
    if (this.isEmpty()) return undefined;
    const v = this.a[this.lo++];
    if (this.lo > 1000 && this.lo > this.a.length / 2) this._compact();
    return v;
  }
  popBack() {
    return this.a.pop();
  }
  isEmpty() {
    return this.lo >= this.a.length;
  }
  _compact() {
    this.a = this.a.slice(this.lo);
    this.lo = 0;
  }
}
```

---

## Valid parentheses

**Idea:** On an opening bracket, **push** the expected closing type (or the opener). On closing, the stack top must **match**; otherwise invalid. End with an **empty** stack.

```javascript
function isValid(s) {
  const map = { ")": "(", "]": "[", "}": "{" };
  const st = [];
  for (const ch of s) {
    if (ch === "(" || ch === "[" || ch === "{") st.push(ch);
    else {
      if (!st.length || st.pop() !== map[ch]) return false;
    }
  }
  return st.length === 0;
}
```

---

← [Previous: Linked lists](04-linked-lists.md) · [README](../README.md) · [Next: Hash maps & sets →](06-hash-sets-maps.md)
