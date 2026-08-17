# LeetCode 1011 - Capacity To Ship Packages Within D Days

## 📌 Problem

Hume ek array `weights` diya gaya hai jisme packages ke weights hain.

```text
weights = [1,2,3,4,5,6,7,8,9,10]
```

Aur:

```text
days = 5
```

Hume ship ki **minimum possible capacity** find karni hai jisse saare packages `days` ke andar ship ho jaayein.

---

# 🔹 Important Conditions

```text
1. Packages ko given order mein hi ship karna hai.
2. Order change nahi kar sakte.
3. Ek day mein packages ka total weight ship capacity se zyada nahi hona chahiye.
4. Ek package ko split nahi kar sakte.
5. Hume minimum possible ship capacity find karni hai.
```

---

# 🔹 Example

```text
weights = [1,2,3,4,5,6,7,8,9,10]
days = 5
```

Answer:

```text
15
```

Agar capacity `15` hai:

```text
Day 1 → 1 + 2 + 3 + 4 + 5 = 15
Day 2 → 6 + 7 = 13
Day 3 → 8 = 8
Day 4 → 9 = 9
Day 5 → 10 = 10
```

Total:

```text
5 days
```

So capacity `15` valid hai.

---

# 🧠 Main Observation

Yahan hume array ke kisi element ko search nahi karna.

Hume **answer ki value** search karni hai.

Answer:

```text
Ship Capacity
```

Possible capacities:

```text
10,11,12,13,...,55
```

Isliye hum:

```text
Binary Search on Answer
```

use karenge.

---

# 🔥 Search Range

## `low`

Ship ki capacity kisi bhi single package ke weight se chhoti nahi ho sakti.

Example:

```text
weights = [1,2,3,4,5,6,7,8,9,10]
```

Maximum package:

```text
10
```

Therefore:

```cpp
int low = *max_element(weights.begin(), weights.end());
```

So:

```text
low = 10
```

---

## `high`

Worst case mein ship ek hi day mein saare packages carry kar sakti hai.

Uske liye capacity:

```text
sum(weights)
```

Example:

```text
1+2+3+4+5+6+7+8+9+10 = 55
```

Therefore:

```cpp
int high = accumulate(weights.begin(), weights.end(), 0);
```

So:

```text
high = 55
```

Initial search space:

```text
low = 10
high = 55
```

---

# 🔥 Why `low = max(weights)`?

Suppose maximum package:

```text
10
```

Agar capacity:

```text
9
```

hai, toh `10` weight ka package ship hi nahi ho sakta.

Therefore:

```text
capacity >= maximum package weight
```

So:

```cpp
low = max(weights);
```

---

# 🔥 Why `high = sum(weights)`?

Agar ship ki capacity saare packages ke total weight ke equal hai:

```text
capacity = 55
```

toh saare packages ek hi day mein ship ho sakte hain.

Therefore:

```text
sum(weights)
```

definitely valid upper bound hai.

---

# 🔹 `mid`

Binary Search mein:

```cpp
int mid = low + (high - low) / 2;
```

`mid` ko hum **possible ship capacity** maanenge.

Then check karenge:

```text
Agar capacity = mid ho,
toh saare packages kitne days mein ship honge?
```

---

# 🔥 Feasibility Check

Har package ko order mein process karenge.

Do variables:

```cpp
int curload = 0;
int daysneed = 1;
```

### `curload`

Current day mein abhi tak kitna weight load hua hai.

### `daysneed`

Abhi tak kitne days required hain.

Initially:

```text
curload = 0
daysneed = 1
```

---

# 🔹 Package Add Karna

Har weight ke liye:

```cpp
if (curload + weight <= mid) {
    curload += weight;
}
```

Matlab current package ko same day mein ship kar sakte hain.

---

# 🔹 New Day Kab Start Hoga?

Agar:

```cpp
curload + weight > mid
```

toh current package current day mein fit nahi hoga.

Therefore new day:

```cpp
else {
    daysneed++;
    curload = weight;
}
```

Important:

```text
daysneed++
```

because new day start hua.

