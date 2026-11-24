
# 11. Partition Equal Subset Sum

**LeetCode:** 416  
**Pattern:** Knapsack / Subset Sum  
**Approach:** Recursion + Memoization (Top-Down DP)

## 1. Problem

Given an array, check whether it can be divided into two subsets such that both subsets have equal sum.

Example:

```cpp
nums = [1, 5, 11, 5]
Output = true
```

Possible partition:

- Subset 1 = [11] → Sum = 11
- Subset 2 = [1, 5, 5] → Sum = 11

## 2. Approach

We need to find whether any subset has a sum equal to half of the total array sum.

### Step 1: Calculate Total Sum

```cpp
int total = 0;

for (int num : nums) {
    total += num;
}
```

### Step 2: Check Odd Sum

If total sum is odd, equal partition is impossible.

```cpp
if (total % 2 != 0) return false;
```

### Step 3: Calculate Target

```cpp
int target = total / 2;
```

Now we only need to find whether a subset with sum equal to target exists.

## 3. Take / Not Take Logic

For every element, we have two choices:

**Take:**
- Include the current element.
- Subtract its value from target.
- Move to the previous index.

```cpp
take = solve(i - 1, target - nums[i], nums, dp);
```

**Not Take:**
- Do not include the current element.
- Target remains unchanged.
- Move to the previous index.

```cpp
not_take = solve(i - 1, target, nums, dp);
```

Final condition:

```cpp
return take || not_take;
```

If either choice gives a valid subset, return true.

## 4. Base Cases

```cpp
if (target == 0) return true;
if (i < 0) return false;
```

- `target == 0`: Required sum is found.
- `i < 0`: All elements are exhausted, but target is still not zero.

## 5. Why Recursion Gives TLE?

In plain recursion, the same `i` and `target` combinations can be calculated multiple times.

This causes repeated work and exponential time complexity.

**Solution:** Memoization (DP).

We store already calculated answers in a DP array.

## 6. Memoization (DP)

### DP Declaration

```cpp
vector<vector<int>> dp(n, vector<int>(target + 1, -1));
```

- Rows represent indices `0` to `n-1`.
- Columns represent target values `0` to `target`.
- `-1` means the state has not been calculated yet.

### Why target + 1?

If `target = 11`, we need columns for:

```text
0 1 2 3 4 5 6 7 8 9 10 11
```

Total = 12 columns.

Therefore, we use `target + 1`.

### Check Already Calculated State

```cpp
if (dp[i][target] != -1)
    return dp[i][target];
```

If the answer is already stored, return it directly.

### Store the Answer

```cpp
return dp[i][target] = take || not_take;
```

Calculate once and store the result for future use.

## 7. Complete Code (Memoization)

```cpp
class Solution {
public:
    bool solve(int i, int target, vector<int>& nums, vector<vector<int>>& dp) {
        if (target == 0) return true;
        if (i < 0) return false;

        if (dp[i][target] != -1)
            return dp[i][target];

        bool not_take = solve(i - 1, target, nums, dp);

        bool take = false;
        if (nums[i] <= target) {
            take = solve(i - 1, target - nums[i], nums, dp);
        }

        return dp[i][target] = take || not_take;
    }

    bool canPartition(vector<int>& nums) {
        int n = nums.size();
        int total = 0;

        for (int num : nums) {
            total += num;
        }

        if (total % 2 != 0) return false;

        int target = total / 2;

        vector<vector<int>> dp(n, vector<int>(target + 1, -1));

        return solve(n - 1, target, nums, dp);
    }
};
```

## 8. Dry Run

Input:

```cpp
nums = [1, 5, 11, 5]
```

Total sum = 22

Target = 22 / 2 = 11

One successful recursion path:

```text
solve(3, 11)
    |
    | Not Take (5)
    v
solve(2, 11)
    |
    | Take (11)
    v
solve(1, 0)
    |
    v
return true
```

Explanation:

1. Skip the last 5.
2. Take 11.
3. Target becomes 0.
4. Base case returns true.
5. This true propagates back through `take || not_take`.

Final Output:

```cpp
true
```

## 9. Complexity Analysis

Let:
- `n` = Number of elements
- `target` = Total sum / 2

**Time Complexity:** O(n × target)

Each index and target combination is calculated at most once.

**Space Complexity:** O(n × target) for DP + O(n) recursion stack.

## 10. Key Takeaways

- Equal partition means both subsets have the same sum.
- If total sum is odd, return false.
- Target = total sum / 2.
- Every element has two choices: Take or Not Take.
- Take reduces target.
- Not Take keeps target unchanged.
- `take || not_take` checks whether either choice succeeds.
- Memoization prevents repeated calculations.

**Next:** LC 494 – Target Sum
