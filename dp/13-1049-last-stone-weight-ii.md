
# 13. Last Stone Weight II

**LeetCode:** 1049  
**Pattern:** Knapsack / Subset Sum  
**Approach:** Recursion + Memoization (Top-Down DP)

## 1. Problem

Given an array `stones`, divide all stones into two groups such that the absolute difference between their total weights is minimum.

Example:

```cpp
stones = [2, 7, 4, 1, 8, 1]
Output = 1
```

One possible partition:

- Group 1: [2, 7, 1, 1] = 11
- Group 2: [4, 8] = 12

Difference = |12 - 11| = 1

## 2. Approach

Let:

- `total` = Sum of all stones
- `s1` = Sum of first group
- `s2` = Sum of second group

We know:

```cpp
s1 + s2 = total
```

Minimum difference:

```cpp
difference = abs(s1 - s2)
```

Since `s2 = total - s1`:

```cpp
difference = total - 2 * s1
```

To minimize the difference, we need to find the maximum possible `s1` that does not exceed `total / 2`.

This becomes a Subset Sum problem.

## 3. Recursion Logic

For every stone, we have two choices:

1. Take: Include the current stone in the first group.
2. Not Take: Ignore the current stone.

Our function:

```cpp
solve(i, target)
```

Meaning:

Find the maximum subset sum using stones from index `i` to `0`, without exceeding `target`.

### Base Cases

```cpp
if (i < 0 || target == 0) {
    return 0;
}
```

- `i < 0`: No stones are left.
- `target == 0`: No more weight is required.

### Not Take

```cpp
int not_take = solve(i - 1, target, stones);
```

Skip the current stone.

- Move to the previous index.
- Target remains unchanged.

### Take

```cpp
int take = 0;

if (stones[i] <= target) {
    take = stones[i] + solve(i - 1, target - stones[i], stones);
}
```

Include the current stone.

- Add its weight.
- Reduce the remaining target.
- Move to the previous index.

### Return Maximum

```cpp
return max(take, not_take);
```

Choose the maximum subset sum from both choices.

## 4. Recursion Code

```cpp
class Solution {
public:
    int solve(int i, int target, vector<int>& stones) {
        if (i < 0 || target == 0) {
            return 0;
        }

        int not_take = solve(i - 1, target, stones);

        int take = 0;
        if (stones[i] <= target) {
            take = stones[i] + solve(i - 1, target - stones[i], stones);
        }

        return max(take, not_take);
    }

    int lastStoneWeightII(vector<int>& stones) {
        int n = stones.size();
        int total = 0;

        for (int stone : stones) {
            total += stone;
        }

        int target = total / 2;

        int s1 = solve(n - 1, target, stones);

        return total - 2 * s1;
    }
};
```

## 5. Why Memoization?

In normal recursion, the same state can be calculated multiple times.

A state is identified by:

- Current index `i`
- Remaining `target`

We store the answer for each state in a 2D DP array.

## 6. DP Array

```cpp
vector<vector<int>> dp(n, vector<int>(target + 1, -1));
```

- `n` rows represent stone indices.
- `target + 1` columns represent target values from `0` to `target`.
- `-1` means the state has not been calculated yet.

For example, if:

```cpp
n = 6;
target = 11;
```

DP dimensions:

```cpp
vector<vector<int>> dp(6, vector<int>(12, -1));
```

We need 12 columns because indices go from `0` to `11`.

## 7. Memoization Code

```cpp
class Solution {
public:
    int solve(int i, int target, vector<int>& stones, vector<vector<int>>& dp) {
        if (i < 0 || target == 0) {
            return 0;
        }

        if (dp[i][target] != -1) {
            return dp[i][target];
        }

        int not_take = solve(i - 1, target, stones, dp);

        int take = 0;
        if (stones[i] <= target) {
            take = stones[i] + solve(i - 1, target - stones[i], stones, dp);
        }

        return dp[i][target] = max(take, not_take);
    }

    int lastStoneWeightII(vector<int>& stones) {
        int n = stones.size();
        int total = 0;

        for (int stone : stones) {
            total += stone;
        }

        int target = total / 2;

        vector<vector<int>> dp(n, vector<int>(target + 1, -1));

        int s1 = solve(n - 1, target, stones, dp);

        return total - 2 * s1;
    }
};
```

## 8. What Changed in Memoization?

Only three additions:

**1. DP array**

```cpp
vector<vector<int>> dp(n, vector<int>(target + 1, -1));
```

Stores answers for different states.

**2. Check existing answer**

```cpp
if (dp[i][target] != -1) {
    return dp[i][target];
}
```

If the state is already calculated, return its stored answer.

**3. Store calculated answer**

```cpp
return dp[i][target] = max(take, not_take);
```

Calculate the answer and save it.

## 9. Dry Run

Input:

```cpp
stones = [2, 7, 4, 1, 8, 1]
```

Calculate total:

```cpp
total = 2 + 7 + 4 + 1 + 8 + 1
      = 23
```

Calculate target:

```cpp
target = total / 2
       = 11
```

Now find the maximum subset sum that does not exceed 11.

One such subset:

```cpp
[2, 7, 1, 1] = 11
```

Therefore:

```cpp
s1 = 11;
s2 = 23 - 11;
s2 = 12;
```

Final difference:

```cpp
answer = total - 2 * s1
       = 23 - 2 * 11
       = 1;
```

Output:

```cpp
1
```

## 10. Complexity Analysis

Let:

- `n` = Number of stones
- `target` = `total / 2`

**Time Complexity:**

```cpp
O(n * target)
```

There are at most `n * (target + 1)` distinct DP states, and each state performs constant work apart from recursive calls.

**Space Complexity:**

```cpp
O(n * target)
```

For the 2D DP array.

Additional recursion stack:

```cpp
O(n)
```

## 11. Important Takeaways

1. This problem is based on the Subset Sum pattern.
2. Every stone has two choices: Take or Not Take.
3. We find the maximum subset sum that does not exceed `total / 2`.
4. Memoization stores answers for repeated `(i, target)` states.
5. The final minimum difference is:

```cpp
total - 2 * s1
```

**Key Learning:** Instead of directly finding the minimum difference, find the maximum possible subset sum up to half of the total sum.
