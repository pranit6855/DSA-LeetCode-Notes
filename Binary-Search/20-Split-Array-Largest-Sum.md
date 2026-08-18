# LeetCode 410 — Split Array Largest Sum

## Problem

Hume ek integer array `nums` diya gaya hai aur ek integer `k`.

Hume `nums` ko exactly `k` **non-empty continuous subarrays** mein split karna hai.

Goal:

> Har subarray ka sum nikalo, unmein se jo **maximum sum** hai usko minimum karna hai.

Matlab:

```text id="0d0f6a"
1. Array ko k parts mein split karo
2. Har part ka sum nikalo
3. Sabse bada sum find karo
4. Aise split ko choose karo jisme ye largest sum minimum ho
```

---

# Example

```text id="5e5a4e"
nums = [7,2,5,10,8]
k = 2
```

Hume array ko 2 continuous parts mein split karna hai.

Possible splits:

### Split 1

```text id="5p4b4x"
[7] | [2,5,10,8]
```

Sums:

```text id="6q6lpu"
7
25
```

Largest sum:

```text id="v9fjci"
25
```

---

### Split 2

```text id="3m3q6u"
[7,2] | [5,10,8]
```

Sums:

```text id="ug0l4f"
9
23
```

Largest sum:

```text id="tq4kav"
23
```

---

### Split 3

```text id="trc9id"
[7,2,5] | [10,8]
```

Sums:

```text id="cm6ayf"
14
18
```

Largest sum:

```text id="x5v4j3"
18
```

---

### Split 4

```text id="c7l6j1"
[7,2,5,10] | [8]
```

Sums:

```text id="bq0xw7"
24
8
```

Largest sum:

```text id="e6q3px"
24
```

---

Ab sabhi possible largest sums:

```text id="e8t5j2"
25
23
18
24
```

Hume inmein se minimum chahiye:

```text id="1o9t3k"
min(25,23,18,24) = 18
```

Therefore:

```text id="1fjqv4"
Answer = 18
```

---

# Important Observation

Brute force mein hum har possible partition try kar sakte hain.

Lekin array bada hone par partitions ki number bahut zyada ho jayegi.

Isliye hum ek different approach use karenge:

```text id="6e3m8u"
Binary Search on Answer
+
Greedy
```

---

# What Are We Binary Searching?

Hum array ke elements par binary search nahi kar rahe.

Hum search kar rahe hain:

```text id="8t6k1s"
Maximum allowed sum of a subarray
```

Maan lo hum `mid = 18` lete hain.

Ab question:

> Kya hum array ko maximum sum `18` ke saath `k` continuous subarrays mein split kar sakte hain?

Agar yes:

```text id="2a6q3c"
18 possible hai
```

Agar no:

```text id="s0t9wq"
18 possible nahi hai
```

---

# Step 1 — Lowest Possible Answer

Sabse chhota possible answer array ke maximum element se kam nahi ho sakta.

Example:

```text id="h7k3s9"
nums = [7,2,5,10,8]
```

Maximum element:

```text id="v2m5q1"
10
```

Agar answer `9` hota, to `10` ko kisi subarray mein rakhna hi padega.

Us subarray ka sum at least `10` hoga.

So answer `10` se chhota impossible hai.

Therefore:

```cpp id="8w1m4a"
low = *max_element(nums.begin(), nums.end());
```

---

# Step 2 — Highest Possible Answer

Maximum possible answer tab aayega jab poora array ek hi subarray ho.

Example:

```text id="9k4x7m"
7 + 2 + 5 + 10 + 8 = 32
```

So:

```cpp id="5n8p2c"
high = total sum;
```

Therefore:

```text id="r6c3y1"
low = maximum element
high = total sum
```

Example:

```text id="m3v9x5"
low = 10
high = 32
```

---

# Step 3 — `mid` Ka Meaning

Maan lo:

```text id="x8q2f4"
low = 10
high = 32
```

Then:

```text id="j6p1r9"
mid = 21
```

Ab hum check karenge:

> Kya array ko aise split kar sakte hain ki **har subarray ka sum <= 21** ho aur total `k` se zyada subarrays na lagen?

---

# Step 4 — Greedy Feasibility Check

Iske liye helper function:

```cpp id="g5s7n2"
bool cansplit(vector<int>& nums, int k, long long maxsum)
```

Ye check karega ki given `maxsum` ke saath kitne subarrays required honge.

---

# Greedy Rule

