# LC 724 — Find Pivot Index

## Pattern
Prefix Sum → Pivot / Equilibrium Index

---

# 1. Problem

Humein ek integer array `nums` diya gaya hai.

Humein aisa index find karna hai jahan:

    Left side ke elements ka sum
                  =
    Right side ke elements ka sum

Isi index ko **Pivot Index** bolte hain.

Important:

- Current element `nums[i]` left sum mein include nahi hoga.
- Current element `nums[i]` right sum mein bhi include nahi hoga.
- Agar multiple pivot indexes hain, toh leftmost pivot return karna hai.
- Agar koi pivot nahi mila, toh `-1` return karna hai.

---

# 2. Example

Input:

    nums = [1, 7, 3, 6, 5, 6]

Output:

    3

Kyun?

Index `3` par:

    [1, 7, 3 | 6 | 5, 6]
              ↑
            pivot

Left side:

    1 + 7 + 3 = 11

Right side:

    5 + 6 = 11

Therefore:

    Left Sum = Right Sum
    11 = 11

Isliye answer:

    3

---

# 3. Pivot Index ko Simple Language Mein Samjho

Har index par hum array ko 3 parts mein soch sakte hain:

    [ LEFT | CURRENT | RIGHT ]

Example:

    [1, 7, 3 | 6 | 5, 6]
              ↑
            current

Yahan:

    LEFT    = [1, 7, 3]
    CURRENT = [6]
    RIGHT   = [5, 6]

Pivot tab hoga jab:

    Sum(LEFT) = Sum(RIGHT)

Current element ko kisi bhi side mein count nahi karna hai.

---

# 4. Brute Force Approach

Sabse simple approach:

Har index ke liye:

1. Left side ka sum nikalo.
2. Right side ka sum nikalo.
3. Compare karo.
4. Equal mila toh index return karo.

Example:

    nums = [1, 7, 3, 6, 5, 6]

Index `0` ke liye left aur right sum calculate karenge.

Phir index `1` ke liye dobara calculate karenge.

Phir index `2` ke liye dobara.

Problem ye hai ki same elements ka sum baar-baar calculate ho raha hai.

Isliye:

    Time Complexity = O(n²)

Hum isko better bana sakte hain.

---

# 5. Optimized Prefix Sum Idea

Hum poore array ka sum **sirf ek baar** nikalenge.

Uske baad ek `leftSum` maintain karenge.

Maan lo current index `i` hai.

Poora array:

    TOTAL = LEFT + CURRENT + RIGHT

Humein `RIGHT` chahiye.

Toh:

    RIGHT = TOTAL - LEFT - CURRENT

Formula:

    rightSum = totalSum - leftSum - nums[i]

Ab har index par bas:

    1. rightSum nikalo
    2. leftSum aur rightSum compare karo

Agar equal:

    return i

---

# 6. Right Sum Formula Kaise Aaya?

Ye is question ka sabse important concept hai.

Hum jaante hain:

    Total Sum
        =
    Left Sum + Current Element + Right Sum

Mathematically:

    totalSum = leftSum + nums[i] + rightSum

Humein `rightSum` chahiye.

Dono sides se `leftSum` minus:

    totalSum - leftSum
        =
    nums[i] + rightSum

Ab dono sides se `nums[i]` minus:

    totalSum - leftSum - nums[i]
        =
    rightSum

Therefore:

    rightSum = totalSum - leftSum - nums[i]

Bas isi wajah se formula aaya.

---

# 7. Visual Formula

Array ko imagine karo:

    [ LEFT | CURRENT | RIGHT ]

Total mein teeno included hain:

    TOTAL
      |
      +---- LEFT
      |
      +---- CURRENT
      |
      +---- RIGHT

Isliye:

    RIGHT = TOTAL - LEFT - CURRENT

Code:

    int rightSum = totalSum - leftSum - nums[i];

---

# 8. Example Se Formula

Array:

    nums = [1, 7, 3, 6, 5, 6]

Total sum:

    1 + 7 + 3 + 6 + 5 + 6 = 28

Suppose `i = 3`:

    [1, 7, 3 | 6 | 5, 6]
              ↑

Left:

    1 + 7 + 3 = 11

Current:

    6

Right:

    5 + 6 = 11

