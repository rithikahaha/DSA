# 05 · Binary Trees

Node = `val`, `left`, `right`. **Recursion recipe** (works for almost every tree question):
1. **Base case:** `if not node:` → return the answer for "nothing" (0, True, None...).
2. **Do the work for this one node.**
3. **Trust** recursion to solve `node.left` and `node.right`, then combine.
Don't try to picture the whole tree; think about one node.

---
## Maximum Depth
`depth(node) = 1 + max(depth(left), depth(right))`, and an empty tree has depth 0.
Trace `1 → (2, 3)`: depth(2)=1, depth(3)=1 → 1 + 1 = **2**.  ⏱ O(n) 💾 O(h) (h = height, from the call stack)

---
## Invert a Binary Tree
1. Empty → `None`.
2. Swap `root.left` and `root.right`.
3. Invert the left subtree, invert the right subtree.
4. Return `root`.

Trace `1 → (2,3)` → swap → `1 → (3,2)`.  ⏱ O(n)

---
## Same Tree
Two trees are equal if roots match and both children match.
1. Both empty → `True`. Only one empty → `False`. Values differ → `False`.
2. Otherwise → left pair same **and** right pair same.

---
## Subtree of Another Tree
1. If `root` is empty → `False` (nothing left to match).
2. If the tree starting at `root` **is the same as** `subRoot` (use `isSameTree` above) → `True`.
3. Otherwise try the left child or the right child as the new start.

Reuses Same Tree as a helper.  ⏱ O(n·m)

---
## Lowest Common Ancestor in a BST
BST rule: left values < node < right values. Use it to walk down:
1. Start at `root`.
2. Both `p` and `q` are **greater** than `cur` → both are on the right → go right.
3. Both **smaller** → go left.
4. Otherwise they split (or one equals `cur`) → `cur` is the LCA.

Trace: p=2, q=8, root=6 → 2<6<8 → they split → **6**.  ⏱ O(h) 💾 O(1)

---
## Validate BST
Common mistake: only comparing a node to its parent. Every node must fit within a **(low, high) range** inherited from all ancestors.
1. `valid(node, low, high)`; start with `(-inf, +inf)`.
2. Empty → `True`. `node.val` must be strictly between low and high, else `False`.
3. Left child gets range `(low, node.val)`; right child gets `(node.val, high)`.

Trace `5 → (1, 4)`: 4 is the right child, so it needs the range (5, inf); 4 < 5 → **False**.  ⏱ O(n)

---
## Level Order Traversal (BFS)
Idea: process the tree level by level using a **queue** (first in, first out).
1. Put `root` in the queue.
2. While the queue isn't empty: `qLen = len(q)` = the number of nodes in **this level**.
3. Pop exactly `qLen` nodes; record their values into `level`; push their children.
4. Add `level` to the result (if not empty).

Trace `3 → (9, 20)`: level 1 `[3]`, level 2 `[9,20]`.  ⏱ O(n) 💾 O(n)
`for i in range(qLen)` is the trick that separates levels.

---
## Kth Smallest in a BST
In-order traversal (left → node → right) of a BST visits values in **sorted order**. The k-th visited is the answer.
Iterative version with a stack:
1. Go as far **left** as possible, pushing nodes.
2. Pop one → this is the next smallest; `n += 1`. If `n == k` return it.
3. Move to its **right** child and repeat.

Trace `2 → (1, 3)`, k=2: push 2,1 → pop 1 (n=1) → go right (none) → pop 2 (n=2) → **2**.  ⏱ O(h + k)
