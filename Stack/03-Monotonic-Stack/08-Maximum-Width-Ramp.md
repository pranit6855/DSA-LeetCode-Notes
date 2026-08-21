# LeetCode 962 — Maximum Width Ramp

**Pattern:** Stack — Next / Previous Greater-Smaller

---

# Problem

Hume ek integer array `nums` diya hai.

Hume do indexes `i` aur `j` find karne hain jahan:

```text
i < j
```

aur:

```text
nums[i] <= nums[j]
```

Aise pair ko **ramp** kehte hain.

Ramp ki width:

```text
j - i
```

Hume **maximum possible width** return karni hai.

---

# Example

```text
nums = [6,0,8,2,1,5]
```

Indexes:

```text
index:  0  1  2  3  4  5
value:  6  0  8  2  1  5
```

Ek valid pair:

```text
i = 1
j = 5
```

because:

```text
nums[1] = 0
nums[5] = 5
```

and:

```text
0 <= 5
```

Width:

```text
5 - 1 = 4
```

So answer:

```text
4
```

---

# What Is A Ramp?

A pair `(i,j)` valid hai agar:

```text
i < j
```

and:

```text
nums[i] <= nums[j]
```

Example:

```text
nums = [6,0,8,2,1,5]
```

Pair:

```text
(1,5)
```

Valid:

```text
0 <= 5
```

Width:

```text
5 - 1 = 4
```

---

# Brute Force Approach

Sab possible pairs check kar sakte hain.

```cpp
for(int i = 0; i < n; i++) {

    for(int j = i + 1; j < n; j++) {

        if(nums[i] <= nums[j]) {
            ans = max(ans, j - i);
        }
    }
}
```

### Complexity

```text
Time: O(n²)
Space: O(1)
```

Large input ke liye inefficient hai.

---

# Main Observation

Hume maximum width chahiye:

```text
j - i
```

Isliye:

* `i` ko jitna left rakhenge, width utni badi ho sakti hai.
* `j` ko jitna right rakhenge, width utni badi ho sakti hai.

Lekin `nums[i] <= nums[j]` condition bhi satisfy honi chahiye.

---

# Important Observation About Left Index

Suppose:

```text
i1 = 0
nums[i1] = 6
```

and:

```text
i2 = 1
nums[i2] = 0
```

Compare:

```text
index 0 → value 6
index 1 → value 0
```

`0` better left candidate hai.

Kyun?

Agar koi future value `x`:

```text
6 <= x
```

satisfy karti hai, to automatically:

```text
0 <= x
```

bhi satisfy karegi.

So smaller left value is more useful.

---

# Prefix Minimum

Array:

```text
[6,0,8,2,1,5]
```

Left se scan karo aur sirf **new minimum** values save karo.

### Index 0

```text
value = 6
```

First element hai, so save.

### Index 1

```text
value = 0
```

`0 < 6`

So new minimum.

Save.

### Index 2

```text
value = 8
```

`8 < 0` false.

Skip.

### Index 3

```text
2 < 0` false
```

Skip.

### Index 4

```text
1 < 0` false
```

Skip.

### Index 5

```text
5 < 0` false
```

Skip.

Final useful indexes:

```text
[0,1]
```

Corresponding values:

```text
[6,0]
```

These are the prefix-minimum positions.

---

# Why These Are Useful?

Stack:

```text
[0,1]
```

means:

```text
index 0 → value 6
index 1 → value 0
```

`1` is a later index but has smaller value.

So for future `j`, `1` is a better candidate than `0` whenever it satisfies the condition.

We don't need to store useless left indexes.

---

# Phase 1 — Build Stack

```cpp
stack<int> st;