Formula:

    rightSum = totalSum - leftSum - nums[i]

    rightSum = 28 - 11 - 6
             = 11

Correct.

---

# 9. Step-by-Step Approach

## Step 1 — Total Sum Nikalo

Pehle poore array ka sum nikaalenge.

Example:

    nums = [1, 7, 3, 6, 5, 6]

    totalSum = 28

---

## Step 2 — leftSum Initialize Karo

Starting mein index `0` ke left mein kuch bhi nahi hai.

So:

    leftSum = 0

---

## Step 3 — Array Traverse Karo

Har index `i` par:

    rightSum = totalSum - leftSum - nums[i]

---

## Step 4 — Pivot Check Karo

Check:

    if(leftSum == rightSum)

Agar equal:

    return i

---

## Step 5 — leftSum Update Karo

Agar pivot nahi mila:

    leftSum += nums[i]

Ye current element ko **next index ke left side** mein add kar dega.

---

# 10. Sabse Important — leftSum Last Mein Kyun Update Kiya?

Ye bahut important hai.

Code:

    if(leftSum == rightSum) {
        return i;
    }

    leftSum += nums[i];

Question:

**`leftSum += nums[i]` baad mein kyun?**

Reason:

Current element ko current index ke left side mein include nahi karna hai.

Example:

    [1, 7, 3 | 6 | 5, 6]
              ↑
            current

At `i = 3`:

    leftSum = 1 + 7 + 3
            = 11

Current element:

    nums[3] = 6

Isliye current index ke liye:

    leftSum = 11

`6` ko abhi left mein add nahi karna.

Pehle check:

    leftSum == rightSum

Agar pivot nahi mila, tab:

    leftSum += nums[i]

So:

    leftSum = 11 + 6
            = 17

Ab next index `4` par:

    [1, 7, 3, 6 | 5 | 6]
                 ↑

Ab `6` left side mein aa chuka hai.

Therefore:

    Current element
    ↓
    Pehle CHECK
    ↓
    Phir leftSum mein ADD

Ye order bahut important hai.

---

# 11. Golden Rule

Is problem mein hamesha yaad rakho:

    CHECK FIRST
    UPDATE LEFT AFTER

Matlab:

    rightSum calculate karo
           ↓
    left == right check karo
           ↓
    current ko left mein add karo
           ↓
    next index

---

# 12. Detailed Dry Run

Input:

    nums = [1, 7, 3, 6, 5, 6]

Total:

    totalSum = 28

Initially:

    leftSum = 0

---

## i = 0

Array:

    [ | 1 | 7, 3, 6, 5, 6]
        ↑
      current

Values:

    totalSum = 28
    leftSum = 0
    nums[i] = 1

Right:

    rightSum = 28 - 0 - 1
             = 27

Compare:

    leftSum = 0
    rightSum = 27

Not equal.

Update:

    leftSum = 0 + 1
            = 1

---

## i = 1

Array:

    [1 | 7 | 3, 6, 5, 6]
         ↑
       current

Values:

    leftSum = 1
    nums[i] = 7

Right:

    rightSum = 28 - 1 - 7
             = 20

Compare:

    1 != 20

Not pivot.

Update:

    leftSum = 1 + 7
            = 8

---

## i = 2

Array:

    [1, 7 | 3 | 6, 5, 6]
           ↑
         current

Values:

    leftSum = 8
    nums[i] = 3

Right:

    rightSum = 28 - 8 - 3
             = 17

Compare:

    8 != 17

Not pivot.

Update:

    leftSum = 8 + 3
            = 11

---

## i = 3

Array:

    [1, 7, 3 | 6 | 5, 6]
              ↑
            current

Values:

    leftSum = 11
    nums[i] = 6
    totalSum = 28

Right:

    rightSum = 28 - 11 - 6
             = 11

Compare:

    leftSum  = 11
    rightSum = 11

Equal!

Therefore:

    return 3

Final answer:

    3

---

# 13. Dry Run Table

| i | nums[i] | leftSum Before | rightSum | Pivot? | leftSum After |
|---|---:|---:|---:|---|---:|
| 0 | 1 | 0 | 27 | No | 1 |
| 1 | 7 | 1 | 20 | No | 8 |
| 2 | 3 | 8 | 17 | No | 11 |
| 3 | 6 | 11 | 11 | YES | -- |

