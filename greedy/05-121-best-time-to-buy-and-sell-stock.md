# Greedy Pattern — Question 5
# LC 121 — Best Time to Buy and Sell Stock

## 1. Problem Details

- **LeetCode Number:** 121
- **Pattern:** Greedy
- **Difficulty:** Easy
- **Topics:** Array, Greedy

## 2. Problem Statement

We are given an array `prices`.

- `prices[i]` represents the stock price on day `i`.
- We must choose one day to buy the stock.
- We must sell the stock on a future day.
- We can complete at most one transaction.

Our task is to return the maximum possible profit.

If we cannot make any profit, return 0.

Important:
- We must buy before selling.
- We cannot sell before buying.
- We can make only one buy-and-sell transaction.

## 3. Example

Input:

    prices = [7, 1, 5, 3, 6, 4]

Output:

    5

Explanation:

Buy the stock on the day when its price is 1.

Sell the stock later when its price is 6.

Profit:

    Selling Price - Buying Price
    = 6 - 1
    = 5

Therefore, the maximum profit is 5.

## 4. Greedy Concept

Greedy means making a useful choice at each step to obtain
the overall best result.

In this problem, we want to buy at a low price and sell at
a higher price on a future day.

Instead of checking every possible buying and selling pair,
we maintain two variables:

- `minPrice`: The lowest price seen so far.
- `maxProfit`: The maximum profit found so far.

For each day, we calculate the profit we could make by
selling at today's price.

    profit = currentPrice - minPrice

Then we update the maximum profit if today's profit is
better than the previous maximum.

If today's price is lower than `minPrice`, we update
`minPrice` because we have found a cheaper buying price.

## 5. Approach — Step by Step

### Step 1: Initialize minimum price

    int minPrice = prices[0];

Initially, the first day's price is the lowest price we
have seen.

### Step 2: Initialize maximum profit

    int maxProfit = 0;

Initially, no transaction has been completed, so profit is 0.

### Step 3: Traverse the array

Start from index 1 because index 0 has already been used
to initialize `minPrice`.

    for (int i = 1; i < prices.size(); i++)

### Step 4: Update the minimum buying price

    if (prices[i] < minPrice) {
        minPrice = prices[i];
    }

If today's price is lower than the minimum price seen
earlier, update `minPrice`.

### Step 5: Calculate today's potential profit

    int profit = prices[i] - minPrice;

This calculates the profit possible using today's price
as the selling price.

### Step 6: Update maximum profit

    if (profit > maxProfit) {
        maxProfit = profit;
    }

If today's profit is greater than the maximum profit
found so far, update `maxProfit`.

### Step 7: Return the answer

    return maxProfit;

The answer is the maximum profit found during traversal.

