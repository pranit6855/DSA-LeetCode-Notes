# LeetCode 1552 — Magnetic Force Between Two Balls

## Problem

Hume ek array `position` diya gaya hai jisme different positions di hui hain.

Hume exactly `m` balls place karni hain.

Goal:

> Balls ke beech ki **minimum distance ko maximum** karna hai.

Matlab aise `m` positions choose karni hain jisse selected balls ke beech ki minimum distance sabse zyada ho.

---

## Example

```text
position = [1, 2, 3, 4, 7]
m = 3
```

Agar balls ko:

```text
1, 2, 7
```

par rakhein:

```text
2 - 1 = 1
7 - 2 = 5
```

Minimum distance:

```text
1
```

Lekin agar balls ko:

```text
1, 4, 7
```

par rakhein:

```text
4 - 1 = 3
7 - 4 = 3
```

Minimum distance:

```text
3
```

Isse better minimum distance possible nahi hai.

Therefore:

```text
Answer = 3
```

---

# Main Idea

Question directly ye nahi pooch raha ki balls ko exactly kahan rakhna hai.

Hume find karna hai:

> **Maximum possible minimum distance kya ho sakti hai?**

Ye optimization problem hai.

Isliye yahan:

```text
Binary Search on Answer
```

use karenge.

Yahan:

```text
Answer = minimum distance between placed balls
```

---

# Step 1 — Positions Sort Karna

Sabse pehle positions ko sort karenge:

```cpp
sort(position.begin(), position.end());
```

Example:

```text
Before:
[4, 7, 1, 2, 3]

After:
[1, 2, 3, 4, 7]
```

Sorting isliye zaroori hai kyunki hume positions ko left se right check karna hai.

---

# Step 2 — Answer Range

Minimum possible distance:

```text
1
```

Maximum possible distance:

```text
maximum position - minimum position
```

Example:

```text
position = [1, 2, 3, 4, 7]
```

To:

```text
low = 1
high = 7 - 1
     = 6
```

Ab binary search distance `1` se `6` ke beech chalegi.

---

# Step 3 — Mid Ka Meaning

Maan lo:

```text
low = 1
high = 6
```

Then:

```text
mid = 3
```

Hum ye assume nahi kar rahe ki answer `3` hai.

Hum sirf check kar rahe hain:

> Kya 3 balls ko aise place kar sakte hain ki har placed ball ke beech minimum 3 distance ho?

Agar haan:

```text
3 is possible
```

Agar nahi:

```text
3 is not possible
```

---

# Step 4 — Greedy Feasibility Check

Iske liye helper function:

```cpp
bool canplace(vector<int>& position, int m, int dist)
```

Ye check karega:

> Given minimum distance `dist` ke saath kya `m` balls place kar sakte hain?

---

# Greedy Rule

Har next ball ko:

> **Sabse earliest possible position par place karo.**

Example:

```text
position = [1, 2, 3, 4, 7]
dist = 3
```

First ball:

```text
1
```

par.

Next ball ke liye minimum required position:

```text
1 + 3 = 4
```

Positions check:

```text
2 → nahi
3 → nahi
4 → yes
```

So second ball:

```text
4
```

par.

Next ball ke liye:

```text
4 + 3 = 7
```

Position `7` available hai.

So third ball:

```text
7
```

par.

Final:

```text
1, 4, 7
```

Total:

```text
3 balls
```

Therefore:

```text
dist = 3 is possible
```

---

# Why Greedy Works?

Maan lo previous ball `1` par hai aur minimum distance `3` chahiye.

Next ball ke liye minimum valid position:

```text
1 + 3 = 4
```

Agar `4` available hai, to `4` par ball rakhna best hai.

Agar hum usko `5` ya `6` par rakh denge, to future balls ke liye space kam ho jayega.

Isliye:

> **Har next ball ko earliest possible position par rakhne se future ke liye maximum space bachti hai.**

---

# `canplace()` Function

```cpp
bool canplace(vector<int>& position, int m, int dist) {

    int balls = 1;
    int lastposition = position[0];

    for (int i = 1; i < position.size(); i++) {

        if (position[i] - lastposition >= dist) {
            balls++;
            lastposition = position[i];
        }

        if (balls >= m) {
            return true;
        }
    }

    return false;
}
```

---

# `canplace()` Line-by-Line

