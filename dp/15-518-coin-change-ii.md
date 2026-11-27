
# 15. Coin Change II

**LeetCode:** 518  
**Pattern:** Knapsack / Coins  
**Approach:** Recursion + Memoization (Top-Down DP)  
**Type:** Unbounded Knapsack

## 1. Problem

Given an array of coin denominations and an integer `amount`, find the number of different combinations that make up that amount.

You can use each coin unlimited times.

Example:

```cpp
coins = [1, 2, 5]
amount = 5

Output = 4
```

Possible combinations:

1. `5`
2. `2 + 2 + 1`
3. `2 + 1 + 1 + 1`
4. `1 + 1 + 1 + 1 + 1`

**Important:** Order does not matter. `1 + 2` and `2 + 1` are the same combination.

## 2. Approach

For every coin, we have two choices:

1. Take: Use the current coin.
2. Not Take: Skip the current coin.

We need to count the total number of different combinations.

This is an **Unbounded Knapsack** problem because each coin can be used unlimited times.

### Important Difference

- Take → Same index `i`, because the coin can be reused.
- Not Take → Previous index `i - 1`, because we skip the current coin.

## 3. Function Meaning

```cpp
solve(i, amount)
```

Find the total number of different combinations to make the given amount using coins from index `0` to `i`.

## 4. Base Cases

### Amount becomes zero

```cpp
if (amount == 0) {
    return 1;
}
```

If the amount becomes zero, we have successfully formed one valid combination.

We return `1`.

### All coins are exhausted

```cpp
if (i < 0) {
    return 0;
}
```

If all coins are exhausted but the amount is still remaining, no valid combination is possible.

We return `0`.

## 5. Recursion Logic

### Not Take

```cpp
int not_take = solve(i - 1, amount, coins);
```

Skip the current coin and move to the previous coin.

- Index decreases by one.
- Amount remains unchanged.

### Take

```cpp
int take = 0;

if (coins[i] <= amount) {
    take = solve(i, amount - coins[i], coins);
}
```

Use the current coin.

- Reduce the remaining amount.
- Keep the same index because the coin can be reused.

### Return Total Combinations

```cpp
return take + not_take;
```

Add the combinations obtained from both choices.

## 6. Recursion Code

```cpp
class Solution {
public:
    int solve(int i, int amount, vector<int>& coins) {
        if (amount == 0) {
            return 1;
        }

        if (i < 0) {
            return 0;
        }

        int not_take = solve(i - 1, amount, coins);

        int take = 0;
        if (coins[i] <= amount) {
            take = solve(i, amount - coins[i], coins);
        }

        return take + not_take;
    }

    int change(int amount, vector<int>& coins) {
        int n = coins.size();

        return solve(n - 1, amount, coins);
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
n = 2;
amount = 5;
```

DP dimensions:

```cpp
vector<vector<int>> dp(2, vector<int>(6, -1));
```

We need 6 columns because amounts range from `0` to `5`.

## 9. Memoization Code

```cpp
class Solution {
public:
    int solve(int i, int amount, vector<int>& coins, vector<vector<int>>& dp) {
        if (amount == 0) {
            return 1;
        }

        if (i < 0) {
            return 0;
        }

        if (dp[i][amount] != -1) {
            return dp[i][amount];
        }

        int not_take = solve(i - 1, amount, coins, dp);

        int take = 0;
        if (coins[i] <= amount) {
            take = solve(i, amount - coins[i], coins, dp);
        }

        return dp[i][amount] = take + not_take;
    }

    int change(int amount, vector<int>& coins) {
        int n = coins.size();

        vector<vector<int>> dp(n, vector<int>(amount + 1, -1));

        return solve(n - 1, amount, coins, dp);
    }
};
```

## 10. What Changed in Memoization?

Only three additions:

### 1. DP Array

```cpp
vector<vector<int>> dp(n, vector<int>(amount + 1, -1));
```

Stores answers for different states.

### 2. Check Existing Answer

```cpp
if (dp[i][amount] != -1) {
    return dp[i][amount];
}
```

If the state is already calculated, return its stored answer.

### 3. Store Calculated Answer

```cpp
return dp[i][amount] = take + not_take;
```

Calculate the total combinations and store the answer.

## 11. Why `take = 0`?

```cpp
int take = 0;

if (coins[i] <= amount) {
    take = solve(i, amount - coins[i], coins, dp);
}
```

If the current coin is greater than the remaining amount, we cannot use it.

Therefore, the Take choice contributes zero combinations.

If the coin can be used, the recursive function calculates the actual number of combinations and replaces the initial value.

Each recursive call has its own local `take` variable. Initializing it to zero does not reset any previously stored DP answer.

## 12. Why `i` in Take and `i - 1` in Not Take?

```cpp
// Take
take = solve(i, amount - coins[i], coins, dp);

// Not Take
not_take = solve(i - 1, amount, coins, dp);
```

**Take:** Keep the same index because we can reuse the current coin.

**Not Take:** Move to the previous index because we have skipped the current coin.

This is the Unbounded Knapsack pattern.

## 13. Dry Run

Input:

```cpp
coins = [1, 2]
amount = 3
```

Possible combinations:

**Combination 1:**

```cpp
1 + 1 + 1 = 3
```

**Combination 2:**

```cpp
1 + 2 = 3
```

`2 + 1` is not counted separately because order does not matter.

Total combinations:

```cpp
1 + 1 = 2
```

Output:

```cpp
2
```

## 14. Complexity Analysis

Let:

- `n` = Number of coins
- `amount` = Given amount

**Time Complexity:**

```cpp
O(n * amount)
```

There are at most `n * (amount + 1)` distinct DP states, and each state performs constant additional work.

**DP Space Complexity:**

```cpp
O(n * amount)
```

**Recursion Stack:**

```cpp
O(n + amount)
```

## 15. Coin Change 322 vs Coin Change II 518

| Coin Change (322) | Coin Change II (518) |
|---|---|
| Minimum coins required | Total combinations |
| `min(take, not_take)` | `take + not_take` |
| Impossible case = large value | Impossible case = `0` |
| Amount 0 returns `0` | Amount 0 returns `1` |
| Unbounded Knapsack | Unbounded Knapsack |

## 16. Important Takeaways

1. This problem follows the Unbounded Knapsack pattern.
2. Every coin has two choices: Take and Not Take.
3. Take uses the same index `i` because coins can be reused.
4. Not Take moves to `i - 1`.
5. Use `take + not_take` because we need total combinations.
6. Order does not matter, so duplicate arrangements are not counted.
7. Memoization stores answers for repeated `(i, amount)` states.

**Key Learning:** Coin Change 322 finds the minimum number of coins, while Coin Change II counts the total number of different combinations.
