# LeetCode 1482 — Minimum Number of Days to Make m Bouquets

## Problem

Hume ek array `bloomDay` diya gaya hai.

```cpp
bloomDay[i]
```

ka matlab hai ki `i-th` flower kis din bloom hoga.

Hume total `m` bouquets banane hain.

Har bouquet banane ke liye exactly `k` **adjacent flowers** chahiye.

Hume return karna hai:

> Minimum kitne days wait karne padenge taaki `m` bouquets ban sake.

Agar `m` bouquets banana possible hi nahi hai, to `-1` return karo.

---

## Example

```text
bloomDay = [1, 10, 3, 10, 2]
m = 3
k = 1
```

Hume:

```text
3 bouquets
```

banane hain aur har bouquet ke liye:

```text
1 flower
```

chahiye.

Day `3` tak:

```text
[1, 10, 3, 10, 2]
```

mein bloom ho chuke flowers:

```text
1 <= 3  → bloom
10 > 3  → nahi bloom
3 <= 3  → bloom
10 > 3  → nahi bloom
2 <= 3  → bloom
```

Total:

```text
3 bloomed flowers
```

Aur `k = 1`, isliye har flower ek bouquet bana sakta hai.

Answer:

```text
3
```

---

# Sabse Important Observation

Normal way mein hum soch sakte hain:

> Day 1 check karo, day 2 check karo, day 3 check karo...

Lekin agar bloom days bahut bade hain, to ye slow ho jayega.

Example:

```text
bloomDay = [1, 1000000000, 500000000]
```

Hume 1 se `1000000000` tak har day check karne ki zarurat nahi hai.

Yahan **Binary Search on Answer** use karenge.

---

# Binary Search on Answer

Yahan hum array ke andar search nahi kar rahe.

Hum search kar rahe hain:

```text
Minimum number of days
```

Possible answer ka range:

```text
minimum bloom day → maximum bloom day
```

Example:

```text
bloomDay = [1, 10, 3, 10, 2]
```

To:

```text
low = 1
high = 10
```

Ab binary search karenge.

---

# Feasibility Check

Sabse important function hoga:

```cpp
canMake(day)
```

Ye check karega:

> Kya `day` tak hum `m` bouquets bana sakte hain?

Agar haan:

```text
true
```

Agar nahi:

```text
false
```

---

# Monotonic Property

Ye problem binary search ke liye perfect hai because iska answer monotonic hai.

Maan lo day `5` par hum bouquets bana sakte hain.

To day:

```text
6
7
8
9
10
...
```

par bhi bana sakte hain.

Kyun?

Kyuki flower bloom hone ke baad unbloom nahi hota.

Isliye result kuch aisa hoga:

```text
Day:       1  2  3  4  5  6  7  8
Possible:  F  F  T  T  T  T  T  T
```

Hume first `true` find karna hai.

```text
             ↓
F F F F T T T T
        ^
     Answer
```

---

# Bouquet Kaise Count Karenge?

Maan lo:

```text
bloomDay = [1, 2, 3, 10, 2]
```

Aur:

```text
day = 3
```

To day 3 tak:

```text
1 <= 3 → bloom
2 <= 3 → bloom
3 <= 3 → bloom
10 > 3 → nahi bloom
2 <= 3 → bloom
```

Pattern:

```text
[B B B N B]
```

Agar:

```text
k = 2
```

to first do adjacent flowers:

```text
[B B]
```

ek bouquet bana denge.

Remaining:

```text
[B N B]
```

`N` aane ke baad consecutive count reset ho jayega.

---

# Consecutive Flowers Count

Hum do variables lenge:

```cpp
int bouquets = 0;
int flowers = 0;
```

### `flowers`

Ye batayega ki currently kitne **continuous bloomed flowers** mile hain.

Agar:

```cpp
bloomDay[i] <= day
```

to flower bloom ho chuka hai:

```cpp
flowers++;
```

Agar:

```cpp
flowers == k
```

to ek bouquet ban gaya:

```cpp
bouquets++;
flowers = 0;
```

---

# `flowers = 0` Kyun?

Maan lo:

```text
k = 3
```

aur hume:

```text
[B B B]
```

mil gaya.

Ye 3 flowers ek bouquet mein use ho gaye.

Isliye next bouquet ke liye fresh count start karna hai:

```cpp
flowers = 0;
```

---

# Unbloomed Flower Milne Par

Agar:

```cpp
bloomDay[i] > day
```

to flower abhi bloom nahi hua.

Iska matlab consecutive sequence toot gaya.

Example:

```text
[B B N B B]
```