Answer:

    3

---

# 14. C++ Code

    class Solution {
    public:
        int pivotIndex(vector<int>& nums) {

            int totalSum = 0;

            // Calculate total sum
            for(int x : nums) {
                totalSum += x;
            }

            int leftSum = 0;

            // Check every index
            for(int i = 0; i < nums.size(); i++) {

                // Calculate right sum
                int rightSum = totalSum - leftSum - nums[i];

                // Check pivot
                if(leftSum == rightSum) {
                    return i;
                }

                // Current becomes left for next index
                leftSum += nums[i];
            }

            return -1;
        }
    };

---

# 15. Code Ko Line By Line Samjho

## Line 1

    int totalSum = 0;

Poore array ka total store karne ke liye.

---

## Total Sum Calculate

    for(int x : nums) {
        totalSum += x;
    }

Example:

    [1, 7, 3, 6, 5, 6]

Result:

    totalSum = 28

---

## leftSum

    int leftSum = 0;

Starting mein left side empty hai.

So:

    leftSum = 0

---

## Loop

    for(int i = 0; i < nums.size(); i++)

Har index ko pivot candidate maan kar check karenge.

---

## Right Sum

    int rightSum = totalSum - leftSum - nums[i];

Ye main formula hai.

Remember:

    TOTAL = LEFT + CURRENT + RIGHT

Therefore:

    RIGHT = TOTAL - LEFT - CURRENT

---

## Pivot Check

    if(leftSum == rightSum) {
        return i;
    }

Agar left aur right equal hain, pivot mil gaya.

---

## leftSum Update

    leftSum += nums[i];

Current element ko next index ke left side mein daal rahe hain.

---

## No Pivot

    return -1;

Agar poori array traverse kar li aur koi pivot nahi mila.

---

# 16. Brute Force vs Optimized

## Brute Force

Har index par left aur right sum dobara calculate karna.

Time:

    O(n²)

Space:

    O(1)

---

## Optimized

Total sum ek baar.

Left sum running maintain.

Right sum formula se.

Time:

    O(n)

Space:

    O(1)

---

# 17. Complexity

## Time Complexity

First loop:

    O(n)

Second loop:

    O(n)

Overall:

    O(n) + O(n)
    = O(n)

Therefore:

    Time = O(n)

---

## Space Complexity

Sirf variables use ho rahe hain:

    totalSum
    leftSum
    rightSum

Koi extra array nahi.

Therefore:

    Space = O(1)

---

# 18. Common Mistakes

## Mistake 1 — Current Element Pehle Add Kar Dena

Wrong:

    leftSum += nums[i];

    if(leftSum == rightSum) {
        return i;
    }

Problem:

Current element left sum mein aa jayega.

But current element left side ka part nahi hai.

Correct:

    if(leftSum == rightSum) {
        return i;
    }

    leftSum += nums[i];

---

## Mistake 2 — Current Element Ko Right Sum Se Minus Na Karna

Wrong:

    rightSum = totalSum - leftSum;

Problem:

`totalSum` ke andar current element bhi included hai.

Correct:

    rightSum = totalSum - leftSum - nums[i];

---

## Mistake 3 — Har Baar Sum Recalculate Karna

Aisa karoge toh:

    O(n²)

ho jayega.

Better:

    totalSum → once
    leftSum  → running
    rightSum → formula

So:

    O(n)

---

# 19. Edge Cases

## Case 1 — Single Element

    nums = [5]

Left side:

    0

Right side:

    0

So pivot index:

    0

---

## Case 2 — No Pivot

    nums = [1, 2, 3]

Kisi bhi index par left sum aur right sum equal nahi hoga.

Answer:

    -1

---

## Case 3 — Pivot At First Index

    nums = [2, 1, -1]

At index `0`:

    leftSum = 0

    rightSum = 1 + (-1)
            = 0

So answer:

    0

---

## Case 4 — Negative Numbers

Negative numbers ke saath bhi same logic chalega.

Example:

    nums = [-1, -1, -1, 0, 1, 1]

Formula same rahega:

    rightSum = totalSum - leftSum - nums[i]

---

# 20. Pattern Recognition

Jab question mein aisa kuch diya ho:

    "Find an index where the sum of elements
     on the left is equal to the sum on the right."

