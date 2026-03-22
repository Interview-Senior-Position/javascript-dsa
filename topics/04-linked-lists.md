# Linked lists

## In plain English

A **linked list** is a **treasure hunt**: each box holds a **value** and a **clue** pointing to the next box. There is no “slot 7” in a book—you **follow clues** from the start (**slow** to reach the end).

**Layman:** Like a scavenger hunt vs a street with numbered houses (array). Inserting between two boxes is easy **if you already hold the clue**; finding the 100th box means walking.

**Technical:** No random access **O(1)**; index **O(n)**. Insert/delete at known node **O(1)**.

---

## Node & basic list

**Layman:** A **dummy** node is a fake “start” box so you always have something to hook the first real box onto—fewer special cases for “empty list.”

**Technical:** `ListNode { val, next }`; dummy head simplifies insertion.

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

**Layman:** Walk the chain; keep turning each arrow **backward** as you go. **Prev** is the reversed part; **next** is where you step before you break the link.

**Technical:** One pass **O(n)**, **O(1)** extra space.

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

**Layman:** Two runners on a **loop** track: one walks, one **runs**. If there’s a loop, the fast runner **laps** the slow one. If there’s an end, the fast runner **falls off** (`null`).

**Technical:** Tortoise and hare **O(n)** time, **O(1)** space—no hash set needed.

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

**Layman:** Two sorted lines of people; repeatedly take whoever has the **smaller** number at the front of their line.

**Technical:** Same merge step as merge-sort; dummy node avoids head edge cases.

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

**Layman:** When the **fast** walker reaches the finish (two steps per move), the **slow** walker (one step) is at the **middle**.

**Technical:** Even length: often returns **second** middle (common convention).

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
