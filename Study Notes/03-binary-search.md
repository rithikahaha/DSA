# 03 · Binary Search

**Core idea:** if data is sorted, look at the middle and throw away half. Each step halves the search: O(log n).
Template: `l, r = 0, len-1`; `while l <= r:` compute `m`; move `l = m+1` or `r = m-1`.
(Use `m = l + (r-l)//2` to avoid overflow in other languages; in Python `(l+r)//2` is also fine.)

---
## Binary Search
1. `m` = middle.
2. `nums[m] == target` → return `m`.
3. `nums[m] > target` → target is in the left half → `r = m-1`.
4. `nums[m] < target` → right half → `l = m+1`.
5. Loop ends without a hit → `-1`.

Trace `[-1,0,2,4], target 2`: m=1 (0<2) → l=2; m=2 → found **2**.  ⏱ O(log n) 💾 O(1)

---
## Find Minimum in Rotated Sorted Array
**Rotated** = e.g. `[3,4,5,1,2]`: two sorted runs; the minimum is where the drop happens.
1. If `nums[l] < nums[r]`, this slice is already sorted → the min is `nums[l]`; stop.
2. Otherwise take `m`, and remember `min(res, nums[m])`.
3. If `nums[m] >= nums[l]`, `m` is in the **left (big) run** → the min is to the right: `l = m+1`.
4. Else `m` is in the right run → min is at `m` or left: `r = m-1`.

Trace `[3,4,5,1,2]`: m=2 (5≥3) → l=3; slice `[1,2]` sorted → min **1**.  ⏱ O(log n)

---
## Search in Rotated Sorted Array
Same rotation idea, but looking for a target. Whichever half contains `m` is **sorted**, so you can tell if the target is in it.
1. `nums[m] == target` → return.
2. **Left half sorted** (`nums[l] <= nums[m]`):
   - target > `nums[m]` or target < `nums[l]` → it's not in the left → `l = m+1`
   - else → `r = m-1`.
3. **Right half sorted** (else):
   - target < `nums[m]` or target > `nums[r]` → not in the right → `r = m-1`
   - else → `l = m+1`.

Trace `[3,4,5,6,1,2]`, target 1: m=2 (left sorted 3..5; 1<3 → go right) → l=3; m=4 → found **4**.  ⏱ O(log n)
