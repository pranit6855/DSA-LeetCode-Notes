# Greedy Pattern — Question 6
# LC 122 — Best Time to Buy and Sell Stock II

## 1. Problem Details

- **LeetCode Number:** 122
- **Pattern:** Greedy
- **Difficulty:** Medium
- **Topics:** Array, Greedy

## 2. Problem Statement

We are given an array `prices`, where `prices[i]`
represents the stock price on day `i`.

We can buy and sell the stock multiple times to maximize
our total profit.

Rules:
- We can hold at most one share at a time.
- We must sell before buying again.
- We can make multiple transactions.
- We cannot buy and sell on the same day.

Our task is to return the maximum total profit.

## 3. Example

Input:

    prices = [7, 1, 5, 3, 6, 4]

Output:

    7

Explanation:

Transaction 1:
- Buy at price 1.
- Sell at price 5.
- Profit = 5 - 1 = 4.

Transaction 2:
- Buy at price 3.
- Sell at price 6.
- Profit = 6 - 3 = 3.

Total profit:

    4 + 3 = 7

Therefore, the maximum total profit is 7.

## 4. Greedy Concept

Greedy means making a suitable choice at every step to
maximize the overall result.

In this problem, multiple transactions are allowed.

The main idea is:

**Add every positive difference between consecutive prices.**

If today's price is greater than yesterday's price, we
can add that increase to our total profit.

Formula:

    If prices[i] > prices[i - 1]:

        profit += prices[i] - prices[i - 1]

If today's price is less than or equal to yesterday's
price, we do not add anything.

## 5. Difference Between LC 121 and LC 122

### LC 121 — Best Time to Buy and Sell Stock

- Only one transaction is allowed.
- We track the minimum buying price.
- We calculate the maximum single profit.

### LC 122 — Best Time to Buy and Sell Stock II

- Multiple transactions are allowed.
- We can sell and then buy again.
- We add all positive consecutive price differences.

## 6. Approach — Step by Step

### Step 1: Initialize profit

    int profit = 0;

Initially, no profit has been made.

### Step 2: Traverse the array

    for (int i = 1; i < prices.size(); i++)

Start from index 1 because we need to compare today's
price with the previous day's price.

### Step 3: Check whether the price increased

    if (prices[i] > prices[i - 1])

If today's price is greater than yesterday's price,
there is a positive difference.

### Step 4: Add the difference to profit

    profit += prices[i] - prices[i - 1];

This adds today's price increase to the total profit.

If the price decreased or stayed the same, we skip it.

### Step 5: Return the total profit

    return profit;

After processing all prices, return the accumulated profit.

## 7. C++ Code

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int profit = 0;

        for (int i = 1; i < prices.size(); i++) {
            if (prices[i] > prices[i - 1]) {
                profit += prices[i] - prices[i - 1];
            }
        }

        return profit;
    }
};
```

## 8. Code Explanation

### A. Initialize profit

```cpp
int profit = 0;
```

This variable stores the total profit from all
profitable price increases.

Initially, profit is zero.

### B. Start from index 1

```cpp
for (int i = 1; i < prices.size(); i++)
```

We start from index 1 because we compare the current
price with the previous price.

For example:

    prices = [7, 1, 5]

At index 1:
- Current price = 1
- Previous price = 7

At index 2:
- Current price = 5
- Previous price = 1

### C. Check for a price increase

```cpp
if (prices[i] > prices[i - 1])
```

If today's price is higher than yesterday's price,
the difference is positive.

Example:

    Previous price = 1
    Current price = 5

Since 5 > 1, we can add the increase to our profit.

### D. Add the price difference

```cpp
profit += prices[i] - prices[i - 1];
```

Suppose:

    profit = 0
    prices[i] = 5
    prices[i - 1] = 1

Then:

    profit = 0 + (5 - 1)
    profit = 4

If another positive increase is found, it is added
to the existing profit.

### E. Return the result

```cpp
return profit;
```

After processing every day, return the total profit.

## 9. Detailed Dry Run

Input:

    prices = [7, 1, 5, 3, 6, 4]

Initially:

    profit = 0

### Step 1: i = 1

Current price:

    prices[1] = 1

Previous price:

    prices[0] = 7

Check:

    1 > 7

False.

The price decreased, so we do not add any profit.

    profit = 0

### Step 2: i = 2

Current price:

    prices[2] = 5

Previous price:

    prices[1] = 1

Check:

    5 > 1

True.

Calculate the difference:

    5 - 1 = 4

Update:

    profit = 0 + 4
    profit = 4

### Step 3: i = 3

Current price:

    prices[3] = 3

Previous price:

    prices[2] = 5

Check:

    3 > 5

False.

The price decreased, so we skip it.

    profit = 4

### Step 4: i = 4

Current price:

    prices[4] = 6

Previous price:

    prices[3] = 3

Check:

    6 > 3

True.

Calculate the difference:

    6 - 3 = 3

Update:

    profit = 4 + 3
    profit = 7

### Step 5: i = 5

Current price:

    prices[5] = 4

Previous price:

    prices[4] = 6

Check:

    4 > 6

False.

The price decreased, so we do not add anything.

    profit = 7

### Final Answer

After processing all prices:

    return profit;

Output:

    7

## 10. Dry Run Table

Input:

    prices = [7, 1, 5, 3, 6, 4]

| i | Previous Price | Current Price | Decision | Profit |
|---:|---:|---:|---|---:|
| Initial | - | - | Start | 0 |
| 1 | 7 | 1 | Skip decrease | 0 |
| 2 | 1 | 5 | Add 5 - 1 = 4 | 4 |
| 3 | 5 | 3 | Skip decrease | 4 |
| 4 | 3 | 6 | Add 6 - 3 = 3 | 7 |
| 5 | 6 | 4 | Skip decrease | 7 |

Final output:

    7

## 11. Another Example

Input:

    prices = [1, 2, 3, 4, 5]

Every day, the price increases.

Differences:

    2 - 1 = 1
    3 - 2 = 1
    4 - 3 = 1
    5 - 4 = 1

Total profit:

    1 + 1 + 1 + 1 = 4

Output:

    4

We can also buy at 1 and sell at 5 to earn 4.
Adding the consecutive increases gives the same result.

## 12. Complexity Analysis

Let `n` be the number of days.

### Time Complexity: O(n)

We traverse the array exactly once.

Each iteration performs a constant number of operations.

Therefore:

    O(n)

### Extra Space Complexity: O(1)

We use only one extra variable:

    profit

Therefore:

    O(1)

## 13. Important Interview Points

1. Multiple transactions are allowed.
2. We can hold only one share at a time.
3. Compare each day's price with the previous day's price.
4. Add the difference only when the current price is higher.
5. Ignore negative and zero differences.
6. The final answer is the sum of all positive consecutive
   price differences.
7. Time complexity is O(n).
8. Extra space complexity is O(1).

## 14. Key Takeaway

**If today's price is higher than yesterday's price,
add the difference to the total profit.**

Since multiple transactions are allowed, adding all
positive consecutive price increases gives the maximum
possible total profit.

## Filename

06-122-best-time-to-buy-and-sell-stock-ii.md