## First Ball

```cpp
int balls = 1;
```

First ball ko first position par place kar diya.

```cpp
int lastposition = position[0];
```

`lastposition` mein last placed ball ki position store karenge.

Example:

```text
position = [1, 2, 3, 4, 7]
```

Initially:

```text
balls = 1
lastposition = 1
```

---

## Loop

```cpp
for (int i = 1; i < position.size(); i++)
```

Ab first position ke baad baaki positions check karenge.

---

## Distance Check

```cpp
if (position[i] - lastposition >= dist)
```

Agar current position aur last placed ball ke beech distance kam se kam `dist` hai, tab ball place kar sakte hain.

Example:

```text
lastposition = 1
current position = 4
dist = 3
```

Then:

```text
4 - 1 = 3
```

Since:

```text
3 >= 3
```

ball place kar sakte hain.

---

## Ball Place Karna

```cpp
balls++;
lastposition = position[i];
```

Ball count increase karo.

Aur `lastposition` ko current position bana do.

---

## Required Balls Mil Gayi

```cpp
if (balls >= m) {
    return true;
}
```

Agar required `m` balls place ho gayi hain, to current distance possible hai.

---

# Detailed Dry Run — `dist = 3`

Input:

```text
position = [1, 2, 3, 4, 7]
m = 3
dist = 3
```

Initial:

```text
balls = 1
lastposition = 1
```

### Position = 2

```text
2 - 1 = 1
```

Required:

```text
>= 3
```

No.

---

### Position = 3

```text
3 - 1 = 2
```

Still:

```text
2 < 3
```

No.

---

### Position = 4

```text
4 - 1 = 3
```

Valid.

```text
balls = 2
lastposition = 4
```

---

### Position = 7

```text
7 - 4 = 3
```

Valid.

```text
balls = 3
lastposition = 7
```

Required:

```text
m = 3
```

mil gaya.

Therefore:

```text
canplace(3) = true
```

---

# What If `dist = 4`?

First ball:

```text
1
```

Next ball ke liye:

```text
1 + 4 = 5
```

Positions:

```text
2 → no
3 → no
4 → no
7 → yes
```

Second ball:

```text
7
```

Ab third ball ke liye:

```text
7 + 4 = 11
```

Lekin position `11` available nahi hai.

Only:

```text
1, 7
```

2 balls place hui.

Required:

```text
3
```

Therefore:

```text
canplace(4) = false
```

---

# Binary Search

Initial:

```text
low = 1
high = 6
ans = 0
```

## Iteration 1

```text
mid = 1 + (6 - 1) / 2
    = 3
```

`canplace(3)`:

```text
true
```

To:

```cpp
ans = 3;
low = mid + 1;
```

Now:

```text
low = 4
high = 6
```

Hume maximum distance chahiye, isliye right side search kar rahe hain.

---

## Iteration 2

```text
mid = 4 + (6 - 4) / 2
    = 5
```

`canplace(5)`:

```text
false
```

To:

```cpp
high = mid - 1;
```

Now:

```text
low = 4
high = 4
```

---

## Iteration 3

```text
mid = 4
```

`canplace(4)`:

```text
false
```

So:

```cpp
high = 3;
```

Now:

```text
low = 4
high = 3
```

Loop end.

Answer:

```text
ans = 3
```

---

# Binary Search Logic

Yahan hume:

```text
Maximum valid answer
```

find karna hai.

Pattern:

```text
Possible Possible Possible Impossible Impossible
                      ↑
               Maximum Valid
```

Isliye:

### Possible

```cpp
ans = mid;
low = mid + 1;
```

Matlab current answer valid hai, lekin aur bada distance try karo.

### Impossible

```cpp
high = mid - 1;
```

Matlab distance bahut bada hai, chhota distance try karo.

---

# Complete Code

```cpp
class Solution {
public:

    bool canplace(vector<int>& position, int m, int dist) {

        int balls = 1;
        int lastposition = position[0];

        for (int i = 1; i < position.size(); i++) {

            if (position[i] - lastposition >= dist) {
                balls++;
                lastposition = position[i];
            }

            if (balls >= m) {
                return true;
            }
        }

        return false;
    }

    int maxDistance(vector<int>& position, int m) {

        sort(position.begin(), position.end());

        int low = 1;

        int high = *max_element(
            position.begin(),
            position.end()
        ) - *min_element(
            position.begin(),
            position.end()
        );

        int ans = 0;

        while (low <= high) {

            int mid = low + (high - low) / 2;

            if (canplace(position, m, mid)) {
                ans = mid;
                low = mid + 1;
            }
            else {
                high = mid - 1;
            }
        }

        return ans;
    }
};
```