## 6. C++ Code

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int minPrice = prices[0];
        int maxProfit = 0;

        for (int i = 1; i < prices.size(); i++) {

            if (prices[i] < minPrice) {
                minPrice = prices[i];
            }

            int profit = prices[i] - minPrice;

            if (profit > maxProfit) {
                maxProfit = profit;
            }
        }

        return maxProfit;
    }
};
```

## 7. Code Explanation

### A. Initialize minimum price

```cpp
int minPrice = prices[0];
```

We store the first day's price as the minimum price
initially.

For example:

    prices = [7, 1, 5, 3, 6, 4]

Initially:

    minPrice = 7

### B. Initialize maximum profit

```cpp
int maxProfit = 0;
```

We have not found any profitable transaction yet.

Therefore, the initial maximum profit is 0.

### C. Traverse the prices

```cpp
for (int i = 1; i < prices.size(); i++)
```

We start from index 1 because index 0 is already stored
in `minPrice`.

Each iteration processes one day's stock price.

### D. Update the minimum price

```cpp
if (prices[i] < minPrice) {
    minPrice = prices[i];
}
```

If today's price is lower than the minimum price seen
so far, update `minPrice`.

Example:

    minPrice = 7
    prices[i] = 1

Since:

    1 < 7

We update:

    minPrice = 1

This gives us a lower potential buying price.

### E. Calculate profit

```cpp
int profit = prices[i] - minPrice;
```

Profit is calculated by subtracting the buying price
from the selling price.

Example:

    Current price = 6
    Minimum price = 1

    profit = 6 - 1
           = 5

We calculate this for every day.

### F. Update maximum profit

```cpp
if (profit > maxProfit) {
    maxProfit = profit;
}
```

Suppose:

    maxProfit = 4
    profit = 5

Since 5 is greater than 4, we update:

    maxProfit = 5

If today's profit is smaller, the previous maximum
profit remains unchanged.

### G. Return the answer

```cpp
return maxProfit;
```

After processing every day, return the maximum profit found.

## 8. Detailed Dry Run

Input:

    prices = [7, 1, 5, 3, 6, 4]

### Initial State

The first day's price is 7.

    minPrice = 7
    maxProfit = 0

We begin the loop from index 1.

### Step 1: i = 1, price = 1

Check:

    prices[i] < minPrice
    1 < 7

This is true.

Update:

    minPrice = 1

Calculate profit:

    profit = 1 - 1 = 0

Compare with maximum profit:

    0 > 0

This is false.

Current values:

    minPrice = 1
    maxProfit = 0

### Step 2: i = 2, price = 5

Check:

    5 < 1

False. The minimum price stays 1.

Calculate profit:

    profit = 5 - 1 = 4

Compare:

    4 > 0

True.

Update:

    maxProfit = 4

Current values:

    minPrice = 1
    maxProfit = 4

### Step 3: i = 3, price = 3

Check:

    3 < 1

False. The minimum price stays 1.

Calculate profit:

    profit = 3 - 1 = 2

Compare:

    2 > 4

False.

The maximum profit remains 4.

Current values:

    minPrice = 1
    maxProfit = 4

### Step 4: i = 4, price = 6

Check:

    6 < 1

False. The minimum price stays 1.

Calculate profit:

    profit = 6 - 1 = 5

Compare:

    5 > 4

True.

Update:

    maxProfit = 5

Current values:

    minPrice = 1
    maxProfit = 5

### Step 5: i = 5, price = 4

Check:

    4 < 1

False. The minimum price stays 1.

Calculate profit:

    profit = 4 - 1 = 3

Compare:

    3 > 5

False.

The maximum profit remains 5.

### Final Answer

After checking all prices:

    minPrice = 1
    maxProfit = 5

Return:

    5

Output:

    5

## 9. Dry Run Table

Input:

    prices = [7, 1, 5, 3, 6, 4]

| Day Index | Price | Minimum Price | Today's Profit | Maximum Profit |
|---:|---:|---:|---:|---:|
| 0 | 7 | 7 | - | 0 |
| 1 | 1 | 1 | 0 | 0 |
| 2 | 5 | 1 | 4 | 4 |
| 3 | 3 | 1 | 2 | 4 |
| 4 | 6 | 1 | 5 | 5 |
| 5 | 4 | 1 | 3 | 5 |

Final output:

    5

Best transaction:
- Buy at 1.
- Sell later at 6.
- Maximum profit = 5.

## 10. Another Important Example

Input:

    prices = [7, 6, 4, 3, 1]

Every day's price is lower than the previous day's price.

We never find a future selling price that is higher than
the lowest available buying price.

The maximum profit remains:

    maxProfit = 0

Output:

    0

We return 0 because we are not required to complete a
transaction when no profit is possible.

## 11. Complexity Analysis

Let `n` be the number of days.

### Time Complexity: O(n)

We traverse the prices array once.

Each iteration performs a constant number of operations.

Therefore:

    O(n)

### Extra Space Complexity: O(1)

We use only two variables:

    minPrice
    maxProfit

Therefore:

    O(1)

## 12. Important Interview Points

1. Buy before selling.
2. Only one transaction is allowed.
3. Maintain the minimum price seen so far.
4. Calculate today's potential profit using the minimum price.
5. Update the maximum profit whenever a better profit is found.
6. If prices only decrease, return 0.
7. We do not need nested loops to check every possible pair.
8. Time complexity is O(n), and extra space complexity is O(1).

## 13. Key Takeaway

**Track the lowest buying price and calculate the best
possible profit for each day's selling price.**

This solves the problem in one traversal without checking
every possible buying and selling pair.

## Filename

05-121-best-time-to-buy-and-sell-stock.md
