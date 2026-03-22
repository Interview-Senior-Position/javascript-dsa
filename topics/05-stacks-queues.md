# Stacks & queues

## In plain English

**Stack:** Last in, first out—like a **stack of plates**: you only add/remove from the top.

**Queue:** First in, first out—like a **checkout line**: the person who arrived first leaves first.

**Layman:** In JS, `push`/`pop` at the **end** of an array is fast. **Taking from the front** (`shift`) shuffles everyone forward—**slow** for long lines. **Two stacks** can simulate a queue without shifting everyone each time.

---

## Stack (array)

**Layman:** Use for “**undo**,” matching **parentheses**, or “what did I see last?”

**Technical:** `push`/`pop` amortized **O(1)** at array end.

```javascript
const stack = [];
stack.push(1);
stack.push(2);
const top = stack[stack.length - 1];
const x = stack.pop();
```

---

## Queue with two stacks (amortized O(1))

**Layman:** **Inbox** and **outbox** trays. New mail goes to **inbox**. To serve, if **outbox** is empty, **flip** the whole inbox into the outbox (order reverses so oldest is on top). Each letter moves at most twice.

**Technical:** Two-stack queue; amortized **O(1)** per enqueue/dequeue.

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

**Layman:** Keep a **deck of indices** with decreasing heights. When a **taller** building shows up, it tells everyone shorter still in the deck “I’m your next taller neighbor to the right”—then they leave the deck.

**Technical:** Indices in **decreasing** value order; pop **smaller** tops when hitting larger `nums[i]`.

```javascript
function nextGreaterElements(nums) {
  const n = nums.length;
  const ans = Array(n).fill(-1);
  const st = [];
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

**Layman:** **Deque** = add/remove from **both** ends of a line. This version avoids shifting the whole array by tracking a **start offset** and occasionally **compacting**.

**Technical:** Double-ended queue; **compact** when front index drifts too far.

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

**Layman:** Read left to right. **Opening** bracket → push onto a **pile**. **Closing** bracket → the top of the pile must be the matching opener. End with an **empty** pile.

**Technical:** Stack **O(n)** time.

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