---

# Why Binary Search Works?

Feasibility monotonic hai.

Agar distance `3` possible hai, to smaller distances:

```text
1
2
3
```

also possible hongi.

Agar distance `4` impossible hai, to larger distances:

```text
5
6
...
```

bhi impossible hongi.

Pattern:

```text
Distance:  1  2  3  4  5  6
Possible:   Y  Y  Y  N  N  N
```

Hume last `Y` find karna hai.

---

# Important Difference: 1482 vs 1552

## 1482 — Minimum Number of Days

Hume:

```text
Minimum valid answer
```

chahiye.

Pattern:

```text
N N N Y Y Y
      ↑
 First Valid
```

Possible hone par left:

```cpp
high = mid;
```

---

## 1552 — Magnetic Force

Hume:

```text
Maximum valid answer
```

chahiye.

Pattern:

```text
Y Y Y N N N
    ↑
 Last Valid
```

Possible hone par right:

```cpp
low = mid + 1;
```

---

# Complexity

Let:

```text
n = position.size()
```

Sorting:

```text
O(n log n)
```

Har feasibility check:

```text
O(n)
```

Binary search:

```text
O(log(maxPosition - minPosition))
```

Total:

```text
O(n log n + n log(maxPosition - minPosition))
```

Extra Space:

```text
O(1)
```

Sorting ki internal implementation space ko ignore karke.

---

# Common Mistakes

## 1. Sorting Bhoolna

Always:

```cpp
sort(position.begin(), position.end());
```

---

## 2. Possible Hone Par Left Jana

1552 mein possible hone par:

```cpp
low = mid + 1;
```

because hume **maximum** distance chahiye.

---

## 3. `high = mid` Karna

Ye minimum-valid-answer problems mein hota hai.

Yahan:

```cpp
high = mid - 1;
```

use hoga jab `mid` impossible ho.

---

## 4. Balls Randomly Place Karna

Har next ball ko earliest possible valid position par place karo.

Ye greedy strategy hai.

---

# Pattern Recognition

Agar question mein aaye:

> **Maximize the minimum distance**

to immediately think:

```text
Binary Search on Answer
+
Greedy Feasibility Check
```

Typical steps:

```text
1. Sort
2. Answer range define karo
3. mid distance assume karo
4. Greedy se check karo
5. Possible → right
6. Impossible → left
7. Maximum valid answer
```

---

# Interview Explanation

Agar interviewer puche:

> Explain your approach.

Answer:

> Main pehle positions ko sort karta hoon. Kyunki hume minimum distance ko maximum karna hai, main possible distance par binary search karta hoon. Har `mid` distance ke liye greedy check karta hoon ki kya `m` balls place ki ja sakti hain. First ball ko first position par rakhta hoon aur har next ball ko earliest possible position par rakhta hoon jahan previous ball se kam se kam `mid` distance ho. Agar `m` balls place ho jaati hain, to `mid` possible hai aur main larger distance search karta hoon. Agar possible nahi hai, to smaller distance search karta hoon. Is tarah maximum possible minimum distance mil jaati hai.

---

# Key Takeaways

### Pattern

```text
Binary Search on Answer
```

### Answer

```text
Maximum possible minimum distance
```

### Feasibility

```text
Greedy
```

### Greedy Rule

```text
Har next ball ko earliest possible valid position par place karo.
```

### Binary Search

```text
Possible:
    ans = mid
    low = mid + 1

Impossible:
    high = mid - 1
```

### Complexity

```text
Time  : O(n log n + n log range)
Space : O(1) extra
```

---

# One-Line Revision

> **"1552 mein minimum distance ko maximum karna hai, isliye distance par binary search lagao aur har distance ko greedy se check karo ki `m` balls place ho rahi hain ya nahi."**

---

## LeetCode

**Problem:** 1552 — Magnetic Force Between Two Balls

**Difficulty:** Medium

**Pattern:** Binary Search on Answer + Greedy

**Question Number in Our Binary Search Series:** #19