Array ko left se right traverse karo.

Jab tak:

```text id="u8k3p0"
currentSum + nums[i] <= maxsum
```

hai, current subarray mein element add karte jao.

Jaise hi:

```text id="d1m6x8"
currentSum + nums[i] > maxsum
```

ho:

> Current subarray ko yahin close karo aur ek naya subarray start karo.

---

# Example — `maxsum = 18`

```text id="r9p5k2"
nums = [7,2,5,10,8]
```

Start:

```text id="q2v7m4"
subarrays = 1
currentSum = 0
```

### Add 7

```text id="c7n1x5"
currentSum = 7
```

### Add 2

```text id="k4m8q2"
currentSum = 9
```

### Add 5

```text id="t3x6p9"
currentSum = 14
```

### Try adding 10

```text id="b8r2v6"
14 + 10 = 24
```

But:

```text id="m1q9x3"
24 > 18
```

So `10` current subarray mein nahi aa sakta.

Split:

```text id="j5k8c1"
[7,2,5] | [10]
```

Now:

```text id="e3v7n4"
subarrays = 2
currentSum = 10
```

### Add 8

```text id="z6p2m8"
10 + 8 = 18
```

Allowed.

Final:

```text id="f4x9q7"
[7,2,5] | [10,8]
```

Number of subarrays:

```text id="w2k6n1"
2
```

Since:

```text id="p7m4c8"
2 <= k
```

`18` is possible.

---

# Why Greedy Works?

Hum har subarray ko jitna possible ho utna fill kar rahe hain.

Example:

```text id="q8m1v5"
maxsum = 18
```

`[7,2,5]` ka sum:

```text id="3r6x9p"
14
```

Ab `10` add karenge to:

```text id="d4k7m2"
24
```

ho jayega.

Isliye usko next subarray mein bhejna compulsory hai.

Greedy mein hum **earliest possible split** karte hain.

---

# `cansplit()` Function

```cpp id="9v2m5x"
bool cansplit(vector<int>& nums, int k, long long maxsum) {

    int subarrays = 1;
    long long cursum = 0;

    for (int i = 0; i < nums.size(); i++) {

        if (cursum + nums[i] > maxsum) {
            subarrays++;
            cursum = nums[i];
        }
        else {
            cursum += nums[i];
        }

        if (subarrays > k) {
            return false;
        }
    }

    return true;
}
```

---

# `cansplit()` Line-by-Line

## `subarrays = 1`

```cpp id="z3k7p2"
int subarrays = 1;
```

Initially first subarray exist karta hai.

---

## `cursum = 0`

```cpp id="x5m1q8"
long long cursum = 0;
```

Current subarray ka sum store karega.

---

## Current Element Add Karna

```cpp id="m7v2c4"
if (cursum + nums[i] > maxsum)
```

Agar current element add karne se maximum allowed sum cross ho jayega, to new subarray banana padega.

Example:

```text id="a9r3k6"
cursum = 14
nums[i] = 10
maxsum = 18
```

Check:

```text id="n6x2p7"
14 + 10 > 18
```

True.

So new subarray.

---

## New Subarray

```cpp id="q4m8x1"
subarrays++;
cursum = nums[i];
```

Example:

```text id="d7v3k9"
subarrays = 2
cursum = 10
```

---

## Same Subarray Mein Add

Agar limit cross nahi hoti:

```cpp id="h2p6n5"
cursum += nums[i];
```

---

## Too Many Subarrays

```cpp id="r8x1m4"
if (subarrays > k) {
    return false;
}
```

Agar required `k` se zyada subarrays lag gaye, to `maxsum` bahut chhota hai.

Therefore:

```text id="u5q9c2"
false
```

---

# Why `subarrays <= k` Means Possible?

Suppose:

```text id="n1x5m7"
k = 3
```

Aur given `maxsum` ke saath hume:

```text id="p8v2r4"
2 subarrays
```

lage.

Ye bhi valid hai.

Kyunki hum further kisi subarray ko split karke 3 parts bana sakte hain, without increasing any subarray sum.

So feasibility condition:

```text id="z6m3q8"
subarrays <= k
```

---

# Binary Search

Initial:

```text id="c5p9v1"
low = 10
high = 32
```

Maan lo:

```text id="s7x2m4"
mid = 21
```

Check:

```text id="k3q8n6"
cansplit(nums, k, 21)
```

Agar true:

```cpp id="w1r5x9"
high = mid;
```

