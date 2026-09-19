# 02 · Two Pointers & Sliding Window

**Two pointers:** two indexes `l`, `r` that move toward each other (or together) so you skip work instead of checking all pairs.
**Sliding window:** a contiguous range `[l..r]`. Grow with `r`, shrink with `l` whenever the window becomes invalid.

---
## Valid Palindrome
**Ask:** reads the same forwards/backwards, ignoring punctuation/case.
1. Build a clean string: keep only `c.isalnum()`, lowercased.
2. Compare it to its reverse `[::-1]`.

Trace `"Was it a car"` → `"wasitacar"`; reversed is `"racatisaw"` → not equal → **False**. (`"A man, a plan, a canal: Panama"` → `"amanaplanacanalpanama"` equals its reverse → **True**.) ⏱ O(n) 💾 O(n)
(Two-pointer version: `l` from left, `r` from right, skip non-letters, compare, move inward, giving O(1) space.)

---
## Container With Most Water
**Ask:** pick two bars to hold the most water. Area = width × the **shorter** bar.
1. Start with the widest container: `l=0`, `r=end`.
2. Compute area, update best.
3. Move the **shorter** bar's pointer inward. Why? The shorter bar caps the height; moving the taller one can only make width smaller with no height gain. Moving the shorter one is the only chance to improve.
4. Repeat until `l` meets `r`.

Trace `[1,7,2,5]`: (1,5): width 3 × 1 = 3; move `l` (shorter) → (7,5): width 2 × 5 = 10; move `r` → (7,2): 1 × 2 = 2. Best **10**.  ⏱ O(n) 💾 O(1)

---
## 3Sum
**Ask:** all unique triples adding to 0.
1. **Sort** first (enables two pointers and easy duplicate skipping).
2. Fix the first number `a` (loop `i`). Skip it if equal to the previous (avoids duplicate triples).
3. Now it's "Two Sum in a sorted array" on the rest: `l=i+1`, `r=end`.
   - sum > 0 → too big → `r -= 1`
   - sum < 0 → too small → `l += 1`
   - sum == 0 → save triple, `l += 1`, and keep moving `l` while it equals the previous value (skip duplicates).

Trace `[-1,0,1,2,-1,-4]` → sorted `[-4,-1,-1,0,1,2]`. Take a=-1 (index 1): l→-1, r→2 gives sum 0 → save `[-1,-1,2]`; move l→0: sum 1 is too big → r→1: sum 0 → save `[-1,0,1]`.
⏱ O(n²) 💾 O(1) extra

---
## Best Time to Buy & Sell Stock
**Ask:** max profit from one buy then one later sell.
1. `l` = buy day, `r` = sell day (starts at 1).
2. If `prices[r] > prices[l]`: profit = difference; keep the max.
3. Else (`r` is cheaper than `l`) → a better buy day found: `l = r`.
4. `r += 1` each step.

Trace `[10,1,5,6]`: r=1: 1<10 → l=1. r=2: profit 4. r=3: profit **5**.  ⏱ O(n) 💾 O(1)

---
## Longest Substring Without Repeating Characters (sliding window)
**Ask:** longest stretch with no repeated letter.
1. `charSet` = letters currently in the window; `l` = window start.
2. For each `r`: while `s[r]` is **already in the window**, remove `s[l]` and `l += 1` (shrink until the duplicate is gone).
3. Add `s[r]`. Window length = `r - l + 1`; keep the max.

Trace `"zxyzxyz"`: window grows z,zx,zxy (len 3); next `z` duplicates → drop leading z → `xyz`... max **3**.  ⏱ O(n) 💾 O(1)–O(n)

---
## Longest Repeating Character Replacement
**Ask:** longest substring you can make all-same-letter with at most `k` replacements.
Key formula: **replacements needed = window size − count of the most frequent letter** (keep the majority letter, change the rest).
1. Expand `r`, counting letters; track `maxf` = highest count seen in the window.
2. If `(r-l+1) - maxf > k` the window needs too many replacements → shrink: decrement `count[s[l]]`, `l += 1`.
3. Best answer = largest `r-l+1` seen.

Trace `"AABABBA"`, k=1: window `AABA` size 4, maxf(A)=3 → 4-3=1 ≤ 1 OK → **4**.
Note: `maxf` is never decreased; that's fine, because the window only matters when it beats the previous best.  ⏱ O(n) 💾 O(1)
