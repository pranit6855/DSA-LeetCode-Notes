# 🔥 LeetCode 918 — Maximum Sum Circular Subarray

## 📌 Problem

Hume ek integer array `nums` diya hai.

Hume **maximum sum wala non-empty subarray** find karna hai.

Lekin ek twist hai:

> Array **circular** hai.

Matlab array ka **last element first element se connected** hai.

---

# 🧠 Circular Array ka Matlab

Normal array:

```text
1 → 2 → 3 → 4
```

Normal array me hum:

```text
4 → 1
```

nahi le sakte.

Lekin circular array me:

```text
        ┌───────────────┐
        ↓               │
1 → 2 → 3 → 4 ──────────┘
```

So:

```text
4 → 1
```

valid hai.

---

# 🔹 Example 1

```text
Input:
nums = [5,-3,5]
```

Normal maximum subarray:

```text
5 → -3 → 5
```

Sum:

```text
5 + (-3) + 5 = 7
```

Lekin circularly:

```text
5(last) → 5(first)
```

le sakte hain.

Sum:

```text
5 + 5 = 10
```

So:

```text
Output = 10
```

---

# 🔹 Example 2

```text
nums = [1,-2,3,-2]
```

Normal maximum:

```text
3
```

Circularly:

```text
3 → -2 → 1
```

ka sum:

```text
3 - 2 + 1 = 2
```

Actually better circular combination:

```text
1 + 3 = 4
```

So:

```text
Output = 4
```

---

# 🔹 Example 3

```text
nums = [-3,-2,-5]
```

Maximum subarray:

```text
[-2]
```

Answer:

```text
-2
```

Ye example **edge case** ke liye important hai.

---

# 🧠 Approach

Circular array me maximum subarray ke **2 cases** ho sakte hain.

```text
Case 1 → Normal subarray
Case 2 → Circular / wrapping subarray
```

Isliye dono find karenge.

---

# 1️⃣ Case 1 — Normal Maximum Subarray

Ye exactly **LC 53 — Maximum Subarray** wala Kadane Algorithm hai.

Example:

```text
[5,-3,5]
```

Normal maximum:

```text
5 + (-3) + 5 = 7
```

So:

```text
normal = 7
```

Hum apna same function use kar sakte hain:

```cpp
normaxsum(nums)
```

---

# 2️⃣ Case 2 — Circular Maximum

Ab interesting part.

Suppose:

```text
[5,-3,5]
```

Hume circular answer chahiye:

```text
5 → 5
```

Humne beech ka:

```text
-3
```

remove kar diya.

Array ka total sum:

```text
5 + (-3) + 5 = 7
```

Removed part:

```text
-3
```

So:

```text
7 - (-3)
= 10
```

Yahi circular answer hai.

---

# 🔥 Important Formula

```text
Circular Maximum
=
Total Sum - Minimum Subarray Sum
```

Matlab:

```cpp
circularSum = total - minSum;
```

---

# 🤔 Minimum Subarray hi Kyu?

Ye sabse important part hai.

Suppose array:

```text
A → B → C → D → E
```

Circular maximum agar wrap karta hai to wo kuch aisa ho sakta hai:

```text
D → E → A → B
```

Matlab humne middle ka:

```text
C
```

part hata diya.

So:

```text
Total Array
      -
Removed Part
      =
Circular Maximum
```

Ab hume **maximum remaining sum** chahiye.

To hume **minimum possible part remove** karna chahiye.

Therefore:

```text
Circular Maximum
=
Total Sum - Minimum Subarray Sum
```

---

# 🔥 Example Samjho

```text
nums = [5,-3,5]
```

Total:

```text
5 + (-3) + 5 = 7
```

Minimum subarray:

```text
[-3]
```

Minimum sum:

```text
-3
```

Now:

```text
Circular Maximum
=
Total - Minimum

= 7 - (-3)

= 10
```

Remaining elements:

```text
5 → 5
```

Exactly hume jo chahiye tha.

---

# 3️⃣ Minimum Subarray Sum

Ab hume:

```text
minimum subarray sum
```

find karna hai.

Iske liye bhi Kadane use karenge.

Normal Kadane:

```cpp
bestending = max(c1,c2);
```

Minimum Kadane:

```cpp
bestending = min(c1,c2);
```

---

# 🧠 Normal Kadane vs Minimum Kadane

### Maximum:

```cpp
int c1 = nums[i];
int c2 = bestending + nums[i];

bestending = max(c1,c2);
```

Matlab:

```text
Current element se start karu?
OR
Purana subarray continue karu?
```

---

### Minimum:

Exactly same logic:

```cpp
int c1 = nums[i];
int c2 = bestending + nums[i];

bestending = min(c1,c2);
```

