# 06 · Intervals, Dynamic Programming, Bits

---
# Intervals
**Core idea:** sort by start time, then only compare neighbours.

## Meeting Schedule (can one person attend all?)
1. Sort by start.
2. For each meeting, if the **previous one ends after this one starts** → overlap → `False`.
3. No overlaps → `True`.

Trace `(0,30),(5,10)`: 30 > 5 → **False**.  ⏱ O(n log n)

## Meeting Schedule II (minimum rooms)
Idea: rooms needed = the most meetings running **at the same moment**.
1. Sort the start times and the end times **separately**.
2. Two pointers `s` (starts), `e` (ends). If the next start is **before** the earliest end → a new meeting begins while others run → `count += 1`.
3. Otherwise one meeting ended → `count -= 1`.
4. Track the max `count`.

Trace starts `[0,5,15]`, ends `[10,20,30]`: 0<10 (count 1), 5<10 (2), 15≥10 (1), then 15<20 (2)... max **2**.  ⏱ O(n log n)

## Insert New Interval
Intervals are sorted. For each existing interval:
1. New ends **before** it starts → no overlap, and everything after is later too: add new, then add the rest, done.
2. New starts **after** it ends → it's entirely before: add it to the result.
3. Otherwise they **overlap** → merge: `new = [min(starts), max(ends)]`, keep going.
4. After the loop, append `new`.

Trace `[[1,3],[6,9]]` + `[2,5]`: overlaps `[1,3]` → new becomes `[1,5]`; `[6,9]` starts after it → add `[1,5]`, then `[6,9]`.  ⏱ O(n)

---
# Dynamic Programming (DP)
**Core idea:** the answer for `n` is built from answers for smaller inputs, and you remember them instead of recomputing.
Ask: "what's my last decision, and which smaller answers does it depend on?"

## Climbing Stairs
To reach step `n`, your last move was 1 step (from `n-1`) or 2 steps (from `n-2`) → `ways(n) = ways(n-1) + ways(n-2)` (Fibonacci).
Only the last two values matter, so keep two variables (`one`, `two`) and roll them forward.
Trace: ways for n = 1,2,3,4 → 1, 2, 3, **5**.  ⏱ O(n) 💾 O(1)

## House Robber
At each house choose: **rob it** (`n + best up to two houses back`) or **skip it** (best up to the previous house).
`new = max(n + rob1, rob2)`; then shift: `rob1 = rob2`, `rob2 = new`.
Trace `[2,7,9,3]`: best so far after each house = 2, 7, 11, 11 → **11** (2 + 9).  ⏱ O(n) 💾 O(1)

## House Robber II (houses in a circle)
First and last are neighbours, so you can't take both. Solve **two** normal problems and take the best:
1. Skip the first house: `helper(nums[1:])`
2. Skip the last house: `helper(nums[:-1])`
3. Include `nums[0]` in the `max` to cover the single-house case.

## Counting Bits
For every `i` in `0..n`, count its 1s.
Idea: `i` = (the biggest power of two ≤ i) + a smaller number. E.g. 6 = 4 + 2 → bits(6) = 1 + bits(2).
1. `offset` = latest power of two. When `offset*2 == i`, update it.
2. `dp[i] = 1 + dp[i - offset]`.

Trace: dp[0]=0, dp[1]=1, dp[2]=1+dp[0]=1, dp[3]=1+dp[1]=2, dp[4]=1+dp[0]=1.  ⏱ O(n)

---
# Bit Manipulation
Handy facts: `n & 1` = last bit. `n >> 1` = divide by 2 (drops the last bit). `n << k` = shift left. `^` XOR (bits that differ), `&` AND (both 1).

## Number of 1 Bits
1. While `n` isn't 0: add its last bit (`n % 2`), then `n >>= 1`.

Trace 5 (`101`): +1 → `10` +0 → `1` +1 → **2**.

## Reverse Bits
1. For each of the 32 positions `i`: read bit `i` → `(n >> i) & 1`.
2. Place it at the mirrored position `31 - i` → `bit << (31 - i)`, and OR it into the result.

## Missing Number (`0..n`, one missing)
Sum of `0..n` minus the sum of the actual values = the missing one. The code does it in one loop:
start `res = n`, then for each index add `i - nums[i]`; matching values cancel and the leftover is the missing number.

Trace `[0,2]` (n=2): res=2 → +(0-0) → +(1-2) = **1**.  ⏱ O(n) 💾 O(1)

## Sum of Two Integers (add without `+`)
Addition splits into two parts:
- `a ^ b` = the sum **ignoring carries**
- `(a & b) << 1` = the **carries**

Repeat until no carry remains (`b == 0`). `MASK = 0xffffffff` forces 32-bit behaviour (Python ints are unbounded), and the last line converts back to a negative number if bit 31 is set.
Trace 1+2: `a^b=3`, carry 0 → **3**. Trace 1+1: `a^b=0`, carry `2` → `a=0,b=2` → `a=2`, carry 0 → **2**.
