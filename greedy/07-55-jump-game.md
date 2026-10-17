# Greedy Pattern — Question 7
# LC 55 — Jump Game

## 1. Problem Details

- **LeetCode Number:** 55
- **Pattern:** Greedy
- **Difficulty:** Medium
- **Topics:** Array, Greedy, Reachability

## 2. Problem Statement

We are given an integer array `nums`.

Each element `nums[i]` represents the maximum number of
steps we can jump forward from index `i`.

We start at index `0` and need to reach the last index.

Return `true` if reaching the last index is possible.
Otherwise, return `false`.

Important:
- We can jump fewer steps than `nums[i]`.
- We cannot jump backward.
- If `nums[i] = 3`, we can jump 1, 2, or 3 steps forward.
- We only need to check whether the last index is reachable.

## 3. Example 1

Input:

    nums = [2, 3, 1, 1, 4]

Output:

    true

Explanation:

Start at index 0.

    Index 0 -> Index 1 -> Index 4

At index 0, the maximum jump length is 2.

We can move to index 1, where the maximum jump length is 3.

From index 1, we can reach index 4.

Therefore, the last index is reachable.

## 4. Example 2

Input:

    nums = [3, 2, 1, 0, 4]

Output:

    false

Explanation:

From index 0, we can reach indices 1, 2, or 3.

However, index 3 contains 0, so we cannot jump forward
from there.

The last index, index 4, cannot be reached.

Therefore, the answer is false.

## 5. Greedy Concept

Greedy means making a useful choice at every step to
reach the overall goal.

In this problem, we maintain a variable:

    maxjump

It represents the maximum index we can reach so far.

At every reachable index, we calculate how far we can
jump from that index.

The formula is:

    maxjump = max(maxjump, i + nums[i])

Here:
- `i` is the current index.
- `nums[i]` is the maximum jump length at that index.
- `i + nums[i]` is the farthest index reachable from it.
- `max()` keeps the greater of the old and new reach.

We continue updating `maxjump` while moving from left
to right.

## 6. Approach — Step by Step

### Step 1: Initialize maxjump

    int maxjump = 0;

Initially, we are at index 0, so our maximum reachable
index is 0.

### Step 2: Traverse the array

    for (int i = 0; i < nums.size(); i++)

Visit each index from left to right.

### Step 3: Check whether the current index is reachable

    if (i > maxjump) {
        return false;
    }

If the current index is greater than `maxjump`, we cannot
reach that index.

Therefore, return false.

### Step 4: Update the maximum reachable index

    maxjump = max(maxjump, i + nums[i]);

Calculate how far we can reach from the current index.

Keep the maximum reachable index found so far.

### Step 5: Return true

    return true;

If the loop finishes without finding an unreachable
index, the last index is reachable.

Return true.

## 7. C++ Code

```cpp
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int maxjump = 0;

        for (int i = 0; i < nums.size(); i++) {
            if (i > maxjump) {
                return false;
            }

            maxjump = max(maxjump, i + nums[i]);
        }

        return true;
    }
};
```

## 8. Code Explanation

### A. Initialize maxjump

```cpp
int maxjump = 0;
```

Initially, we can reach index 0.

Therefore, the maximum reachable index starts at 0.

### B. Traverse the array

```cpp
for (int i = 0; i < nums.size(); i++)
```

We check every index from left to right.

### C. Check reachability

```cpp
if (i > maxjump) {
    return false;
}
```

Suppose:

    i = 4
    maxjump = 3

Since index 4 is beyond our maximum reachable index 3,
we cannot reach index 4.

Therefore, return false.

This check must happen before updating `maxjump`.

### D. Update maximum reach

```cpp
maxjump = max(maxjump, i + nums[i]);
```

Suppose:

    i = 1
    nums[i] = 3
    maxjump = 2

The farthest index reachable from the current index is:

    i + nums[i] = 1 + 3 = 4

Now compare:

    max(2, 4) = 4

So:

    maxjump = 4

We can now reach as far as index 4.

### E. Return true

```cpp
return true;
```

If the loop finishes, every index we processed was
reachable.

Since the loop includes the last index, reaching the end
is possible.

## 9. Detailed Dry Run — Example 1

Input:

    nums = [2, 3, 1, 1, 4]

Initially:

    maxjump = 0

### Step 1: i = 0

Current value:

    nums[0] = 2

Check:

    i > maxjump
    0 > 0

False. Index 0 is reachable.

Update:

    maxjump = max(0, 0 + 2)
    maxjump = 2

Now we can reach index 2.

### Step 2: i = 1

Current value:

    nums[1] = 3

Check:

    1 > 2

False. Index 1 is reachable.

Update:

    maxjump = max(2, 1 + 3)
    maxjump = max(2, 4)
    maxjump = 4

Now we can reach index 4.

### Step 3: i = 2

Current value:

    nums[2] = 1

Check:

    2 > 4

False.

Update:

    maxjump = max(4, 2 + 1)
    maxjump = max(4, 3)
    maxjump = 4

The maximum reachable index remains 4.

### Step 4: i = 3

Current value:

    nums[3] = 1

Index 3 is reachable because:

    3 <= 4

Update:

    maxjump = max(4, 3 + 1)
    maxjump = 4

### Step 5: i = 4

Current value:

    nums[4] = 4

Check:

    4 > 4

False. The last index is reachable.

Update:

    maxjump = max(4, 4 + 4)
    maxjump = 8

The loop finishes.

Return:

    true

Final output:

    true

## 10. Dry Run Table — Example 1

Input:

    nums = [2, 3, 1, 1, 4]

| i | nums[i] | maxjump Before Update | New Reach | maxjump After Update |
|---:|---:|---:|---:|---:|
| 0 | 2 | 0 | 2 | 2 |
| 1 | 3 | 2 | 4 | 4 |
| 2 | 1 | 4 | 3 | 4 |
| 3 | 1 | 4 | 4 | 4 |
| 4 | 4 | 4 | 8 | 8 |

Output:

    true

## 11. Detailed Dry Run — Example 2

Input:

    nums = [3, 2, 1, 0, 4]

Initially:

    maxjump = 0

### Step 1: i = 0

Current value:

    nums[0] = 3

Index 0 is reachable.

Update:

    maxjump = max(0, 0 + 3)
    maxjump = 3

We can reach index 3.

### Step 2: i = 1

Current value:

    nums[1] = 2

Index 1 is reachable.

Update:

    maxjump = max(3, 1 + 2)
    maxjump = max(3, 3)
    maxjump = 3

### Step 3: i = 2

Current value:

    nums[2] = 1

Index 2 is reachable.

Update:

    maxjump = max(3, 2 + 1)
    maxjump = 3

### Step 4: i = 3

Current value:

    nums[3] = 0

Index 3 is reachable because:

    3 <= maxjump

Update:

    maxjump = max(3, 3 + 0)
    maxjump = 3

We cannot jump any farther from index 3.

### Step 5: i = 4

Current index:

    i = 4

Maximum reachable index:

    maxjump = 3

Check:

    i > maxjump
    4 > 3

This is true.

Therefore:

    return false;

The last index cannot be reached.

Final output:

    false

## 12. Dry Run Table — Example 2

Input:

    nums = [3, 2, 1, 0