Bas:

```text
max()
```

ki jagah:

```text
min()
```

---

# 🔥 3 Functions

Hum solution ko simple rakhne ke liye 3 functions banayenge:

```text
normaxsum()
totalsum()
minsum()
```

---

# 🟢 Function 1 — `normaxsum()`

Ye normal maximum subarray sum dega.

```cpp
int normaxsum(vector<int> nums)
```

Isme normal Kadane lagega.

### Variables:

```cpp
int bestending = nums[0];
int res = nums[0];
```

`bestending`:

```text
Current index par ending maximum sum
```

`res`:

```text
Ab tak ka overall maximum
```

---

# 🟢 Function 2 — `totalsum()`

Simple array ka total sum:

```cpp
int totalsum(vector<int> nums)
```

Example:

```text
[5,-3,5]
```

```text
5 + (-3) + 5 = 7
```

So:

```text
total = 7
```

---

# 🟢 Function 3 — `minsum()`

Ye minimum subarray sum find karega.

Example:

```text
[5,-3,5]
```

Minimum subarray:

```text
[-3]
```

So:

```text
minSum = -3
```

---

# 🔥 Complete Logic

Sabse pehle:

```cpp
int normal = normaxsum(nums);
```

Then:

```cpp
int total = totalsum(nums);
```

Then:

```cpp
int minimum = minsum(nums);
```

Circular answer:

```cpp
int circularSum = total - minimum;
```

Finally:

```cpp
int ans = max(normal,circularSum);
```

---

# ⚠️ Important Edge Case — All Negative

Ye question ka sabse important edge case hai.

Suppose:

```text
nums = [-3,-2,-5]
```

Normal maximum:

```text
-2
```

Correct answer:

```text
-2
```

Lekin agar circular formula lagaya:

### Total:

```text
-3 + (-2) + (-5)
= -10
```

### Minimum:

```text
-3 + (-2) + (-5)
= -10
```

Now:

```text
total - minimum

= -10 - (-10)

= 0
```

❌ But `0` wrong hai.

---

# 🤔 0 Kyu Galat Hai?

Because:

```text
total - minimum
```

me humne:

```text
poori array
```

remove kar di.

Matlab:

```text
Empty subarray
```

bach gaya.

But problem bolta hai:

```text
Subarray must be non-empty.
```

So empty subarray allowed nahi hai.

---

# 🔥 All Negative Kaise Detect Kare?

Hum:

```cpp
normal = normaxsum(nums);
```

already calculate kar rahe hain.

Agar:

```cpp
normal < 0
```

iska matlab maximum element bhi negative hai.

Therefore saare elements negative hain.

So:

```cpp
if(normal < 0){
    return normal;
}
```

---

# ⚠️ Ye Galti Mat Karna

Tumne initially socha tha:

```cpp
if(total < 0)
```

But ye correct check nahi hai.

Example:

```text
nums = [-5,4,-2]
```

Total:

```text
-5 + 4 - 2
= -3
```

So:

```text
total < 0
```

true hai.

But array me:

```text
4
```

positive hai.

Correct maximum:

```text
4
```

Agar `total` check karke return kar diya:

```text
-3
```

❌ Wrong.

Isliye:

```cpp
if(normal < 0)
```

use karna hai.

---

# 🔄 Detailed Dry Run

Let's take:

```text
nums = [5,-3,5]
```

---

## Step 1 — Normal Maximum

Initial:

```text
bestending = 5
res = 5
```

`-3`:

```text
c1 = -3
c2 = 5 + (-3)
   = 2
```

So:

```text
bestending = max(-3,2)
           = 2
```

```text
res = max(5,2)
    = 5
```

Next `5`:

```text
c1 = 5
c2 = 2 + 5
   = 7
```

So:

```text
bestending = 7
res = 7
```

Therefore:

```text
normal = 7
```

---

# Step 2 — Total Sum

```text
5 + (-3) + 5
```

```text
total = 7
```

---

# Step 3 — Minimum Sum

Start:

```text
bestending = 5
res = 5
```

`-3`:

```text
c1 = -3
c2 = 5 + (-3)
   = 2
```

Minimum:

```text
bestending = min(-3,2)
           = -3
```

```text
res = min(5,-3)
    = -3
```

Next `5`:

```text
c1 = 5
c2 = -3 + 5
   = 2
```

Minimum:

```text
bestending = -? 
min(5,2) = 2
```

But `res` remains:

```text
-3
```

So:

```text
minSum = -3
```

---

# Step 4 — Circular Sum

```text
circularSum = total - minSum
```

So:

```text
7 - (-3)
= 10
```

---

# Step 5 — Final Answer

```text
normal = 7
circular = 10
```

Therefore:

```text
ans = max(7,10)
    = 10
```

✅ Answer:

```text
10
```

---

# 🔄 Another Example

```text
nums = [3,-1,2,-1]
```

### Normal Kadane

Maximum:

```text
3 + (-1) + 2
= 4
```

So:

```text
normal = 4
```

### Total

```text
3 - 1 + 2 - 1
= 3
```

### Minimum

Minimum subarray:

```text
[-1]
```

So:

```text
minSum = -1
```

### Circular

```text
3 - (-1)
= 4
```

Final:

```text
max(4,4)
= 4
```

---

# 💻 Final Code

```cpp
class Solution {
public:

    int normaxsum(vector<int> nums){

        int bestending = nums[0];
        int res = nums[0];

        for(int i = 1; i < nums.size(); i++){

            int c1 = nums[i];
            int c2 = bestending + nums[i];

            bestending = max(c1,c2);

            res = max(res,bestending);
        }

        return res;
    }

    int totalsum(vector<int> nums){

        int sum = 0;

        for(int i = 0; i < nums.size(); i++){

            sum += nums[i];
        }

        return sum;
    }

    int minsum(vector<int> nums){

        int bestending = nums[0];
        int res = nums[0];

        for(int i = 1; i < nums.size(); i++){

            int c1 = nums[i];
            int c2 = bestending + nums[i];

            bestending = min(c1,c2);

            res = min(res,bestending);
        }

        return res;
    }

    int maxSubarraySumCircular(vector<int>& nums){

        int total = totalsum(nums);

        int normal = normaxsum(nums);

        // All elements are negative
        if(normal < 0){
            return normal;
        }

        int minimum = minsum(nums);

        int circularSum = total - minimum;

        int ans = max(normal,circularSum);

        return ans;
    }
};
```

---

# 🧠 Code Flow

```text
nums
 ↓
normaxsum()
 ↓
normal maximum
 ↓
normal < 0 ?
 ├── YES → return normal
 │
 └── NO
      ↓
   totalsum()
      ↓
   minsum()
      ↓
 total - minimum
      ↓
 max(normal,circularSum)
      ↓
    answer
```

---

# 🔥 Sabse Important Formula

```text
Circular Maximum
=
Total Sum - Minimum Subarray Sum
```

Aur final:

```text
Answer =
max(
    Normal Maximum,
    Circular Maximum
)
```

Except:

```text
If all elements are negative:
    Answer = Normal Maximum
```

---

# 🧠 Why This Is Kadane Pattern?

Is question me hum **same Kadane logic ko do baar** use kar rahe hain.

### Normal:

```cpp
bestending = max(c1,c2);
```

### Minimum:

```cpp
bestending = min(c1,c2);
```

So Kadane ka pattern:

```text
Current element
      ↓
Continue previous subarray
      OR
Start new subarray
      ↓
Choose best
```

Maximum ke liye:

```text
max()
```

Minimum ke liye:

```text
min()
```

---

# ⚠️ Common Mistakes

### ❌ Mistake 1

```cpp
if(total < 0)
```

Wrong.

Use:

```cpp
if(normal < 0)
```

---

### ❌ Mistake 2

```cpp
total + minimum
```

Wrong.

Correct:

```cpp
total - minimum
```

Example:

```text
total = 7
minimum = -3

7 - (-3)
= 10
```

---

### ❌ Mistake 3

Sirf circular answer return karna.

Wrong:

```cpp
return total - minimum;
```

Because normal subarray better ho sakta hai.

Always:

```cpp
max(normal,circularSum)
```

---

### ❌ Mistake 4

All-negative case ignore karna.

Example:

```text
[-3,-2,-5]
```

Circular formula:

```text
0
```

But correct:

```text
-2
```

So:

```cpp
if(normal < 0){
    return normal;
}
```

---

# ⏱️ Complexity

### Time

```text
O(n)
```

Hum array ko constant number of times traverse kar rahe hain.

### Space

```text
O(1)
```

Koi extra array nahi banaya.

---

# 🔥 Quick Revision

```text
Normal maximum
      ↓
   Kadane

Minimum subarray
      ↓
Reverse Kadane

Circular maximum
      ↓
Total - Minimum

Final
      ↓
max(Normal, Circular)

All negative
      ↓
return Normal
```

### One-line trick:

> **Circular maximum nikalne ke liye total array me se minimum-sum wala middle part hata do.**

---

# 📁 GitHub Structure

```text
Kadane's-Algorithm/
│
├── 01-Maximum-Subarray.md
├── 02-Maximum-Product-Subarray.md
└── 03-Maximum-Sum-Circular-Subarray.md
```

### Commit Message

```text
Add LC 918 Maximum Sum Circular Subarray detailed Hinglish notes
```