Aur:

```text
curload = weight
```

because current package new day ka first package hai.

---

# 🔥 Main Condition

**Saare packages process karne ke BAAD** `daysneed` ka final value check karenge.

```cpp
if (daysneed <= days) {
    high = mid - 1;
}
else {
    low = mid + 1;
}
```

---

# 🔹 Case 1 — Capacity Valid

Agar:

```text
daysneed <= days
```

Example:

```text
daysneed = 4
days = 5
```

Capacity valid hai.

But hume **minimum capacity** chahiye.

Therefore smaller capacity try karenge:

```cpp
high = mid - 1;
```

Meaning:

```text
LEFT
```

---

# 🔹 Case 2 — Capacity Too Small

Agar:

```text
daysneed > days
```

Example:

```text
daysneed = 6
days = 5
```

Capacity too small hai.

Hume capacity increase karni padegi:

```cpp
low = mid + 1;
```

Meaning:

```text
RIGHT
```

---

# ⚠️ Important Mistake

Condition ko `for` loop ke andar nahi rakhna.

Wrong:

```cpp
for (int weight : weights) {

    if (curload + weight <= mid) {
        curload += weight;
    }
    else {
        daysneed++;
        curload = weight;
    }

    // ❌ Wrong place
    if (daysneed <= days) {
        high = mid - 1;
    }
}
```

Kyun?

Kyuki `daysneed` ka **final value** tabhi pata chalega jab saare packages process ho jaayenge.

Correct structure:

```cpp
for (int weight : weights) {

    if (curload + weight <= mid) {
        curload += weight;
    }
    else {
        daysneed++;
        curload = weight;
    }
}

// ✅ Saare packages ke baad
if (daysneed <= days) {
    high = mid - 1;
}
else {
    low = mid + 1;
}
```

---

# 🔄 Complete Dry Run

Given:

```text
weights = [1,2,3,4,5,6,7,8,9,10]
days = 5
```

Initial:

```text
low = 10
high = 55
```

---

# Iteration 1

```text
low = 10
high = 55
```

Calculate:

```text
mid = 10 + (55 - 10) / 2
    = 32
```

Current capacity:

```text
32
```

---

## Day 1

Packages:

```text
1,2,3,4,5,6,7
```

Total:

```text
1+2+3+4+5+6+7 = 28
```

Next package:

```text
8
```

Check:

```text
28 + 8 = 36
```

But:

```text
36 > 32
```

So `8` next day jayega.

Day 1:

```text
[1,2,3,4,5,6,7]
```

---

## Day 2

```text
[8,9,10]
```

Total:

```text
8+9+10 = 27
```

So:

```text
daysneed = 2
```

Compare:

```text
2 <= 5
```

Capacity `32` valid hai.

Minimum chahiye:

```cpp
high = mid - 1;
```

Therefore:

```text
high = 31
```

Now:

```text
low = 10
high = 31
```

---

# Iteration 2

```text
low = 10
high = 31
```

Calculate:

```text
mid = 10 + (31 - 10) / 2
    = 20
```

Capacity:

```text
20
```

---

## Day 1

```text
1+2+3+4+5 = 15
```

Next `6`:

```text
15 + 6 = 21 > 20
```

So:

```text
Day 1 → [1,2,3,4,5]
```

---

## Day 2

```text
6+7 = 13
```

Next `8`:

```text
13 + 8 = 21 > 20
```

So:

```text
Day 2 → [6,7]
```

---

## Day 3

```text
8+9 = 17
```

Next `10`:

```text
17 + 10 = 27 > 20
```

So:

```text
Day 3 → [8,9]
```

---

## Day 4

```text
Day 4 → [10]
```

Total:

```text
daysneed = 4
```

Compare:

```text
4 <= 5
```

Capacity `20` valid hai.

Minimum chahiye:

```text
high = 19
```

Now:

```text
low = 10
high = 19
```

---

# Iteration 3

```text
low = 10
high = 19
```

Calculate:

```text
mid = 10 + (19 - 10) / 2
    = 14
```

Capacity:

```text
14
```

---

