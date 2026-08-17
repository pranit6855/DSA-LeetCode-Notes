# LeetCode 875 - Koko Eating Bananas

## 📌 Problem

Koko ke paas bananas ke multiple piles hain.

Hume ek integer array `piles` diya hai:

```text
piles = [3,6,7,11]
```

Aur Koko ke paas total:

```text
h = 8
```

hours hain.

Koko ek fixed speed `k` bananas per hour se khati hai.

Hume **minimum eating speed `k`** find karni hai jisse Koko saare bananas `h` hours ke andar finish kar sake.

---

# 🔹 Important Conditions

```text
1. Koko ki eating speed fixed hoti hai.
2. Har hour Koko sirf ek pile se bananas kha sakti hai.
3. Ek pile finish hone ke baad next pile par ja sakti hai.
4. Ek hour mein maximum k bananas kha sakti hai.
5. Agar pile mein k se kam bananas hain, toh woh pile usi hour mein finish ho jayega.
6. Hume minimum possible eating speed find karni hai.
```

---

# 🔹 Example

```text
piles = [3,6,7,11]
h = 8
```

Agar:

```text
k = 4
```

Then:

```text
Pile 3  → 1 hour
Pile 6  → 2 hours
Pile 7  → 2 hours
Pile 11 → 3 hours
```

Total:

```text
1 + 2 + 2 + 3 = 8 hours
```

Koko ke paas:

```text
8 hours
```

hain.

So:

```text
Output = 4
```

---

# 🧠 Main Observation

Yahan hume array ke andar koi element search nahi karna.

Hume **answer ki value** search karni hai.

Answer hai:

```text
Eating Speed = k
```

Possible speeds:

```text
1,2,3,4,5,6,...,11
```

In possible speeds par Binary Search laga sakte hain.

Is pattern ko:

```text
Binary Search on Answer
```

kehte hain.

---

# 🔥 Search Range

Minimum possible speed:

```cpp
int low = 1;
```

Koko minimum `1` banana/hour kha sakti hai.

Maximum possible speed:

```cpp
int high = *max_element(piles.begin(), piles.end());
```

Example:

```text
piles = [3,6,7,11]
```

Maximum pile:

```text
11
```

So:

```text
low = 1
high = 11
```

---

# ❓ Why `high = max(piles)`?

Suppose:

```text
piles = [3,6,7,11]
```

Agar Koko:

```text
k = 11
```

speed se khaye:

```text
3  → 1 hour
6  → 1 hour
7  → 1 hour
11 → 1 hour
```

Kisi bhi pile ko 1 hour se zyada nahi lagega.

Isliye maximum required speed:

```text
max(piles)
```

se zyada kabhi nahi hogi.

---

# 🔥 Binary Search

Har iteration mein:

```cpp
int mid = low + (high - low) / 2;
```

`mid` ko hum Koko ki **possible eating speed** maanenge.

Phir check karenge:

```text
Agar Koko speed = mid se khaye,
toh total kitne hours lagenge?
```

---

# 🔹 Hours Calculate Karna

Har pile ke liye:

```text
hours = ceil(pile / mid)
```

Example:

```text
pile = 7
mid = 4
```

Then:

```text
7 / 4 = 1.75
```

Koko ko complete pile finish karne ke liye:

```text
2 hours
```

lagenge.

So:

```text
ceil(7/4) = 2
```

---

# 💻 C++ mein Ceiling

Direct mathematical form:

```cpp
hours += ceil((double)pile / mid);
```

Ye beginner-friendly hai.

Example:

```text
pile = 7
mid = 4

ceil(7.0 / 4)
= ceil(1.75)
= 2
```

---

# 🔹 Integer Ceiling Formula

C++ mein integer-only version bhi use kar sakte hain:

```cpp
hours += (pile + mid - 1) / mid;
```

Example:

```text
pile = 7
mid = 4

(7 + 4 - 1) / 4
= 10 / 4
= 2
```

Ye basically:

```text
ceil(pile / mid)
```

hi hai.

---

# 🔥 Main Binary Search Logic

Total hours calculate karne ke baad:

```cpp
if (hours <= h)
```

check karenge.

---

# 🔥 Case 1 — Speed Valid

```cpp
if (hours <= h)
```

Agar required hours available hours se kam ya equal hain:

```text
hours <= h
```

Matlab current speed valid hai.

Example:

```text
hours = 8
h = 8

8 <= 8
```

Speed valid hai.

Lekin hume **minimum speed** chahiye.

Isliye aur chhoti speed search karenge:

```cpp
high = mid - 1;
```

Meaning:

```text
LEFT
```

---

# 🔥 Case 2 — Speed Too Slow

Agar:

```text
hours > h
```

Matlab current speed par Koko time ke andar finish nahi kar sakti.

Example:

```text
hours = 10
h = 8
```

So:

```text
10 > 8
```

Current speed too slow hai.

Speed badhani padegi:

```cpp
low = mid + 1;
```

Meaning:

```text
RIGHT
```

---

# 📌 Main Logic

```text
hours <= h
    ↓
Speed valid
    ↓
Minimum chahiye
    ↓
LEFT
    ↓
high = mid - 1
```

```text
hours > h
    ↓
Speed too slow
    ↓
Speed badhao
    ↓
RIGHT
    ↓
low = mid + 1
```

---

# 🔄 Complete Dry Run

Given:

```text
piles = [3,6,7,11]
h = 8
```

Initial:

```text
low = 1
high = 11
```

---

## Iteration 1

```text
low = 1
high = 11
```

Calculate:

```text
mid = 1 + (11 - 1) / 2
    = 6
```

So current speed:

```text
k = 6
```

### Calculate Hours

```text
Pile 3:
ceil(3/6) = 1

Pile 6:
ceil(6/6) = 1

Pile 7:
ceil(7/6) = 2

Pile 11:
ceil(11/6) = 2
```

Total:

```text
hours = 1 + 1 + 2 + 2
      = 6
```

Compare:

```text
6 <= 8
```

Valid speed.

But minimum speed chahiye.

So:

```cpp
high = mid - 1;
```

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

# Iteration 2

```text
low = 1
high = 5
```

Calculate:

```text
mid = 1 + (5 - 1) / 2
    = 3
```

Current speed:

```text
k = 3
```

### Calculate Hours

```text
Pile 3:
ceil(3/3) = 1

Pile 6:
ceil(6/3) = 2

Pile 7:
ceil(7/3) = 3

Pile 11:
ceil(11/3) = 4
```

Total:

```text
hours = 1 + 2 + 3 + 4
      = 10
```

Compare:

```text
10 > 8
```

Invalid.

Speed too slow hai.

So:

```cpp
low = mid + 1;
```

Therefore:

```text
low = 4
```

Now:

```text
low = 4
high = 5
```

---

# Iteration 3

```text
low = 4
high = 5
```

Calculate:

```text
mid = 4 + (5 - 4) / 2
    = 4
```

Current speed:

```text
k = 4
```

### Calculate Hours

```text
Pile 3:
ceil(3/4) = 1

Pile 6:
ceil(6/4) = 2

Pile 7:
ceil(7/4) = 2

Pile 11:
ceil(11/4) = 3
```

Total:

```text
hours = 1 + 2 + 2 + 3
      = 8
```

Compare:

```text
8 <= 8
```

Valid.

But minimum chahiye.

So:

```cpp
high = mid - 1;
```

Therefore:

```text
high = 3
```

Now:

```text
low = 4
high = 3
```

---

# 🔚 Loop Stop

Loop condition:

```cpp
while(low <= high)
```

Currently:

```text
low = 4
high = 3
```

Check:

```text
4 <= 3
```

False.

Binary Search stop.

---

# ✅ Final Answer

```text
low = 4
```

So:

```cpp
return low;
```

returns:

```text
4
```

Therefore:

```text
Output = 4
```

---

# 📊 Complete Dry Run Table