Immediately socho:

    Prefix Sum
         +
    Running Sum

Is problem mein full prefix array banana necessary nahi hai.

Hum sirf:

    totalSum
    leftSum

maintain kar rahe hain.

---

# 21. Prefix Sum Is Actually Kya Kar Raha Hai?

Prefix Sum ka basic idea hota hai:

    Previous calculated sum ko reuse karna.

Yahan hum:

    leftSum

ko baar-baar calculate nahi kar rahe.

Example:

    i = 0
    leftSum = 0

    i = 1
    leftSum = 1

    i = 2
    leftSum = 1 + 7 = 8

    i = 3
    leftSum = 8 + 3 = 11

Matlab previous information reuse ho rahi hai.

Isi wajah se repeated calculation avoid hoti hai.

---

# 22. Core Formula

Sabse important formula:

    totalSum = leftSum + nums[i] + rightSum

Rearrange:

    rightSum = totalSum - leftSum - nums[i]

Aur pivot condition:

    leftSum == rightSum

Bas problem ka poora logic in 2 lines mein hai:

    rightSum = totalSum - leftSum - nums[i]

    if(leftSum == rightSum)
        return i;

---

# 23. Interview Mein Kaise Explain Karna Hai?

Simple Hinglish:

> "Main pehle poore array ka total sum calculate karunga. Phir array ko traverse karte time ek running left sum maintain karunga. Har index par right sum ko `totalSum - leftSum - nums[i]` se calculate karunga. Agar left sum aur right sum equal hain, toh current index pivot index hai. Check karne ke baad current element ko left sum mein add karunga, taaki next index ke liye wo left side mein count ho."

---

# 24. One-Line Interview Explanation

    Total - Left - Current = Right

Then:

    Left == Right

means:

    Pivot Index

---

# 25. Memory Trick

Array ko hamesha aise imagine karo:

    [ LEFT | CURRENT | RIGHT ]

And:

    TOTAL
      =
    LEFT + CURRENT + RIGHT

Therefore:

    RIGHT
      =
    TOTAL - LEFT - CURRENT

Then:

    LEFT == RIGHT

    ↓

    Pivot Found

---

# 26. Most Important Learning From LC 724

Is question se Prefix Sum ka pehla major concept samjho:

    Repeated Sum Calculation ko avoid karna.

Instead of:

    Har baar poora left sum calculate karo

we do:

    leftSum ko maintain karo.

Instead of:

    Har baar poora right sum calculate karo

we do:

    rightSum = totalSum - leftSum - current

Result:

    O(n²) → O(n)

---

# 27. Full Final Code

    class Solution {
    public:
        int pivotIndex(vector<int>& nums) {

            int totalSum = 0;

            for(int x : nums) {
                totalSum += x;
            }

            int leftSum = 0;

            for(int i = 0; i < nums.size(); i++) {

                int rightSum = totalSum - leftSum - nums[i];

                if(leftSum == rightSum) {
                    return i;
                }

                leftSum += nums[i];
            }

            return -1;
        }
    };

---

# 28. Quick Revision

Question:

    Find Pivot Index

Pattern:

    Prefix Sum → Pivot / Equilibrium

Main Formula:

    rightSum = totalSum - leftSum - nums[i]

Pivot Condition:

    leftSum == rightSum

Order:

    1. Calculate rightSum
    2. Check pivot
    3. Update leftSum
    4. Move next

Time:

    O(n)

Space:

    O(1)

---

# 29. 10-Second Revision Before Interview

Agar interview se just pehle dekhna ho:

    totalSum = complete array ka sum

    leftSum = 0

    for every i:

        rightSum = totalSum - leftSum - nums[i]

        if(leftSum == rightSum):
            return i

        leftSum += nums[i]

    return -1

Most important:

    CHECK FIRST
    UPDATE AFTER

---

# 30. Status

✅ LC 724 — Find Pivot Index

Pattern Completed:

    Prefix Sum → Pivot / Equilibrium

Core Concept:

    Total Sum + Running Left Sum

Core Formula:

    rightSum = totalSum - leftSum - nums[i]

Complexity:

    O(n) Time
    O(1) Space

Next Major Prefix Sum Pattern:

    Prefix Sum + HashMap

Next Important Question:

    LC 560 — Subarray Sum Equals K