## Day 1

```text
1+2+3+4 = 10
```

Next `5`:

```text
10 + 5 = 15 > 14
```

So:

```text
Day 1 → [1,2,3,4]
```

---

## Day 2

```text
5+6 = 11
```

Next `7`:

```text
11 + 7 = 18 > 14
```

So:

```text
Day 2 → [5,6]
```

---

## Day 3

```text
Day 3 → [7]
```

Next `8`:

```text
7 + 8 = 15 > 14
```

---

## Day 4

```text
Day 4 → [8]
```

Next `9`:

```text
8 + 9 = 17 > 14
```

---

## Day 5

```text
Day 5 → [9]
```

Next `10`:

```text
9 + 10 = 19 > 14
```

So:

```text
Day 6 → [10]
```

Total:

```text
daysneed = 6
```

Compare:

```text
6 > 5
```

Capacity `14` too small hai.

Therefore:

```cpp
low = mid + 1;
```

So:

```text
low = 15
```

Now:

```text
low = 15
high = 19
```

---

# Iteration 4

```text
low = 15
high = 19
```

Calculate:

```text
mid = 15 + (19 - 15) / 2
    = 17
```

Capacity:

```text
17
```

---

## Day 1

```text
1+2+3+4+5 = 15
```

Next `6`:

```text
15 + 6 = 21 > 17
```

So:

```text
Day 1 → [1,2,3,4,5]
```

---

## Day 2

```text
6+7 = 13
```

Next `8`:

```text
13 + 8 = 21 > 17
```

So:

```text
Day 2 → [6,7]
```

---

## Day 3

```text
8+9 = 17
```

Next `10`:

```text
17 + 10 = 27 > 17
```

So:

```text
Day 3 → [8,9]
```

---

## Day 4

```text
Day 4 → [10]
```

Total:

```text
daysneed = 4
```

Compare:

```text
4 <= 5
```

Valid.

Minimum chahiye:

```text
high = 16
```

Now:

```text
low = 15
high = 16
```

---

# Iteration 5

```text
low = 15
high = 16
```

Calculate:

```text
mid = 15 + (16 - 15) / 2
    = 15
```

Capacity:

```text
15
```

---

## Day 1

```text
1+2+3+4+5 = 15
```

Next `6` fit nahi karega:

```text
15 + 6 = 21 > 15
```

So:

```text
Day 1 → [1,2,3,4,5]
```

---

## Day 2

```text
Day 2 → [6,7]
```

Weight:

```text
6+7 = 13
```

Next `8`:

```text
13 + 8 = 21 > 15
```

---

## Day 3

```text
Day 3 → [8]
```

Next `9`:

```text
8 + 9 = 17 > 15
```

---

## Day 4

```text
Day 4 → [9]
```

Next `10`:

```text
9 + 10 = 19 > 15
```

---

## Day 5

```text
Day 5 → [10]
```

Total:

```text
daysneed = 5
```

Compare:

```text
5 <= 5
```

Capacity `15` valid hai.

Minimum chahiye:

```text
high = 14
```

Now:

```text
low = 15
high = 14
```

---

# 🔚 Binary Search Ends

Loop:

```cpp
while (low <= high)
```

Current:

```text
low = 15
high = 14
```

Check:

```text
15 <= 14
```

False.

Loop end.

Therefore:

```text
answer = low
       = 15
```

---

# 📊 Complete Dry Run Table

| Iteration | Low | High | Mid / Capacity | Days Needed | Valid? | Action      |
| --------- | --: | ---: | -------------: | ----------: | ------ | ----------- |
| 1         |  10 |   55 |             32 |           2 | Yes    | `high = 31` |
| 2         |  10 |   31 |             20 |           4 | Yes    | `high = 19` |
| 3         |  10 |   19 |             14 |           6 | No     | `low = 15`  |
| 4         |  15 |   19 |             17 |           4 | Yes    | `high = 16` |
| 5         |  15 |   16 |             15 |           5 | Yes    | `high = 14` |

Final:

```text
low = 15
high = 14
```

Answer:

```text
15
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    int shipWithinDays(vector<int>& weights, int days) {

        int low = *max_element(weights.begin(), weights.end());
        int high = accumulate(weights.begin(), weights.end(), 0);

        while (low <= high) {

            int mid = low + (high - low) / 2;

            int curload = 0;
            int daysneed = 1;

            for (int weight : weights) {

                if (weight + curload <= mid) {
                    curload += weight;
                }
                else {
                    daysneed++;
                    curload = weight;
                }
            }

            if (daysneed <= days) {
                high = mid - 1;
            }
            else {
                low = mid + 1;
            }
        }

        return low;
    }
};
```

---

# 🧠 Code Line By Line

## 1. Minimum Capacity

```cpp
int low = *max_element(weights.begin(), weights.end());
```

Capacity kisi bhi single package se kam nahi ho sakti.

---

## 2. Maximum Capacity

```cpp
int high = accumulate(weights.begin(), weights.end(), 0);
```

Saare packages ek day mein ship karne ke liye total weight capacity enough hai.

---

## 3. Binary Search

```cpp
while (low <= high)
```

Possible capacities ke range mein search.

---

## 4. Middle Capacity

```cpp
int mid = low + (high - low) / 2;
```

`mid` current possible ship capacity hai.

---

## 5. Current Day Load

```cpp
int curload = 0;
```

Current day mein kitna weight load hai.

---

## 6. Required Days

```cpp
int daysneed = 1;
```

Initially first day already available hai.

---

## 7. Packages Process

```cpp
for (int weight : weights)
```

Packages ko given order mein process karte hain.

---

## 8. Same Day Mein Fit

```cpp
if (weight + curload <= mid)
```

Agar current package capacity ke andar fit ho raha hai:

```cpp
curload += weight;
```

---

## 9. New Day

Agar:

```cpp
weight + curload > mid
```

toh current package current day mein fit nahi hai.

```cpp
daysneed++;
curload = weight;
```

New day start karte hain.

---

# ⚠️ Important: Condition `for` ke BAAD

Ye bahut important hai.

Correct:

```cpp
for (int weight : weights) {
    ...
}

if (daysneed <= days) {
    high = mid - 1;
}
else {
    low = mid + 1;
}
```

### Kyu?

Kyuki:

```text
daysneed
```

ka final value tabhi pata chalega jab **saare packages process ho jaayenge**.

Agar condition `for` ke andar laga di:

```cpp
for (...) {

    ...

    if (daysneed <= days) {
        ...
    }
}
```

toh Binary Search prematurely `low/high` change kar degi.

---

# 🔥 Why `high = mid - 1`?

Suppose:

```text
mid = 15
daysneed = 5
days = 5
```

Capacity `15` valid hai.

Humne `15` ko already check kar liya.

But hume minimum valid capacity chahiye.

Therefore:

```cpp
high = mid - 1;
```

Search:

```text
10 ... 14
```

mein hogi.

---

# 🔥 Why `low = mid + 1`?

Suppose:

```text
mid = 14
daysneed = 6
days = 5
```

Capacity `14` invalid hai.

Hume capacity increase karni padegi.

`14` already check ho chuka hai aur invalid hai.

Therefore:

```cpp
low = mid + 1;
```

Search:

```text
15 ... 
```

se hogi.

---

# 📌 Main Pattern

```text
low = max(weights)
high = sum(weights)
        ↓
mid = possible capacity
        ↓
Calculate days needed
        ↓
daysneed <= days
        ↓
VALID
        ↓
LEFT
        ↓
high = mid - 1
```

Or:

```text
daysneed > days
        ↓
INVALID
        ↓
RIGHT
        ↓
low = mid + 1
```

Finally:

```text
return low;
```

---

# 🧠 Pattern Recognition

Agar question mein:

```text
Find minimum capacity
+
Given days
+
Packages/order fixed
+
Possible capacity ko check kar sakte ho
```

Think:

```text
Binary Search on Answer
```

---

# 🔗 Relation With Previous Question

## 875 — Koko Eating Bananas