`N` ke baad pehle wale flowers ko continue nahi kar sakte.

Isliye:

```cpp
flowers = 0;
```

---

# Complete `canMake()` Function

```cpp
bool canMake(vector<int>& bloomDay, int day, int m, int k) {

    int bouquets = 0;
    int flowers = 0;

    for (int i = 0; i < bloomDay.size(); i++) {

        if (bloomDay[i] <= day) {
            flowers++;

            if (flowers == k) {
                bouquets++;
                flowers = 0;
            }
        }
        else {
            flowers = 0;
        }
    }

    return bouquets >= m;
}
```

---

# Full Code

```cpp
class Solution {
public:

    bool canMake(vector<int>& bloomDay, int day, int m, int k) {

        int bouquets = 0;
        int flowers = 0;

        for (int i = 0; i < bloomDay.size(); i++) {

            if (bloomDay[i] <= day) {
                flowers++;

                if (flowers == k) {
                    bouquets++;
                    flowers = 0;
                }
            }
            else {
                flowers = 0;
            }
        }

        return bouquets >= m;
    }

    int minDays(vector<int>& bloomDay, int m, int k) {

        long long required = 1LL * m * k;

        if (required > bloomDay.size()) {
            return -1;
        }

        int low = *min_element(
            bloomDay.begin(),
            bloomDay.end()
        );

        int high = *max_element(
            bloomDay.begin(),
            bloomDay.end()
        );

        while (low < high) {

            int mid = low + (high - low) / 2;

            if (canMake(bloomDay, mid, m, k)) {
                high = mid;
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

# Code Ko Step-by-Step Samjho

## Step 1 — Impossible Case

```cpp
long long required = 1LL * m * k;

if (required > bloomDay.size()) {
    return -1;
}
```

Ek bouquet ke liye:

```text
k flowers
```

chahiye.

`m` bouquets ke liye:

```text
m × k flowers
```

chahiye.

Example:

```text
m = 3
k = 2
```

Required:

```text
3 × 2 = 6 flowers
```

Agar array mein sirf 5 flowers hain:

```text
5 < 6
```

to impossible hai.

Return:

```text
-1
```

---

# `1LL` Kyun Use Kiya?

Hum likh sakte hain:

```cpp
m * k
```

Lekin large values mein `int` overflow ho sakta hai.

Isliye:

```cpp
1LL * m * k
```

likhte hain.

`1LL` calculation ko `long long` bana deta hai.

---

# Step 2 — Binary Search Range

```cpp
int low = *min_element(
    bloomDay.begin(),
    bloomDay.end()
);
```

Sabse jaldi possible answer:

```text
minimum bloom day
```

Hai.

Aur:

```cpp
int high = *max_element(
    bloomDay.begin(),
    bloomDay.end()
);
```

Sabse late possible answer:

```text
maximum bloom day
```

Hai.

Example:

```text
[1, 10, 3, 10, 2]
```

To:

```text
low = 1
high = 10
```

---

# Step 3 — Mid

```cpp
int mid = low + (high - low) / 2;
```

Example:

```text
low = 1
high = 10

mid = 1 + (10 - 1) / 2
    = 5
```

Ab hum check karenge:

> Kya day 5 tak `m` bouquets ban sakte hain?

---

# Step 4 — Agar Possible Hai

```cpp
if (canMake(bloomDay, mid, m, k)) {
    high = mid;
}
```

Agar `mid` valid hai, to answer `mid` ho sakta hai.

Lekin hume **minimum** answer chahiye.

Isliye left side search karenge:

```text
high = mid
```

---

# Step 5 — Agar Possible Nahi Hai

```cpp
else {
    low = mid + 1;
}
```

Agar `mid` days enough nahi hain, to hume aur days chahiye.

Isliye right side:

```text
low = mid + 1
```

---

# Detailed Dry Run

Input:

```text
bloomDay = [1, 10, 3, 10, 2]
m = 3
k = 1
```

Required flowers:

```text
3 × 1 = 3
```

Array mein 5 flowers hain, so possible hai.

Initial:

```text
low = 1
high = 10
```

---

## Iteration 1

```text
low = 1
high = 10

mid = 5
```

Check day 5:

```text
1  → bloom
10 → no
3  → bloom
10 → no
2  → bloom
```

Pattern:

```text
B N B N B
```

`k = 1`, so:

```text
3 bouquets
```

Required:

```text
3
```

Possible.

Therefore:

```text
high = 5
```

Now:

```text
low = 1
high = 5
```

---

## Iteration 2

```text
mid = 1 + (5 - 1) / 2
    = 3
