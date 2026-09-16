# LeetCode 974 — Subarray Sums Divisible by K

## Problem

Given an integer array `nums` and an integer `k`, return the number of non-empty subarrays whose sum is divisible by `k`.

A subarray is valid if:

```text
subarraySum % k == 0
```

### Example

```text
Input:
nums = [4,5,0,-2,-3,1]
k = 5

Output:
7
```

---

# Approach

We use:

```text
Prefix Sum + HashMap
```

But instead of storing the exact prefix sum, we store its **remainder when divided by k**.

## Main Idea

Suppose two prefix sums have the same remainder:

```text
prefixSum1 % k == prefixSum2 % k
```

Then:

```text
(prefixSum2 - prefixSum1) % k == 0
```

The difference between two prefix sums represents the sum of the subarray between them.

Therefore, that subarray is divisible by `k`.

### Example

```text
prefixSum1 = 4
prefixSum2 = 9
k = 5
```

Remainders:

```text
4 % 5 = 4
9 % 5 = 4
```

Same remainder.

Difference:

```text
9 - 4 = 5
```

And:

```text
5 % 5 = 0
```

So the subarray between those two prefix sums is valid.

---

# Why HashMap?

We store the frequency of each remainder.

```text
mp[remainder] = frequency
```

If the current remainder has already appeared `x` times, then there are `x` previous prefix sums that can form a valid subarray with the current prefix sum.

Therefore:

```cpp
ans += mp[rem];
```

Then store the current remainder:

```cpp
mp[rem]++;
```

---

# Why mp[0] = 1?

Initially:

```cpp
mp[0] = 1;
```

This represents the prefix sum `0` before the array starts.

It is required to count subarrays starting from index `0`.

### Example

```text
nums = [5]
k = 5
```

Prefix sum:

```text
5
```

Remainder:

```text
5 % 5 = 0
```

Because:

```cpp
mp[0] = 1;
```

we count this subarray:

```text
[5]
```

Without `mp[0] = 1`, this subarray would not be counted.

---

# Negative Remainder

In C++, negative numbers can produce negative remainders.

Example:

```cpp
-2 % 5
```

gives:

```text
-2
```

But we want the remainder in the range:

```text
0 to k-1
```

So:

```cpp
if (rem < 0) {
    rem += k;
}
```

Example:

```text
rem = -2
k = 5

rem = -2 + 5
    = 3
```

So we store `3`.

---

# Code

```cpp
class Solution {
public:
    int subarraysDivByK(vector<int>& nums, int k) {
        unordered_map<int, int> mp;

        int prefixSum = 0;
        int ans = 0;

        // Prefix sum 0 exists before the array starts
        mp[0] = 1;

        for (int x : nums) {
            // Calculate current prefix sum
            prefixSum += x;

            // Calculate remainder
            int rem = prefixSum % k;

            // Convert negative remainder to positive
            if (rem < 0) {
                rem += k;
            }

            // Same remainder means a divisible subarray exists
            if (mp.find(rem) != mp.end()) {
                ans += mp[rem];
            }

            // Store frequency of current remainder
            mp[rem]++;
        }

        return ans;
    }
};
```

---

# Dry Run

```text
nums = [4,5,0,-2,-3,1]
k = 5
```

Initial:

```text
prefixSum = 0
ans = 0

mp = {0 : 1}
```

| Element | Prefix Sum | Remainder | Previous Frequency | Answer |
| ------- | ---------: | --------: | -----------------: | -----: |
| 4       |          4 |         4 |                  0 |      0 |
| 5       |          9 |         4 |                  1 |      1 |
| 0       |          9 |         4 |                  2 |      3 |
| -2      |          7 |         2 |                  0 |      3 |
| -3      |          4 |         4 |                  3 |      6 |
| 1       |          5 |         0 |                  1 |      7 |

Final answer:

```text
7
```

---

# Valid Subarrays

For:

```text
nums = [4,5,0,-2,-3,1]
k = 5
```

The 7 valid subarrays are:

```text
[5]
[5,0]
[5,0,-2,-3]
[0]
[0,-2,-3]
[-2,-3]
[4,5,0,-2,-3,1]
```

All of their sums are divisible by `5`.

---

# Important Logic

Remember this:

```text
Same remainder
       ↓
Difference of prefix sums is divisible by k
       ↓
Valid subarray
```

The main line is:

```cpp
ans += mp[rem];
```

Not:

```cpp
ans++;
```

Because the same remainder can occur multiple times.

---

# Example of Multiple Frequency

Suppose:

```text
mp[4] = 3
```

and current remainder is also:

```text
4
```

Then the current prefix can pair with all 3 previous prefix sums.

Therefore:

```cpp
ans += mp[4];
```

means:

```text
ans += 3
```

---

# Complexity

### Time Complexity

```text
O(n)
```

We traverse the array once.

### Space Complexity

```text
O(k)
```

There can be at most `k` different normalized remainders:

```text
0, 1, 2, ..., k-1
```

---

# Interview Explanation

> I use prefix sum and a hashmap to store the frequency of each remainder. If the current prefix sum and a previous prefix sum have the same remainder when divided by K, their difference is divisible by K, which means the subarray between them is valid. So I add the frequency of the current remainder to the answer and then update its frequency.

---

# Pattern

```text
Prefix Sum
    +
Remainder
    +
HashMap Frequency
```

### Core Formula

```text
prefixSum[i] % k == prefixSum[j] % k
                    ↓
(prefixSum[i] - prefixSum[j]) % k == 0
                    ↓
             Valid Subarray
```

### One-Line Memory Trick

```text
974 = S
```