```text
Answer = Eating Speed

Check = Hours Needed
```

## 1011 — Capacity To Ship Packages

```text
Answer = Ship Capacity

Check = Days Needed
```

Pattern same:

```text
Possible Answer
      ↓
Feasibility Check
      ↓
Valid → LEFT
Invalid → RIGHT
```

---

# ⏱️ Time Complexity

Binary Search possible capacities par:

```text
O(log(sum(weights)))
```

Har Binary Search iteration mein saare `n` packages process hote hain:

```text
O(n)
```

Therefore:

```text
Time Complexity = O(n log(sum(weights)))
```

---

# 💾 Space Complexity

Extra data structure use nahi ho raha.

Sirf:

```text
low
high
mid
curload
daysneed
```

variables hain.

Therefore:

```text
Space Complexity = O(1)
```

---

# 📊 Complexity

```text
Time  → O(n log(sum(weights)))

Space → O(1)
```

---

# 🧠 Interview Explanation

Agar interviewer pooche:

**"How would you solve Capacity To Ship Packages Within D Days?"**

Bolo:

```text
I use Binary Search on the answer, where the answer is
the ship capacity.

The minimum possible capacity is the maximum package weight,
because every package must fit individually.

The maximum possible capacity is the sum of all package
weights, because then all packages can be shipped in one day.

For each middle capacity, I greedily calculate how many days
are required while preserving the package order.

If adding a package exceeds the current capacity, I start a
new day.

If the required days are less than or equal to the given days,
the capacity is valid, so I search for a smaller capacity.

Otherwise, I increase the capacity.

Finally, low gives the minimum valid capacity.
```

---

# ⭐ Interview Revision Code

```cpp
class Solution {
public:
    int shipWithinDays(vector<int>& weights, int days) {

        int low = *max_element(weights.begin(), weights.end());
        int high = accumulate(weights.begin(), weights.end(), 0);

        while (low <= high) {

            int mid = low + (high - low) / 2;

            int curload = 0;
            int daysneed = 1;

            for (int weight : weights) {

                if (curload + weight <= mid) {
                    curload += weight;
                }
                else {
                    daysneed++;
                    curload = weight;
                }
            }

            if (daysneed <= days) {
                high = mid - 1;
            }
            else {
                low = mid + 1;
            }
        }

        return low;
    }
};
```

---

# 🔄 Quick Revision

```text
1011 - Capacity To Ship Packages Within D Days
                    ↓
        Binary Search on Answer
                    ↓
          Answer = Capacity
                    ↓
        low = max(weights)
        high = sum(weights)
                    ↓
              mid = capacity
                    ↓
         Calculate days needed
                    ↓
       ┌────────────┴────────────┐
       ↓                         ↓
daysneed <= days            daysneed > days
       ↓                         ↓
    VALID                     INVALID
       ↓                         ↓
    LEFT                      RIGHT
       ↓                         ↓
high = mid - 1              low = mid + 1
                    ↓
               return low
```

---

# 🧠 One-Line Revision

```text
Minimum ship capacity ko Binary Search karo → har capacity par greedily days count karo → valid ho to left, invalid ho to right → low answer hai.
```

---

# 🔥 Main Formula / Logic

```text
low = max(weights)
```

```text
high = sum(weights)
```

```text
curload + weight <= mid
→ same day
```

```text
curload + weight > mid
→ new day
→ daysneed++
→ curload = weight
```

```text
daysneed <= days
→ high = mid - 1
```

```text
daysneed > days
→ low = mid + 1
```

```text
answer = low
```

---

# 📌 Pattern

```text
Binary Search
      ↓
Search on Answer
      ↓
Possible Capacity
      ↓
Greedy Feasibility Check
      ↓
Minimum Valid Capacity
      ↓
LeetCode 1011
```

---

# 📚 Binary Search Progress

```text
13 → 69   Sqrt(x)
14 → 367  Valid Perfect Square
15 → 1539 Kth Missing Positive Number
16 → 875  Koko Eating Bananas
17 → 1011 Capacity To Ship Packages Within D Days ✅
```

**17th question complete.**
