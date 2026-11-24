
# 12. Target Sum

**LeetCode:** 494  
**Pattern:** Knapsack / Subset Sum  
**Approach:** Recursion + Memoization (Top-Down DP)

## 1. Problem

Given an array of integers, assign either `+` or `-` to every element.

Find the number of ways to achieve the given target.

Example:

```cpp
nums = [1, 1, 1, 1, 1]
target = 3

Output = 5
```

Possible combinations:

```text
+1 +1 +1 +1 -1 = 3
+1 +1 +1 -1 +1 = 3
+1 +1 -1 +1 +1 = 3
+1 -1 +1 +1 +1 = 3
-1 +1 +1 +1 +1 = 3
```

## 2. Approach – Recursion

Every element has two choices:

1. Positive (+): Add the current element to sum.
2. Negative (-): Subtract the current element from sum.

We need to count all possible ways that produce the target.

## 3. Recursion Logic

Function:

```cpp
int solve(int i, int sum, int target, vector<int>& nums)
```

- `i` = Current index.
- `sum` = Current calculated sum.
- `target` = Required sum.
- `nums` = Input array.

### Positive Choice

Add the current element:

```cpp
int positive = solve(i - 1, sum + nums[i], target, nums);
```

### Negative Choice

Subtract the current element:

```cpp
int negative = solve(i - 1, sum - nums[i], target, nums);
```

### Final Answer

```cpp
return positive + negative;
```

We add both answers because we need to count all valid ways.

## 4. Base Case

```cpp
if (i < 0) {
    return sum == target;
}
```

When all elements have been processed:

- If sum equals target, return `1` (one valid way).
- Otherwise, return `0` (no valid way).

## 5. Why Recursion Gives TLE?

Plain recursion calculates the same states multiple times.

A state is identified by:

- Current index `i`
- Current sum `sum`

We use memoization to store the result of each state and avoid repeated calculations.

## 6. Memoization (DP)

### Problem: Sum Can Be Negative

In this question, sum can be negative or positive.

Example:

```text
-6 -5 -4 -3 -2 -1 0 1 2 3 4 5 6
```

But a vector cannot use negative indexes.

### 1. Offset = Total

```cpp
int offset = total;
```

Offset helps convert negative sums into valid positive DP indexes.

Example:

```cpp
sum = -2;
offset = 6;

index = sum + offset;
index = -2 + 6 = 4;
```

Actual sum remains `-2`. Only its DP storage index becomes `4`.

### 2. Why 2 * total + 1?

Possible sums range from `-total` to `+total`.

If total = 6:

```text
-6 -5 -4 -3 -2 -1 0 1 2 3 4 5 6
```

Total positions = 13.

Therefore:

```cpp
vector<vector<int>> dp(n, vector<int>(2 * total + 1, -1));
```

Formula:

```text
Negative range + Positive range + Zero
= total + total + 1
= 2 * total + 1
```

### 3. Why sum + offset?

```cpp
dp[i][sum + offset]
```

It converts the actual sum into a valid DP column index.

Example:

| Actual Sum | Offset | DP Index |
|---:|---:|---:|
| -6 | 6 | 0 |
| -3 | 6 | 3 |
| -2 | 6 | 4 |
| 0 | 6 | 6 |
| 2 | 6 | 8 |
| 6 | 6 | 12 |

Important:

- `sum` is used for actual mathematical calculations.
- `sum + offset` is used only for DP indexing.
- Offset does not change the actual sum or recursion logic.

### Check Already Calculated State

```cpp
if (dp[i][sum + offset] != -1)
    return dp[i][sum + offset];
```

### Store the Answer

```cpp
return dp[i][sum + offset] = positive + negative;
```

## 7. Complete Code (Memoization)

```cpp
class Solution {
public:
    int solve(int i, int sum, int target, vector<int>& nums,
              vector<vector<int>>& dp, int offset) {

        if (i < 0) {
            return sum == target;
        }

        if (dp[i][sum + offset] != -1)
            return dp[i][sum + offset];

        int positive = solve(i - 1, sum + nums[i], target, nums, dp, offset);

        int negative = solve(i - 1, sum - nums[i], target, nums, dp, offset);

        return dp[i][sum + offset] = positive + negative;
    }

    int findTargetSumWays(vector<int>& nums, int target) {
        int n = nums.size();
        int total = 0;

        for (int num : nums) {
            total += num;
        }

        int offset = total;

        vector<vector<int>> dp(n, vector<int>(2 * total + 1, -1));

        return solve(n - 1, 0, target, nums, dp, offset);
    }
};
```

## 8. Dry Run

Input:

```cpp
nums = [1, 1]
target = 0
```

Possible combinations:

```text
+1 +1 = 2
+1 -1 = 0  ✓
-1 +1 = 0  ✓
-1 -1 = -2
```

Recursion tree:

```text
                 solve(1, 0)
                 /         \
          Positive         Negative
          sum = 1          sum = -1
          /    \           /     \
       +1       -1       +1       -1
        2        0        0       -2
                 ✓        ✓
```

Two valid ways produce target 0.

Output:

```cpp
2
```

## 9. Complexity Analysis

Let:
- `n` = Number of elements
- `S` = Total sum of array

**Time Complexity:** O(n × S)

Each index and sum combination is calculated at most once.

**Space Complexity:** O(n × S) for DP + O(n) recursion stack.

## 10. Key Takeaways

- Every element has two choices: Positive and Negative.
- Positive adds the current number.
- Negative subtracts the current number.
- We count ways using `positive + negative`.
- Base case returns `1` if sum equals target, otherwise `0`.
- Memoization avoids repeated calculations.
- Offset handles negative sums in a normal 2D vector.
- `2 * total + 1` creates space for all possible sums.
- `sum + offset` converts actual sum into a valid DP index.

**Next:** LC 1049 – Last Stone Weight II