```

Day 3:

```text
1  → bloom
10 → no
3  → bloom
10 → no
2  → bloom
```

Again:

```text
3 bouquets
```

Possible.

So:

```text
high = 3
```

---

## Iteration 3

Now:

```text
low = 1
high = 3
```

Mid:

```text
mid = 2
```

Day 2:

```text
1  → bloom
10 → no
3  → no
10 → no
2  → bloom
```

Pattern:

```text
B N N N B
```

Only:

```text
2 bouquets
```

ban sakte hain.

Required:

```text
3
```

So impossible.

Therefore:

```text
low = mid + 1
low = 3
```

Now:

```text
low = 3
high = 3
```

Loop stop.

Answer:

```text
3
```

---

# Why `while (low < high)`?

Hum minimum valid answer find kar rahe hain.

Jab:

```text
low == high
```

to sirf ek value bachi hai.

Wahi answer hai.

Isliye:

```cpp
while (low < high)
```

use karte hain.

---

# Important Binary Search Logic

Is problem mein ye logic yaad rakho:

```text
          mid
           ↓
F F F F F T T T T
          ↑
       minimum
```

Agar `mid` possible hai:

```cpp
high = mid;
```

Agar `mid` possible nahi hai:

```cpp
low = mid + 1;
```

End mein:

```cpp
return low;
```

---

# Complexity

Let:

```text
n = bloomDay.size()
```

Har `canMake()` call mein poora array traverse hota hai:

```text
O(n)
```

Binary search approximately:

```text
O(log(maxDay - minDay))
```

times chalega.

Therefore:

```text
Time Complexity:
O(n log(maxDay - minDay))
```

Space:

```text
O(1)
```

---

# Interview Mein Kaise Explain Karna Hai

Agar interviewer puche:

> How did you solve this?

Simple answer:

> Is problem mein hume minimum days find karne hain. Maine binary search on answer use kiya. Search range minimum bloom day se maximum bloom day tak hai. Har `mid` day ke liye main check karta hoon ki us day tak kitne bouquets ban sakte hain. Iske liye consecutive bloomed flowers count karta hoon. Jab consecutive flowers `k` ho jaate hain, ek bouquet bana deta hoon. Agar `m` bouquets ban jaate hain, current day possible hai aur main left side search karta hoon. Agar bouquets nahi bante, to right side search karta hoon. Kyunki feasibility monotonic hai, binary search efficiently minimum valid day find kar sakti hai.

---

# Common Mistakes

## 1. Adjacent Flowers Bhoolna

Galat:

```text
Koi bhi k bloomed flowers le lo
```

Sahi:

```text
k consecutive / adjacent flowers hone chahiye
```

---

## 2. Unbloomed Flower Par Count Reset Na Karna

Agar:

```cpp
bloomDay[i] > day
```

to:

```cpp
flowers = 0;
```

karna zaroori hai.

---

## 3. `m * k` Overflow

Large values ke liye:

```cpp
1LL * m * k
```

use karo.

---

## 4. Wrong Binary Search Direction

Agar `mid` possible hai:

```cpp
high = mid;
```

Agar `mid` possible nahi hai:

```cpp
low = mid + 1;
```

---

# Pattern Recognition

Ye question ek important pattern sikhata hai:

## Binary Search on Answer

Agar question mein kuch aisa ho:

```text
Minimum possible...
Maximum possible...
Minimum days...
Minimum capacity...
Minimum speed...
Minimum time...
```

aur tum ek function bana sakte ho:

```cpp
isPossible(mid)
```

to Binary Search on Answer ka possibility check karo.

General pattern:

```cpp
int low = minimumAnswer;
int high = maximumAnswer;

while (low < high) {

    int mid = low + (high - low) / 2;

    if (isPossible(mid)) {
        high = mid;
    }
    else {
        low = mid + 1;
    }
}

return low;
```

---

# Final Takeaway

LeetCode 1482 ka main idea:

```text
Array par Binary Search nahi karni.
             ↓
Answer par Binary Search karni hai.
             ↓
Candidate day = mid
             ↓
Check karo bouquets ban sakte hain ya nahi.
             ↓
Possible → left
Not possible → right
             ↓
Minimum valid day
```

### Yaad rakhne wali line:

> **"Minimum days find karne hain + kisi day par possible check kar sakte hain = Binary Search on Answer."**

### Complexity

```text
Time  : O(n log(maxDay - minDay))
Space : O(1)
```

### LeetCode

**1482 — Minimum Number of Days to Make m Bouquets**

**Pattern:** Binary Search on Answer

**Difficulty:** Medium
