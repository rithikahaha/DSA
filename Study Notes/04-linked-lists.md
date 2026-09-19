# 04 · Linked Lists

A node is `val` + `next` (pointer to the next node). You can't jump to index i; you only follow `.next`.
Three tools cover almost everything:
- **Dummy node:** a fake node before the head so you never special-case "the head changed".
- **Slow/fast pointers:** slow moves 1, fast moves 2 → fast reaches the end when slow is in the middle.
- **prev / curr / next:** the reversal trio.

---
## Reverse a Linked List
Idea: flip each arrow to point backwards.
```python
prev, curr = None, head
while curr:
    nxt = curr.next     # 1. save the rest (before we break the link)
    curr.next = prev    # 2. flip the arrow
    prev = curr         # 3. step prev forward
    curr = nxt          # 4. step curr forward
return prev             # prev ends on the new head
```
Trace `1→2→3`: after loop 1: `None←1  2→3`; loop 2: `None←1←2  3`; loop 3 → `None←1←2←3`, return 3.  ⏱ O(n) 💾 O(1)
**Always save `next` first**, or you lose the rest of the list.

---
## Merge Two Sorted Linked Lists
1. `dummy` node + `tail` pointer (where the merged list ends).
2. While both lists have nodes: attach the **smaller** head to `tail.next`, advance that list, advance `tail`.
3. One list runs out → attach the remainder of the other (it's already sorted).
4. Return `dummy.next`.

Trace `1→3` and `2`: pick 1, pick 2, list2 empty → attach `3` → `1→2→3`.  ⏱ O(n+m) 💾 O(1)

---
## Linked List Cycle Detection
Idea (Floyd's tortoise & hare): if there's a loop, the fast pointer eventually laps the slow one and they **meet**.
1. `slow` = 1 step, `fast` = 2 steps.
2. If `fast` or `fast.next` becomes `None` → the list ends → no cycle.
3. If `slow == fast` → cycle.

Analogy: two runners on a circular track; the faster one must catch up.  ⏱ O(n) 💾 O(1) (a set of visited nodes also works but uses O(n) memory).

---
## Remove Nth Node From End
Problem: you don't know the length. Idea: keep two pointers **n apart**; when the front hits the end, the back is right before the target.
1. `dummy → head`; `left = dummy`, `right = head`.
2. Move `right` forward `n` steps (gap = n).
3. Move both until `right` is `None`. Now `left` sits just **before** the node to delete.
4. `left.next = left.next.next` (skip it). Return `dummy.next`.

Dummy handles the case where the removed node is the head.
Trace `1→2→3→4`, n=2: right moves to 3; move both: right→4→None, left→1→2; left=2 → delete 3 → `1→2→4`.  ⏱ O(n) 💾 O(1)

---
## Reorder List
Goal: `1→2→3→4→5` becomes `1→5→2→4→3` (front, back, front, back...).
Three steps:
1. **Find the middle** with slow/fast.
2. **Reverse the second half** (the reversal trio above; set `slow.next = None` to cut the list in two).
3. **Merge alternately:** take one from `first`, one from `second`; save both `next`s before relinking.

Trace `1→2→3→4`: halves `1→2` and reversed `4→3` → weave: 1,4,2,3.  ⏱ O(n) 💾 O(1)
This one is just three earlier problems glued together.
