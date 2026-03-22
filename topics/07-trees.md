# Trees & tries

## In plain English

A **tree** is a **family chart**: one **root** (ancestor), branches to children, **no cycles** (you can’t loop back to an ancestor following only parent→child links).

A **binary tree** = at most **two** kids per node (left / right).

**Binary search tree (BST):** For distinct keys, **everything left** is smaller than this node; **everything right** is larger—like a **sorted phone book** folded in half repeatedly. **Balanced** tree → fast search; **skewed** (a chain) → behaves like a **linked list**.

---

## TreeNode

**Layman:** Each box holds a **value** and optional **left** and **right** child.

**Technical:** `TreeNode { val, left, right }`.

```javascript
class TreeNode {
  constructor(val = 0, left = null, right = null) {
    this.val = val;
    this.left = left;
    this.right = right;
  }
}
```

---

## Traversals

**Layman:**

- **Inorder:** Left subtree, **me**, right subtree — on a BST gives **sorted order** (small → big).
- **Preorder:** **Me** first, then left, then right — good for **copying** the shape (“save root, then children”).
- **Postorder:** Children first, **me** last — good for **deleting** from leaves up.
- **Level order:** **Row by row** (breadth-first) — like reading a stadium **level by level**.

**Technical:** DFS recursions **O(n)** time; level order uses a **queue**, **O(n)** time, **O(width)** extra space.

```javascript
function inorder(root, out = []) {
  if (!root) return out;
  inorder(root.left, out);
  out.push(root.val);
  inorder(root.right, out);
  return out;
}

function preorder(root, out = []) {
  if (!root) return out;
  out.push(root.val);
  preorder(root.left, out);
  preorder(root.right, out);
  return out;
}

function postorder(root, out = []) {
  if (!root) return out;
  postorder(root.left, out);
  postorder(root.right, out);
  out.push(root.val);
  return out;
}

function levelOrder(root) {
  if (!root) return [];
  const res = [];
  const q = [root];
  while (q.length) {
    const level = [];
    const n = q.length;
    for (let i = 0; i < n; i++) {
      const node = q.shift();
      level.push(node.val);
      if (node.left) q.push(node.left);
      if (node.right) q.push(node.right);
    }
    res.push(level);
  }
  return res;
}
```

---

## Height / depth

**Layman:** Height = **longest path** down to a leaf. Empty tree = **0** height.

**Technical:** `1 + max(left height, right height)`.

```javascript
function maxDepth(root) {
  if (!root) return 0;
  return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
}
```

---

## Validate BST (bounds)

**Layman:** Each node must stay inside a **valid range** passed down from parents. Left child must be **below** parent; right child **above**—and ranges **tighten** as you go.

**Technical:** `min/max` bounds; **O(n)**.

```javascript
function isValidBST(root, min = -Infinity, max = Infinity) {
  if (!root) return true;
  if (root.val <= min || root.val >= max) return false;
  return (
    isValidBST(root.left, min, root.val) &&
    isValidBST(root.right, root.val, max)
  );
}
```

---

## Lowest common ancestor (BST)

**Layman:** If both targets are **smaller** than root, go **left**; if both **larger**, go **right**. **Split** (one left, one right) → current root is the **first ancestor** shared by both.

**Technical:** **O(height)** time.

```javascript
function lowestCommonAncestor(root, p, q) {
  if (p.val < root.val && q.val < root.val) {
    return lowestCommonAncestor(root.left, p, q);
  }
  if (p.val > root.val && q.val > root.val) {
    return lowestCommonAncestor(root.right, p, q);
  }
  return root;
}
```

---

## Trie (prefix tree)

**Layman:** A **prefix tree** for words—each step down is a **letter**. Paths from the top spell **prefixes**. Great for autocomplete: “do any words start with **ca**?”

**Technical:** Insert/search **O(L)** for word length `L`.

```javascript
class TrieNode {
  constructor() {
    this.children = new Map();
    this.end = false;
  }
}

class Trie {
  constructor() {
    this.root = new TrieNode();
  }
  insert(word) {
    let node = this.root;
    for (const ch of word) {
      if (!node.children.has(ch)) node.children.set(ch, new TrieNode());
      node = node.children.get(ch);
    }
    node.end = true;
  }
  search(word) {
    let node = this.root;
    for (const ch of word) {
      if (!node.children.has(ch)) return false;
      node = node.children.get(ch);
    }
    return node.end;
  }
  startsWith(prefix) {
    let node = this.root;
    for (const ch of prefix) {
      if (!node.children.has(ch)) return false;
      node = node.children.get(ch);
    }
    return true;
  }
}
```

---

← [Previous: Hash maps & sets](06-hash-sets-maps.md) · [README](../README.md) · [Next: Heaps →](08-heaps.md)