| Iteration | Low | High | Mid / Speed | Hours Needed | Valid? | Action     |
| --------- | --: | ---: | ----------: | -----------: | ------ | ---------- |
| 1         |   1 |   11 |           6 |            6 | Yes    | `high = 5` |
| 2         |   1 |    5 |           3 |           10 | No     | `low = 4`  |
| 3         |   4 |    5 |           4 |            8 | Yes    | `high = 3` |

Final:

```text
low = 4
high = 3
```

Answer:

```text
4
```

---

# 🧠 Why `high = mid - 1`?

Ye important Binary Search concept hai.

Suppose:

```text
mid = 4
hours = 8
h = 8
```

Humne speed `4` ko actually check kar liya:

```text
8 <= 8
```

So hume pata hai:

```text
4 is VALID
```

Ab minimum valid speed chahiye.

Isliye hum `4` ko dobara check nahi karte.

```cpp
high = mid - 1;
```

Search range:

```text
1 2 3
```

ho jati hai.

Agar `high = mid` karte, toh `4` search range mein bana rehta.

Is `while(low <= high)` template mein hum already checked `mid` ko remove karte hain.

---

# 🔥 Why `low = mid + 1`?

Suppose:

```text
mid = 3
hours = 10
h = 8
```

So:

```text
10 > 8
```

Speed `3` invalid hai.

Hume pata hai:

```text
3 definitely answer nahi hai
```

Isliye `3` ko bhi remove kar sakte hain:

```cpp
low = mid + 1;
```

Search:

```text
4 5 6...
```

se hogi.

---

# 💻 C++ Code

```cpp
class Solution {
public:
    int minEatingSpeed(vector<int>& piles, int h) {

        int low = 1;
        int high = *max_element(piles.begin(), piles.end());

        while (low <= high) {

            int mid = low + (high - low) / 2;

            long long hours = 0;

            for (int pile : piles) {
                hours += ceil((double)pile / mid);
            }

            if (hours <= h) {
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

## Step 1 — `low`

```cpp
int low = 1;
```

Minimum eating speed `1` banana/hour ho sakti hai.

---

## Step 2 — `high`

```cpp
int high = *max_element(piles.begin(), piles.end());
```

Maximum possible useful speed maximum pile ke equal hoti hai.

---

## Step 3 — Binary Search

```cpp
while (low <= high)
```

Possible speeds ke range mein Binary Search.

---

## Step 4 — `mid`

```cpp
int mid = low + (high - low) / 2;
```

`mid` current possible eating speed hai.

---

## Step 5 — Hours

```cpp
long long hours = 0;
```

Total required hours store karenge.

---

## Step 6 — Every Pile

```cpp
for (int pile : piles)
```

Har pile ko individually process karenge.

---

## Step 7 — Ceiling

```cpp
hours += ceil((double)pile / mid);
```

Current speed `mid` par pile finish karne mein kitne hours lagenge.

---

## Step 8 — Valid Speed

```cpp
if (hours <= h)
```

Agar time limit ke andar finish ho raha hai:

```cpp
high = mid - 1;
```

Aur smaller speed search karo.

---

## Step 9 — Invalid Speed

```cpp
else
```

Agar time limit exceed ho raha hai:

```cpp
low = mid + 1;
```

Speed increase karo.

---

## Step 10 — Answer

```cpp
return low;
```

Binary Search ke end mein `low` **minimum valid speed** hoti hai.

---

# ⚠️ Common Mistakes

## 1. `low = 0`

Wrong:

```cpp
int low = 0;
```

Speed `0` possible nahi hai.

Correct:

```cpp
int low = 1;
```

---

## 2. `high = piles.size() - 1`

Wrong:

```cpp
high = piles.size() - 1;
```

Hum array index par Binary Search nahi kar rahe.

Hum **speed** par Binary Search kar rahe hain.

Correct:

```cpp
high = max(piles)
```

---

## 3. `hours < h`

Wrong:

```cpp
if (hours < h)
```

Agar exactly `h` hours lag rahe hain, speed valid hai.

Example:

```text
hours = 8
h = 8
```

Valid.

So:

```cpp
if (hours <= h)
```

---

## 4. `high = mid`

Is template mein:

```cpp
high = mid;
```

nahi.

Kyuki `mid` already check ho chuka hai.

Valid hone par:

```cpp
high = mid - 1;
```

---

## 5. `low = mid`

Invalid speed ko dobara check karne ki zarurat nahi.

Correct:

```cpp
low = mid + 1;
```

---

# 🔥 Pattern Recognition

Agar question me:

```text
Find minimum/maximum value
+
Ek possible value ko check kar sakte ho
+
Check karne ke baad valid/invalid decide hota hai
+
Valid/invalid monotonic hai
```

Toh socho:

```text
Binary Search on Answer
```

875 mein:

```text
Answer = Eating Speed
```

---

# 📌 Main Pattern

```text
Possible Speed
      ↓
