# How to build DSA logic (read this first)

Every problem here is one of ~10 **patterns**. Your job isn't to invent a solution; it's to
recognise the pattern. Before coding, ask these in order:

1. **What's the brute force?** (Usually "try every pair / every subarray" = slow, O(n²).)
2. **What work am I repeating?** The pattern is the trick that removes the repeat.
3. **What does the input look like?** Use the table below.

| If you see...                                   | Try this pattern            | Notes file |
|-------------------------------------------------|-----------------------------|------------|
| "Have I seen this before?", counting, grouping  | Hash map / set              | 01 |
| Sorted array, pairs/triples, palindromes        | Two pointers                | 02 |
| Longest/shortest **contiguous** substring/subarray | Sliding window           | 02 |
| Sorted (or rotated sorted) + "find"             | Binary search               | 03 |
| Linked list: cycle, middle, nth from end, reverse | Slow/fast pointers, dummy node, `prev/curr/next` | 04 |
| Tree                                            | Recursion (base case + trust children) / BFS queue | 05 |
| Intervals / meetings                            | Sort by start, compare neighbours | 06 |
| "Max/min/number of ways", choice at each step   | Dynamic programming (DP)    | 06 |
| Bits, no `+`, count 1s                          | Bit manipulation            | 06 |
| Brackets, "most recent thing matters"           | Stack                       | 01 |

## Habits that fix "I can't come up with the logic"
- **Draw a tiny example by hand** (3-5 items) and write each step you do. That *is* the algorithm.
- **Write the brute force first**, then ask "what can I remember so I don't recompute?"
- **Trace variables in a table** line by line. Don't "run it in your head"; write it down.
- **Recursion:** only think about ONE node. (1) base case, (2) what do I do here, (3) trust the recursive call for the rest.
- **After solving, re-solve from a blank page in 2-3 days.** If you can't, you memorised, not learned.
- Learn the **pattern**, not the problem. "Sliding window" transfers; "Longest Substring" doesn't.

## Recommended order (easiest thinking first)
01 Hashing & Stack → 02 Pointers & Window → 03 Binary Search → 04 Linked Lists → 05 Trees → 06 Intervals, DP, Bits

Complexity key: **n** = input size. O(1) = constant memory, O(n) = one pass, O(n²) = nested loops, O(log n) = halving each step.