for(int i = 0; i < n; i++) {

    if(st.empty() || nums[i] < nums[st.top()]) {
        st.push(i);
    }
}
```

Stack stores:

```text
prefix minimum indexes
```

---

# Detailed Phase 1 Dry Run

Input:

```text
nums = [6,0,8,2,1,5]
```

Initially:

```text
st = []
```

---

## i = 0

```text
nums[0] = 6
```

Stack empty.

Push:

```text
st = [0]
```

---

## i = 1

```text
nums[1] = 0
```

Top:

```text
nums[0] = 6
```

Check:

```text
0 < 6
```

True.

Push:

```text
st = [0,1]
```

---

## i = 2

```text
nums[2] = 8
```

Top value:

```text
nums[1] = 0
```

Check:

```text
8 < 0
```

False.

Do not push.

---

## i = 3

```text
2 < 0
```

False.

Do not push.

---

## i = 4

```text
1 < 0
```

False.

Do not push.

---

## i = 5

```text
5 < 0
```

False.

Do not push.

---

# Phase 1 Complete

```text
st = [0,1]
```

Values:

```text
[6,0]
```

---

# Why Do We Scan Right To Left?

Ab hume `j` choose karna hai.

Width:

```text
j - i
```

maximize karni hai.

Agar kisi fixed `i` ke liye maximum width chahiye, to hume **largest possible `j`** chahiye.

Isliye rightmost index se start karna smart hai:

```text
j = n-1
n-2
n-3
...
```

---

# Phase 2 — Right To Left

```cpp
for(int j = n - 1; j >= 0; j--) {
```

Current `j` ko right se left move karenge.

Jab:

```text
nums[st.top()] <= nums[j]
```

mil jaata hai, valid ramp mil gaya.

Width:

```text
j - st.top()
```

calculate karenge.

---

# Why Can We Pop After Finding A Valid Pair?

Suppose:

```text
i = 1
j = 5
```

valid hai.

Width:

```text
5 - 1 = 4
```

`j = 5` array ka **maximum possible right index** hai.

Isliye `i = 1` ke liye isse better width future mein nahi aa sakti.

So index `1` ko permanently remove kar sakte hain:

```text
st.pop();
```

---

# Phase 2 Detailed Dry Run

Phase 1 ke baad:

```text
st = [0,1]
ans = 0
```

---

# j = 5

Current:

```text
nums[5] = 5
```

Stack top:

```text
1
```

Top value:

```text
nums[1] = 0
```

Check:

```text
0 <= 5
```

True.

So:

```text
i = 1
j = 5
```

Valid ramp.

Width:

```text
5 - 1 = 4
```

Update:

```cpp
ans = max(ans, 4);
```

So:

```text
ans = 4
```

Pop:

```text
st = [0]
```

---

# Check While Again

Stack top:

```text
0
```

Value:

```text
nums[0] = 6
```

Current:

```text
nums[5] = 5
```

Check:

```text
6 <= 5
```

False.

Stop.

---

# j = 4

Current:

```text
nums[4] = 1
```

Stack top:

```text
0
```

Value:

```text
nums[0] = 6
```

Check:

```text
6 <= 1
```

False.

No change.

```text
ans = 4
st = [0]
```

---

# j = 3

Current:

```text
nums[3] = 2
```

Top:

```text
nums[0] = 6
```

Check:

```text
6 <= 2
```

False.

No change.

---

# j = 2

Current:

```text
nums[2] = 8
```

Top:

```text
nums[0] = 6
```

Check:

```text
6 <= 8
```

True.

Valid ramp:

```text
i = 0
j = 2
```

Width:

```text
2 - 0 = 2
```

Update:

```text
ans = max(4,2)
```

So:

```text
ans = 4
```

Pop index `0`.

```text
st = []
```

---

# j = 1

Stack empty.

Nothing to do.

---

# j = 0

Stack empty.

Nothing to do.

---

# Final Answer

```text
ans = 4
```

---

# Complete C++ Code

```cpp
class Solution {
public:
    int maxWidthRamp(vector<int>& nums) {

        int n = nums.size();

        stack<int> st;

        // Phase 1:
        // Store indexes of prefix minimums
        for (int i = 0; i < n; i++) {

            if (st.empty() || nums[i] < nums[st.top()]) {
                st.push(i);
            }
        }

        int ans = 0;

        // Phase 2:
        // Scan from right to left
        for (int j = n - 1; j >= 0; j--) {

            while (!st.empty() &&
                   nums[st.top()] <= nums[j]) {

                ans = max(ans, j - st.top());

                st.pop();
            }
        }

        return ans;
    }
};
```

---

# Code Explanation

## `stack<int> st`

```cpp
stack<int> st;
```

Stack mein **indexes** store karenge.

Example:

```text
[0,1]
```

means:

```text
index 0 → value 6
index 1 → value 0
```

---

# Phase 1 Condition

```cpp
if (st.empty() || nums[i] < nums[st.top()])
```

Meaning:

> Kya current value ab tak ki smallest value hai?

Agar yes:

```cpp
st.push(i);
```

---

# Why Strict `<`?

```cpp
nums[i] < nums[st.top()]
```

Use kiya hai.

Example:

```text
[5,5,5]
```

Equal value koi new better minimum nahi banati.

First `5` already best candidate hai.

So unnecessary duplicate minimum indexes store nahi karte.

---

# Phase 2 Condition

```cpp
while (!st.empty() &&
       nums[st.top()] <= nums[j])
```

Meaning:

> Kya current right value Stack ke candidate left value se greater/equal hai?

Agar yes:

```text
valid ramp
```

---

# Width

```cpp
ans = max(ans, j - st.top());
```

Stack top = `i`.

Current `j`.

So width:

```text
j - i
```

---

# Why `while`?

Ek `j` multiple left candidates ko satisfy kar sakta hai.

Example:

```text
nums = [1,0,0,5]
```

For `j=3`:

```text
0 <= 5
0 <= 5
1 <= 5
```

Multiple indexes valid ho sakte hain.

So:

```cpp
while(...)
```

use karte hain.

---

# Why Pop?

Jab:

```text
nums[i] <= nums[j]
```

mil gaya and `j` rightmost se process ho raha hai, then current `j` gives maximum possible width for this `i`.

So:

```cpp
st.pop();
```

Ab is `i` ko future j ke liye check karne ki need nahi.

---

# Important Insight

Ye problem ka real learning point:

## Smaller Left Value Is Better

Suppose:

```text
index 0 → 6
index 1 → 0
```

`0` better left candidate hai because:

```text
0 <= x
```

is easier to satisfy than:

```text
6 <= x
```

Therefore we only keep prefix minimum indexes.

---

# Why This Is Monotonic Stack

Stack values:

```text
6
0
```

decreasing order mein hain.

Har new smaller value:

```text
new minimum
```

Stack mein add hoti hai.

So ye monotonic structure maintain karta hai.

---

# Brute Force vs Stack

## Brute Force

```cpp
for(int i = 0; i < n; i++) {
    for(int j = i + 1; j < n; j++) {
        if(nums[i] <= nums[j]) {
            ans = max(ans, j - i);
        }
    }
}
```

Complexity:

```text
O(n²)
```

---

## Monotonic Stack

### Phase 1

```text
O(n)
```

### Phase 2

```text
O(n)
```

Every index:

```text
push → maximum once
pop → maximum once
```

Overall:

```text
O(n)
```

---

# Complexity

### Time Complexity

```text
O(n)
```

### Space Complexity

```text
O(n)
```

Stack mein worst case `n` indexes ho sakte hain.

---

# Common Mistakes

## Mistake 1 — All indexes Stack mein push karna

Wrong:

```cpp
st.push(i);
```

har element ke liye.

Aisa nahi karna.

Sirf new prefix minimum push karo:

```cpp
if(st.empty() || nums[i] < nums[st.top()])
    st.push(i);
```

---

## Mistake 2 — Left to Right second phase karna

Second phase right-to-left hona chahiye:

```cpp
for(int j = n - 1; j >= 0; j--)
```

because maximum `j` chahiye.

---

## Mistake 3 — Width Formula Galat

Correct:

```cpp
j - st.top()
```

Not:

```text
i + j
```

---

## Mistake 4 — `>` Instead of `<=`

Ramp condition:

```text
nums[i] <= nums[j]
```

So:

```cpp
nums[st.top()] <= nums[j]
```

use karna hai.

---

# Interview Explanation

> I solve the problem in two phases. First, I build a monotonic decreasing stack containing only indexes of prefix minimum values. These are the only useful candidates for the left boundary because a smaller value at an earlier index is always easier to satisfy. Then I scan the array from right to left. Whenever the value at the stack top is less than or equal to the current right value, we have a valid ramp, so I update the maximum width and pop that index. Because we process `j` from right to left, the first valid `j` for a left index is the largest possible `j`, so that index can be removed permanently. The overall complexity is O(n) time and O(n) space.

---

# One-Line Revision

> **Left se prefix minimum indexes Stack mein rakho, right se scan karo, `nums[i] <= nums[j]` milte hi `j-i` calculate karo aur `i` pop kar do.**

---

# Pattern #4 Progress

* [x] LC 1019 — Next Greater Node In Linked List
* [x] LC 1944 — Number of Visible People in a Queue
* [x] LC 962 — Maximum Width Ramp

## Pattern #4 — COMPLETE

Next tumhare fixed roadmap ke according:

**Pattern #2 — Stack Simulation**

---

# Folder Structure

```text
Stack/
│
├── 01-Matching-Pairing/
│   ├── 01-Valid-Parentheses.md
│   ├── 02-Minimum-Add-to-Make-Parentheses-Valid.md
│   └── 03-Minimum-Remove-to-Make-Valid-Parentheses.md
│
├── 02-Stack-Simulation/
│
├── 03-Monotonic-Stack/
│   ├── 01-Daily-Temperatures.md
│   ├── 02-Next-Greater-Element-I.md
│   ├── 03-Next-Greater-Element-II.md
│   ├── 04-Online-Stock-Span.md
│   └── 05-Final-Prices-With-a-Special-Discount-in-a-Shop.md
│
├── 04-Next-Previous-Greater-Smaller/
│   ├── 01-Next-Greater-Node-In-Linked-List.md
│   ├── 02-Number-of-Visible-People-in-a-Queue.md
│   └── 03-Maximum-Width-Ramp.md
│
├── 05-Boundary-Contribution/
│
└── 06-Stack-Design/
```