Calculate Required Hours
      ↓
Compare with h
      ↓
 ┌───────────────┐
 ↓               ↓
hours <= h     hours > h
 ↓               ↓
VALID          INVALID
 ↓               ↓
LEFT           RIGHT
 ↓               ↓
high=mid-1    low=mid+1
      ↓
Binary Search End
      ↓
return low
```

---

# ⏱️ Time Complexity

Har Binary Search iteration mein speed range half hoti hai.

Binary Search:

```text
O(log(max(piles)))
```

Har iteration mein hum saare `n` piles check karte hain:

```text
O(n)
```

Therefore total:

```text
O(n log(max(piles)))
```

---

# 💾 Space Complexity

Extra data structure use nahi ho raha.

Sirf:

```text
low
high
mid
hours
```

variables hain.

Therefore:

```text
O(1)
```

---

# 📊 Complexity

```text
Time Complexity  → O(n log(max(piles)))

Space Complexity → O(1)
```

---

# 🧠 Interview Explanation

Agar interviewer pooche:

**"How do you solve Koko Eating Bananas using Binary Search?"**

Bolo:

```text
I use Binary Search on the eating speed.

The minimum possible speed is 1 and the maximum possible
speed is the maximum pile size.

For every possible speed mid, I calculate the total number
of hours required to finish all piles.

For each pile, the required hours are ceil(pile / mid).

If the total hours are less than or equal to h, the speed
is valid, but I try to find a smaller valid speed by moving
left.

If the total hours are greater than h, the speed is too slow,
so I move right.

At the end, low represents the minimum valid eating speed.

Time complexity is O(n log(max(piles))) and space complexity
is O(1).
```

---

# ⭐ Interview Revision Code

```cpp
class Solution {
public:
    int minEatingSpeed(vector<int>& piles, int h) {

        int low = 1;
        int high = *max_element(piles.begin(), piles.end());

        while (low <= high) {

            int mid = low + (high - low) / 2;

            long long hours = 0;

            for (int pile : piles) {
                hours += ceil((double)pile / mid);
            }

            if (hours <= h) {
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
Koko Eating Bananas
        ↓
Answer = Eating Speed
        ↓
low = 1
high = max(piles)
        ↓
mid = possible speed
        ↓
Calculate total hours
        ↓
hours <= h
    → valid
    → LEFT
    → high = mid - 1

hours > h
    → invalid
    → RIGHT
    → low = mid + 1
        ↓
return low
```

---

# 🧠 One-Line Revision

```text
Eating speed par Binary Search lagao → total hours calculate karo → valid ho to left, slow ho to right → minimum valid speed return karo.
```

---

# 🔥 Main Formula

```text
hours for one pile
= ceil(pile / speed)
```

Total:

```text
total hours
= Σ ceil(pile / speed)
```

Then:

```text
hours <= h
→ high = mid - 1
```

```text
hours > h
→ low = mid + 1
```

Finally:

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
Possible Eating Speed
    ↓
Calculate Feasibility
    ↓
Minimum Valid Speed
    ↓
LeetCode 875
```

---

# 🔗 Binary Search Progress

```text
13 → 69  Sqrt(x)
14 → 367 Valid Perfect Square
15 → 1539 Kth Missing Positive Number
16 → 875  Koko Eating Bananas ✅
17 → 1011 Capacity To Ship Packages Within D Days
```

**16th question complete.**
