# 01 · Hashing & Stack

**Core idea of hashing:** a `set`/`dict` answers "have I seen X?" in O(1), so you can replace an inner loop with a lookup.

---
## Duplicate Integer
**Ask:** does any value appear twice?
```python
seen = set()
for n in nums:
    if n in seen: return True
    seen.add(n)
return False
```
1. Keep a set of numbers seen so far.
2. For each number: already in set → duplicate, done.
3. Otherwise add it. Finish loop → no duplicates.

Trace `[1,2,1]`: seen={} → add 1 → add 2 → 1 is in seen → **True**.  ⏱ O(n) 💾 O(n)
(Brute force compared every pair: O(n²).)

---
## Valid Anagram
**Ask:** same letters, same counts?
1. Different lengths → `False` immediately.
2. Build a letter→count dict for `s` and for `t`.
3. Every letter's count in `s` must equal its count in `t` (`.get(c, 0)` avoids a crash if missing).

Trace `"rat"`,`"tar"`: both → {r:1,a:1,t:1} → equal → **True**.  ⏱ O(n) 💾 O(1) (≤26 letters)

---
## Two Sum
**Ask:** find two indices whose values add to `target`.
```python
prev = {}                       # value -> index
for i, n in enumerate(nums):
    diff = target - n           # the partner I need
    if diff in prev: return [prev[diff], i]
    prev[n] = i
```
1. For each `n`, the partner is `target - n`.
2. Have I already seen that partner? Yes → answer = (its index, my index).
3. No → store `n` and its index for future numbers.

Trace `[3,4,5,6]`, target 7: n=3 need 4 (no) store 3 → n=4 need 3 (**yes**, index 0) → `[0,1]`.  ⏱ O(n) 💾 O(n)
Key trick: **store as you go**, so you only ever look backwards.

---
## Group Anagrams
**Ask:** bucket words that are anagrams.
1. Two words are anagrams ⇔ they have identical letter counts.
2. For each word, make a 26-slot count list (`ord(c) - ord('a')` = the letter's slot).
3. Lists can't be dict keys → convert to `tuple`.
4. `res[tuple(count)].append(word)`; same key = same group.
5. Return `res.values()`.

Trace `"eat"`,`"tea"` → both give the same tuple → same bucket.  ⏱ O(n·L) 💾 O(n·L)

---
## Top K Frequent Elements
**Ask:** the `k` most common numbers.
1. Count each number: `count[n]`.
2. Make `freq`, a list of buckets where **index = how many times**; put each number in `freq[its count]`. (Max count is `len(nums)`, hence that size.)
3. Walk `freq` from the **end** (highest counts) to the start, collecting numbers until you have `k`.

Trace `[1,1,1,2,2,3]`, k=2: counts {1:3,2:2,3:1} → freq[3]=[1], freq[2]=[2], freq[1]=[3] → walk back: 1, then 2 → **[1,2]**.
⏱ O(n) 💾 O(n)  (Beats sorting, which is O(n log n).) Pattern: *bucket sort*.

---
## Product of Array Except Self
**Ask:** for each index, the product of everything else, **no division**.
Idea: answer[i] = (product of everything **left** of i) × (product of everything **right** of i).
1. Left→right: `res[i] = prefix`, then `prefix *= nums[i]`. (Store first, multiply after, so `nums[i]` itself is excluded.)
2. Right→left: `res[i] *= postfix`, then `postfix *= nums[i]`.

Trace `[1,2,3,4]`: after pass 1 `res=[1,1,2,6]` (products of everything to the left). Pass 2 goes from the right with postfix = 1, 4, 12, 24 → `res=[24,12,8,6]`.
⏱ O(n) 💾 O(1) extra (output doesn't count).

---
## Longest Consecutive Sequence
**Ask:** longest run like 3,4,5,6 (order in the array doesn't matter), in O(n).
1. Put all numbers in a set.
2. Only **start counting at a sequence's beginning**: `n` is a start if `n-1` is **not** in the set.
3. From a start, keep checking `n+1, n+2, ...` in the set, counting the length.
4. Keep the max.

Why O(n)? Each number is walked at most once, because non-starts are skipped.
Trace `[2,20,4,10,3,4,5]`: start at 2 (1 missing) → 2,3,4,5 → length **4**.

---
## Encode & Decode Strings
**Ask:** turn a list of strings into ONE string and back (strings can contain any character).
Idea: prefix each string with **its length + `#`**, so you never need to guess where it ends.
- Encode `["hi","yo"]` → `"2#hi2#yo"`.
- Decode: at position `i`, walk `j` until `#`; `int(s[i:j])` is the length; the word is the next `length` characters; jump `i` past it.

Length-prefix is what makes it safe even if a word contains `#`.  ⏱ O(total chars)

---
## Valid Parentheses (Stack)
**Ask:** are brackets closed in the right order?
Idea: the **most recent open bracket must close first** = a stack (last in, first out).
1. Map each closer to its opener: `) → (`.
2. For each char: if it's an opener → push.
3. If it's a closer → the top of the stack must be its matching opener; pop it. Otherwise (empty or wrong) → `False`.
4. At the end the stack must be empty (no unclosed openers).

Trace `"([)]"`: push `(`, push `[`, `)` needs `(` but top is `[` → **False**.  ⏱ O(n) 💾 O(n)
