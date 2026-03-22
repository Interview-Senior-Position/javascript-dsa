# Trees & tries

**Tree:** Connected acyclic graph; rooted trees have a **root** and parent/child edges. **Binary tree:** at most two children per node.

**Binary search tree (BST):** For distinct keys, **left subtree** values are smaller than the root; **right subtree** values are larger. Enables **O(log n)** search in a **balanced** BST; skewed trees behave like linked lists (**O(n)**).

---

## TreeNode

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

**Inorder (left, root, right):** On a BST, visits nodes in **sorted order**.

**Preorder (root, left, right):** Useful for copying/serializing tree structure.

**Postorder (left, right, root):** Useful when deleting or computing bottom-up values.

**Level order (BFS):** Process level by level; uses a **queue**. **O(n)** time, **O(w)** extra space where `w` is max width.

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

**Idea:** Height is **1 + max** of children’s heights; empty tree has height **0**. Depth from root to node is path length.

```javascript
function maxDepth(root) {
  if (!root) return 0;
  return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
}
```

---

## Validate BST (bounds)

**Idea:** Each node must lie in an **open interval** `(min, max)` inherited from ancestors. Left child tightens the **upper** bound to `root.val`; right child tightens the **lower** bound to `root.val`. **O(n)** time.

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

**Idea:** Walk from root. If both `p` and `q` are smaller than root, LCA is in the **left** subtree; if both larger, in the **right**; otherwise the current root **separates** them and is the LCA. **O(h)** height time.

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

**Trie:** Each edge is labeled with a character; paths from root spell **prefixes**. Good for **prefix search**, autocomplete, and counting words with shared prefixes. **Insert/search** is **O(L)** for word length `L`.

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
