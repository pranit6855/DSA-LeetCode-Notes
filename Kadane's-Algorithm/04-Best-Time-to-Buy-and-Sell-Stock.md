# LeetCode 121 - Best Time to Buy and Sell Stock

## 📌 Problem

Hume ek array `prices` diya gaya hai.

```text
prices[i] = i-th day ka stock price
```

Hume:

- Ek baar stock **buy** karna hai.
- Buy ke **baad** kisi future day par **sell** karna hai.
- Maximum possible profit return karna hai.

Agar koi profit possible nahi hai, to `0` return karna hai.

---

# 🔹 Example 1

```text
prices = [7,1,5,3,6,4]
```

Best transaction:

```text
Buy  → 1
Sell → 6
```

Profit:

```text
6 - 1 = 5
```

Output:

```text
5
```

---

# 🔹 Example 2

```text
prices = [7,6,4,3,1]
```

Prices continuously decrease ho rahe hain.

```text
7 → 6 → 4 → 3 → 1
```

Koi profitable transaction possible nahi hai.

Output:

```text
0
```

---

# 🧠 Main Idea

Har day par hum ye sochenge:

> Agar main **aaj sell** karun, to ab tak ke cheapest price par buy karke mujhe kitna profit milega?

Formula:

```text
Profit = Current Price - Minimum Price So Far
```

Isliye hume sirf 2 variables maintain karne hain:

```text
minPrice
maxProfit
```

---

# 🔥 `minPrice`

`minPrice` ka matlab:

> Ab tak dekha gaya sabse chhota stock price.

Example:

```text
[7,1,5,3,6,4]
```

Starting:

```text
minPrice = 7
```

`1` aaya:

```text
minPrice = 1
```

Ab aage jitne bhi days hain, unke liye `1` cheapest buying price hai.

---

# 🔥 `maxProfit`

`maxProfit` ka matlab:

> Ab tak mila hua maximum profit.

Har day par calculate karenge:

```text
current profit = prices[i] - minPrice
```

Then:

```text
maxProfit = max(maxProfit, current profit)
```

---

# 🔄 Detailed Dry Run

Array:

```text
[7,1,5,3,6,4]
```

Initially:

```text
minPrice = 7
maxProfit = 0
```

---

## i = 1

Current price:

```text
1
```

Minimum update:

```text
minPrice = min(7,1)
         = 1
```

Profit:

```text
1 - 1 = 0
```

So:

```text
maxProfit = max(0,0)
          = 0
```

Current state:

```text
minPrice = 1
maxProfit = 0
```

---

## i = 2

Current price:

```text
5
```

Minimum:

```text
minPrice = min(1,5)
         = 1
```

Profit:

```text
5 - 1 = 4
```

Update:

```text
maxProfit = max(0,4)
          = 4
```

So:

```text
Buy = 1
Sell = 5
Profit = 4
```

---

## i = 3

Current price:

```text
3
```

Minimum:

```text
minPrice = min(1,3)
         = 1
```

Profit:

```text
3 - 1 = 2
```

Maximum already `4` hai:

```text
maxProfit = max(4,2)
          = 4
```

No change.

---

## i = 4

Current price:

```text
6
```

Minimum:

```text
minPrice = min(1,6)
         = 1
```

Profit:

```text
6 - 1 = 5
```

Update:

```text
maxProfit = max(4,5)
          = 5
```

Best transaction:

```text
Buy  = 1
Sell = 6
Profit = 5
```

---

## i = 5

Current price:

```text
4
```

Minimum:

```text
minPrice = 1
```

Profit:

```text
4 - 1 = 3
```

Maximum already:

```text
maxProfit = 5
```

So final:

```text
Answer = 5
```

---

# 🔥 Important: Buy Hamesha Sell Se Pehle

Example:

```text
[7,1,5]
```

Correct:

```text
Buy  = 1
Sell = 5
Profit = 4
```

Hum kabhi:

```text
Buy = 5
Sell = 1
```

nahi kar sakte.

Isliye hum `minPrice` ko **past/current prices** se maintain karte hain aur current price ko selling price treat karte hain.

---

# ⚠️ Order of Operations

Code me:

```cpp
minPrice = min(minPrice, prices[i]);

int profit = prices[i] - minPrice;
```

Pehle minimum update karna safe hai.

Example:

```text
prices[i] = 1
minPrice = 1
```

Then:

```text
profit = 1 - 1 = 0
```

Hum same day ko meaningful profit ke liye use nahi kar rahe.

Agar next day `5` hai:

```text
5 - 1 = 4
```

So buy previous day aur sell current day.

---

# 🔄 Decreasing Array

Example:

```text
[7,6,4,3,1]
```

Initially:

```text
minPrice = 7
maxProfit = 0
```

### `6`

```text
minPrice = 6
profit = 6 - 6 = 0
maxProfit = 0
```

### `4`

```text
minPrice = 4
profit = 4 - 4 = 0
maxProfit = 0
```

### `3`

```text
minPrice = 3
profit = 0
maxProfit = 0
```

### `1`

```text
minPrice = 1
profit = 0
maxProfit = 0
```

Final:

```text
0
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {

        int minPrice = prices[0];
        int maxProfit = 0;

        for(int i = 1; i < prices.size(); i++){

            minPrice = min(minPrice, prices[i]);

            int profit = prices[i] - minPrice;

            maxProfit = max(maxProfit, profit);
        }

        return maxProfit;
    }
};
```

---

# 🧩 Code Explanation

### Initialization

```cpp
int minPrice = prices[0];
```

First day ka price initially cheapest price maan liya.

```cpp
int maxProfit = 0;
```

Initially koi transaction nahi kiya, so profit `0`.

---

### Loop

```cpp
for(int i = 1; i < prices.size(); i++)
```

Second day se start kar rahe hain because first day already `minPrice` me stored hai.

---

### Minimum Price

```cpp
minPrice = min(minPrice, prices[i]);
```

Check:

```text
Ab tak ka cheapest price
        vs
Aaj ka price
```

Jo chhota hai, usko `minPrice` bana do.

---

### Current Profit

```cpp
int profit = prices[i] - minPrice;
```

Meaning:

```text
Aaj sell
-
Cheapest previous/current buy
=
Profit
```

---

### Maximum Profit

```cpp
maxProfit = max(maxProfit, profit);
```

Current profit aur previous best profit me jo bada hai, use save karo.

---

# 🧠 Pattern Connection

Ye LC 53 ka exact Kadane nahi hai, but **same optimization thinking** use karta hai.

### LC 53

```text
Current element
      ↓
Continue OR Restart
      ↓
Best Ending Sum
      ↓
Global Maximum
```

### LC 121

```text
Current Price
      ↓
Cheapest Price So Far
      ↓
Current Profit
      ↓
Global Maximum Profit
```

Matlab:

```text
Best previous state maintain karo
        ↓
Current answer calculate karo
        ↓
Global answer update karo
```

---

# 🔥 Why We Don't Use Nested Loops

Brute force me:

```text
Har buying day
    ↓
Har future selling day
    ↓
profit calculate
```

Complexity:

```text
O(n²)
```

But hume har previous price ko baar-baar check karne ki zarurat nahi hai.

Hum bas:

```text
minimum price so far
```

save kar rahe hain.

Therefore:

```text
O(n)
```

---

# ❌ Brute Force

```cpp
for(int i = 0; i < prices.size(); i++){

    for(int j = i + 1; j < prices.size(); j++){

        int profit = prices[j] - prices[i];

        // maximum profit
    }
}
```

Time:

```text
O(n²)
```

---

# ✅ Optimized

```cpp
minPrice
maxProfit
```

Sirf ek loop.

Time:

```text
O(n)
```

---

# ⚠️ Common Mistakes

## 1. Maximum price maintain karna

Wrong:

```cpp
maxPrice
```

Hume selling ke liye maximum price nahi chahiye.

Hume:

```text
Cheapest Buy
```

chahiye.

Because:

```text
Profit = Sell - Buy
```

Buy jitna cheap hoga, profit utna zyada hoga.

---

## 2. Profit ko directly return kar dena

Wrong:

```cpp
return prices[i] - minPrice;
```

Hume **overall maximum profit** chahiye.

Isliye:

```cpp
maxProfit = max(maxProfit, profit);
```

---

## 3. Buy ke baad Sell ka order tod dena

Example:

```text
[7,1,5]
```

Correct:

```text
1 → 5
```

Hum future price ko buy ke liye use nahi kar sakte.

`minPrice` ko left-to-right maintain karne se ye automatically handle hota hai.

---

# 🧠 Quick Dry Run Table

```text
Prices:      7   1   5   3   6   4
             ↓   ↓   ↓   ↓   ↓   ↓
minPrice:    7   1   1   1   1   1
             ↓   ↓   ↓   ↓   ↓   ↓
profit:      -   0   4   2   5   3
             ↓   ↓   ↓   ↓   ↓   ↓
maxProfit:   0   0   4   4   5   5
```

Final:

```text
5
```

---

# ⭐ One-Line Memory Trick

> **Har day ko selling day maan lo, aur usse pehle ka minimum price maintain karke profit calculate karo.**

---

# ⏱️ Complexity

### Time

```text
O(n)
```

Array ko ek baar traverse karte hain.

### Space

```text
O(1)
```

Sirf `minPrice`, `profit`, `maxProfit` variables use kiye hain.

---

# 📁 Kadane's Algorithm Folder

```text
Kadane's-Algorithm/
│
├── 01-Maximum-Subarray.md
├── 02-Maximum-Product-Subarray.md
├── 03-Maximum-Sum-Circular-Subarray.md
└── 04-Best-Time-to-Buy-and-Sell-Stock.md
```

---

# 🔥 Revision

```text
minPrice
    ↓
Cheapest buy so far

prices[i]
    ↓
Today's selling price

prices[i] - minPrice
    ↓
Today's profit

maxProfit
    ↓
Overall best profit
```

### Final Formula

```text
Current Profit = Current Price - Minimum Price So Far

Answer = Maximum Profit So Far
```

### Commit Message

```text
Add LC 121 Best Time to Buy and Sell Stock notes and solution
```
