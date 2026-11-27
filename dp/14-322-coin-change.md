
# 14. Coin Change

**LeetCode:** 322  
**Pattern:** Knapsack / Coins  
**Approach:** Recursion + Memoization (Top-Down DP)  
**Type:** Unbounded Knapsack

## 1. Problem

Given an array of coin denominations and an integer `amount`, find the minimum number of coins required to make that amount.

You can use each coin unlimited times.

If the amount cannot be made, return `-1`.

Example:

```cpp
coins = [1, 2, 5]
amount = 11

Output = 3
```

Explanation:

```cpp
5 + 5 + 1 = 11
```

Minimum number of coins = 3.

## 2. Approach

For every coin, we have two choices:

1. Take: Use the current coin.
2. Not Take: Skip the current coin.

We need the minimum number of coins.

### Important Difference

In previous 0/1 Knapsack problems, after taking an element, we moved to `i - 1`.

But here, coins can be used unlimited times.

Therefore:

- Take → Same index `i`
- Not Take → Previous index `i - 1`

This is called Unbounded Knapsack.

## 3. Function Meaning

```cpp
solve(i, amount)
```

Find the minimum number of coins required to make the given amount using coins from index `0` to `i`.

## 4. Base Cases

```cpp
if (amount == 0) {
    return 0;
}
```

If amount becomes zero, no more coins are required.

```cpp
if (i < 0) {
    return 1000000;
}
```

If all coins are exhausted but amount is still remaining, the amount cannot be formed.

We return a large value to represent an impossible answer.

## 5. Recursion Logic

### Not Take

```cpp
int not_take = solve(i - 1, amount, coins);
```

Skip the current coin and move to the previous coin.

### Take

```cpp
int take = 1000000;

if (coins[i] <= amount) {
    take = 1 + solve(i, amount - coins[i], coins);
}
```

- `1` represents using the current coin.
- Reduce the remaining amount.
- Keep the same index because the coin can be reused.

### Return Minimum

```cpp
return min(take, not_take);
```

We need the minimum number of coins, so we choose the smaller answer.

## 6. Recursion Code

```cpp
class Solution {
public:
    int solve(int i, int amount, vector<int>& coins) {
        if (amount == 0) {
            return 0;
        }

        if (i < 0) {
            return 1000000;
        }

        int not_take = solve(i - 1, amount, coins);

        int take = 1000000;
        if (coins[i] <= amount) {
            take = 1 + solve(i, amount - coins[i], coins);
        }

        return min(take, not_take);
    }

    int coinChange(vector<int>& coins, int amount) {
        int n = coins.size();

        int ans = solve(n - 1, amount, coins);

        if (ans == 1000000) {
            return -1;
        }

        return ans;
    }
};
```

## 7. Why Memoization?

In normal recursion, the same state can be calculated multiple times.

A state is identified by:

- Current coin index `i`
- Remaining `amount`

We store the answer for each state in a 2D DP array.

## 8. DP Array

```cpp
vector<vector<int>> dp(n, vector<int>(amount + 1, -1));
```

- `n` rows represent coin indices.
- `amount + 1` columns represent amounts from `0` to `amount`.
- `-1` means the state has not been calculated yet.

For example:

```cpp
n = 3;
amount = 11;
```

DP dimensions:

```cpp
vector<vector<int>> dp(3, vector<int>(12, -1));
```

We need 12 columns because amount ranges from `0` to `11`.

## 9. Memoization Code

```cpp
class Solution {
public:
    int solve(int i, int amount, vector<int>& coins, vector<vector<int>>& dp) {
        if (amount == 0) {
            return 0;
        }

        if (i < 0) {
            return 1000000;
        }

        if (dp[i][amount] != -1) {
            return dp[i][amount];
        }

        int not_take = solve(i - 1, amount, coins, dp);

        int take = 1000000;
        if (coins[i] <= amount) {
            take = 1 + solve(i, amount - coins[i], coins, dp);
        }

        return dp[i][amount] = min(take, not_take);
    }

    int coinChange(vector<int>& coins, int amount) {
        int n = coins.size();

        vector<vector<int>> dp(n, vector<int>(amount + 1, -1));

        int ans = solve(n - 1, amount, coins, dp);

        if (ans == 1000000) {
            return -1;
        }

        return ans;
    }
};
```

## 10. What Changed in Memoization?

Only three additions:

**1. DP array**

```cpp
vector<vector<int>> dp(n, vector<int>(amount + 1, -1));
```

Stores answers for different states.

**2. Check existing answer**

```cpp
if (dp[i][amount] != -1) {
    return dp[i][amount];
}
```

If the state is already calculated, return its stored answer.

**3. Store calculated answer**

```cpp
return dp[i][amount] = min(take, not_take);
```

Calculate the minimum and save it.

## 11. Dry Run

Input:

```cpp
coins = [1, 2, 5]
amount = 11
```

One optimal combination:

```cpp
5 + 5 + 1 = 11
```

Number of coins:

```cpp
2 + 1 = 3
```

Output:

```cpp
3
```

## 12. Complexity Analysis

Let:

- `n` = Number of coins
- `amount` = Given amount

**Time Complexity:**

```cpp
O(n * amount)
```

There are at most `n * (amount + 1)` distinct DP states.

**DP Space Complexity:**

```cpp
O(n * amount)
```

**Recursion Stack:**

```cpp
O(n + amount)
```

## 13. Important Takeaways

1. This problem follows the Unbounded Knapsack pattern.
2. Every coin has two choices: Take and Not Take.
3. Take uses the same index `i` because coins can be reused.
4. Not Take moves to `i - 1`.
5. Use `min(take, not_take)` because we need minimum coins.
6. Return `-1` if the amount cannot be formed.

**Key Learning:**

In 0/1 Knapsack, taking an element moves to the previous index. In Unbounded Knapsack, taking an element keeps the same index because it can be reused.
