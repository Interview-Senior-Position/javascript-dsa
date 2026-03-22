# Linked lists

**Structure:** Nodes hold a **value** and a **pointer** (`next`) to the next node. Unlike arrays, you cannot jump to the `i`-th element in O(1); you must walk from the head.

**Tradeoffs:** Insert/delete **after you have a pointer to a node** is **O(1)**. Finding a node by index is **O(n)**. Memory overhead: extra pointer per element vs contiguous array storage.

---

## Node & basic list

**Dummy head:** A placeholder node before the real head simplifies edge cases (empty list, inserting at head) because you always have a `cur.next` to attach to.

```javascript
class ListNode {
  constructor(val = 0, next = null) {
    this.val = val;
    this.next = next;
  }
}

function toArray(head) {
  const out = [];
  while (head) {
    out.push(head.val);
    head = head.next;
  }
  return out;
}

function fromArray(arr) {
  const dummy = new ListNode();
  let cur = dummy;
  for (const x of arr) {
    cur.next = new ListNode(x);
    cur = cur.next;
  }
  return dummy.next;
}
```

---

## Reverse linked list — iterative

**Idea:** Walk the list once. Keep `prev` (reversed part), `head` (current node), and `next`. Point `head.next` backward, then advance. **O(n)** time, **O(1)** space.

```javascript
function reverseList(head) {
  let prev = null;
  while (head) {
    const next = head.next;
    head.next = prev;
    prev = head;
    head = next;
  }
  return prev;
}
```

---

## Floyd’s cycle detection

**Idea:** **Slow** moves one step per iteration; **fast** moves two. If there is a cycle, fast will eventually lap slow and they meet inside the loop. If `fast` reaches `null`, there is no cycle. **O(n)** time, **O(1)** space (vs hashing visited nodes).

```javascript
function hasCycle(head) {
  let slow = head, fast = head;
  while (fast && fast.next) {
    slow = slow.next;
    fast = fast.next.next;
    if (slow === fast) return true;
  }
  return false;
}
```

---

## Merge two sorted lists

**Idea:** Like merge in merge-sort: repeatedly take the smaller head of the two lists and advance. A **dummy** node avoids special-casing the result head.

```javascript
function mergeTwoLists(a, b) {
  const dummy = new ListNode();
  let cur = dummy;
  while (a && b) {
    if (a.val <= b.val) {
      cur.next = a;
      a = a.next;
    } else {
      cur.next = b;
      b = b.next;
    }
    cur = cur.next;
  }
  cur.next = a || b;
  return dummy.next;
}
```

---

## Middle of linked list (slow/fast)

**Idea:** When `fast` reaches the end (moves 2 steps), `slow` (moves 1 step) is at the middle. For even length, this version returns the **second** middle node (common LeetCode convention).

```javascript
function middleNode(head) {
  let slow = head, fast = head;
  while (fast && fast.next) {
    slow = slow.next;
    fast = fast.next.next;
  }
  return slow;
}
```

---

← [Previous: Strings](03-strings.md) · [README](../README.md) · [Next: Stacks & queues →](05-stacks-queues.md)