Kyunki answer minimum chahiye, to smaller answer try karenge.

---

# If `mid` Is Impossible

Maan lo:

```text id="e8m4v2"
mid = 12
```

Agar `12` ke maximum sum mein required `k` parts mein split nahi kar pa rahe, to:

```text id="y6p1q7"
12 too small
```

So larger answer chahiye:

```cpp id="r3x9m5"
low = mid + 1;
```

---

# Monotonic Property

Binary Search tabhi work karegi jab feasibility monotonic ho.

Example:

```text id="w4k8n2"
Maximum Allowed Sum:

10 11 12 13 14 15 16 17 18 19 20
 N  N  N  N  N  N  N  N  Y  Y  Y
                          ↑
                     First Valid
```

Agar `18` possible hai:

```text id="d7m2x9"
19
20
21
...
```

bhi possible honge.

Aur agar `17` impossible hai, to:

```text id="a4q6p8"
16
15
14
...
```

bhi impossible honge.

Therefore binary search valid hai.

---

# Full Binary Search Logic

```cpp id="v8m3q1"
while (low < high) {

    long long mid = low + (high - low) / 2;

    if (cansplit(nums, k, mid)) {
        high = mid;
    }
    else {
        low = mid + 1;
    }
}
```

End mein:

```text id="r5x2n7"
low == high
```

Aur wahi minimum valid answer hai.

---

# Why `while (low < high)`?

Hum **first valid answer** find kar rahe hain.

Jab:

```text id="f2m7q4"
low == high
```

sirf ek candidate bachta hai.

Therefore loop stop kar sakte hain.

---

# Complete Code

```cpp id="k7x3m9"
class Solution {
public:

    bool cansplit(vector<int>& nums, int k, long long maxsum) {

        int subarrays = 1;
        long long cursum = 0;

        for (int i = 0; i < nums.size(); i++) {

            if (cursum + nums[i] > maxsum) {
                subarrays++;
                cursum = nums[i];
            }
            else {
                cursum += nums[i];
            }

            if (subarrays > k) {
                return false;
            }
        }

        return true;
    }

    int splitArray(vector<int>& nums, int k) {

        long long low = *max_element(
            nums.begin(),
            nums.end()
        );

        long long high = 0;

        for (int x : nums) {
            high += x;
        }

        while (low < high) {

            long long mid = low + (high - low) / 2;

            if (cansplit(nums, k, mid)) {
                high = mid;
            }
            else {
                low = mid + 1;
            }
        }

        return (int)low;
    }
};
```

---

# Detailed Dry Run

Input:

```text id="n8v2m5"
nums = [7,2,5,10,8]
k = 2
```

Initial:

```text id="p4x7q1"
low = 10
high = 32
```

---

## Iteration 1

```text id="m6k2v9"
mid = 10 + (32 - 10) / 2
    = 21
```

Check `21`:

```text id="c9x4p2"
[7,2,5] = 14
[10,8] = 18
```

2 subarrays.

So `21` possible.

Therefore:

```text id="r7m3q8"
high = 21
```

---

## Iteration 2

```text id="v1x5k7"
low = 10
high = 21

mid = 15
```

Try max sum `15`:

```text id="a2m8q4"
[7,2,5] = 14
[10] = 10
[8] = 8
```

Need 3 subarrays.

But:

```text id="s6p3n9"
k = 2
```

So impossible.

Therefore:

```text id="x4q7m1"
low = 16
```

---

## Iteration 3

```text id="c8m2v6"
low = 16
high = 21

mid = 18
```

Check:

```text id="k5p9x3"
[7,2,5] = 14
[10,8] = 18
```

2 subarrays.

Possible.

Therefore:

```text id="q1m6v8"
high = 18
```

---

## Iteration 4

```text id="r3x8k2"
low = 16
high = 18

mid = 17
```

Try:

```text id="v7m2p4"
[7,2,5] = 14
[10] = 10
[8] = 8
```

3 subarrays.

Not possible.

So:

```text id="n5q1x7"
low = 18
```

Now:

```text id="h4m8c2"
low = 18
high = 18
```

Loop ends.

Answer:

```text id="z7p3m6"
18
```

---

# Important Difference From LeetCode 1552

## 1552 — Magnetic Force

Goal:

```text id="e1m7q4"
Maximum minimum distance
```

Possible hone par:

```cpp id="m4x8n2"
low = mid + 1;
```

Matlab right side.

---

## 410 — Split Array Largest Sum

Goal:

```text id="q6p2v9"
Minimum maximum sum
```

Possible hone par:

```cpp id="x8m3k1"
high = mid;
```

Matlab left side.

---

# 410 Ka Binary Search Pattern

Ye pattern yaad rakho:

```text id="t5q9m2"
Minimum valid answer

N N N N Y Y Y Y
        ↑
    First Valid
```

Therefore:

```cpp id="p4x7v1"
if (possible)
    high = mid;
else
    low = mid + 1;
```

---

# Brute Force vs Optimized

## Brute Force

Har possible partition try karna:

```text id="c2m8x5"
Possible partitions
        ↓
Har partition ka largest sum
        ↓
Minimum largest sum
```

Problem:

```text id="n7q3v9"
Bahut saare partitions
```

Time bahut zyada ho jayega.

---

## Optimized

```text id="r1m6k4"
Binary Search on Answer
        +
Greedy Feasibility Check
```

Isliye efficiently answer mil jata hai.

---

# Common Mistakes

## 1. `maxsum` ko `cursum` se compare karna

Galat:

```cpp
if (cursum > maxsum)
```

Sahi:

```cpp
if (cursum + nums[i] > maxsum)
```

Kyunki hume check karna hai ki **new element add karne par** limit cross hogi ya nahi.

---

## 2. `low = 0` karna

Wrong:

```cpp
low = 0;
```

Sahi:

```cpp
low = max(nums)
```

Kyunki answer maximum element se chhota nahi ho sakta.

---

## 3. `high` ko maximum element rakhna

Wrong:

```cpp
high = max(nums);
```

High hona chahiye:

```cpp
high = total sum;
```

Kyunki worst case mein poora array ek subarray ho sakta hai.

---

## 4. Possible Hone Par Right Jana

410 mein minimum answer chahiye.

Therefore:

```cpp
if (possible) {
    high = mid;
}
```

---

## 5. `subarrays == k` Sochna

Check mein:

```cpp
subarrays <= k
```

valid hai.

Agar fewer parts ban rahe hain, to further split kar sakte hain.

---

# Interview Explanation

Agar interviewer puche:

> Explain your approach.

Tum bol sakte ho:

> Main is problem ko binary search on answer se solve karta hoon. Search space maximum element se total array sum tak hota hai, kyunki answer maximum element se kam nahi ho sakta aur poore array ka sum maximum possible value hai. Har `mid` ko main maximum allowed subarray sum maanta hoon aur greedy se array ko left to right split karta hoon. Jab current sum mein next element add karne se `mid` exceed hota hai, tab main new subarray start karta hoon. Agar required `k` se zyada subarrays lag jaate hain, to `mid` feasible nahi hai. Otherwise feasible hai. Feasible hone par main smaller maximum sum search karta hoon. Is tarah minimum possible largest subarray sum mil jata hai.

---

# Pattern Recognition

Agar question mein aisa kuch ho:

> Split array into `k` parts and minimize the maximum sum.

To directly socho:

```text id="v9m4q2"
Binary Search on Answer
+
Greedy
```

Answer space:

```text id="y2x7n5"
max element → total sum
```

Feasibility:

```text id="m8p3c6"
Can we split into at most k parts
with each sum <= mid?
```

---

# Key Takeaways

### Main Pattern

```text id="q4n8x1"
Binary Search on Answer
```

### Answer

```text id="m7v2p5"
Minimum possible maximum subarray sum
```

### Low

```text id="r3x9k6"
Maximum element
```

### High

```text id="n5q1m8"
Total sum
```

### Feasibility

```text id="c8v4p2"
Greedy
```

### Possible

```cpp id="a6m9x3"
high = mid;
```

### Impossible

```cpp id="k2r7v5"
low = mid + 1;
```

### Complexity

```text id="p9m3x7"
Time  : O(n log(sum(nums)))
Space : O(1)
```

---

# One-Line Revision

> **"410 mein maximum element se total sum tak binary search karo, aur har `mid` ko maximum allowed subarray sum maan kar greedy se check karo ki `k` ke andar array split ho raha hai ya nahi."**

---

## LeetCode

**Problem:** 410 — Split Array Largest Sum

**Difficulty:** Hard

**Pattern:** Binary Search on Answer + Greedy

**Question Number in Our Binary Search Series:** #20

**Filename:** `LeetCode_410_Split_Array_Largest_Sum.md`
