# LeetCode 713 - Subarray Product Less Than K

## 📌 Problem

Hume integer array `nums` aur integer `k` diya hai.

Hume total number of **continuous subarrays** find karne hain jinka product:

```text
product < k
```

ho.

### Example

```text
nums = [10,5,2,6]
k = 100
```

Valid subarrays:

```text
[10]      → 10   ✅
[5]       → 5    ✅
[2]       → 2    ✅
[6]       → 6    ✅
[10,5]    → 50   ✅
[5,2]     → 10   ✅
[2,6]     → 12   ✅
[5,2,6]   → 60   ✅
```

But:

```text
[10,5,2] → 100 ❌
```

because condition strictly:

```text
product < k
```

Answer:

```text
8
```

---

# 🧠 Main Idea

Variable size sliding window use karenge.

Current window ka product maintain karenge:

```cpp
int product = 1;
```

`high` se new element add:

```cpp
product = product * nums[high];
```

Agar:

```text
product >= k
```

ho gaya, window invalid hai.

Tab `low` se shrink karenge.

---

# 🔥 Variables

```cpp
int n = nums.size();
int low = 0;
int high = 0;
int count = 0;
int product = 1;
```

Meaning:

```text
low     → window ka left pointer
high    → window ka right pointer
product → current window ka product
count   → total valid subarrays
```

---

# 🔥 Step 1 - Expand Window

`high` wala element product me include:

```cpp
product = product * nums[high];
```

Example:

```text
nums = [10,5,2]
```

Product:

```text
10

10 × 5 = 50

50 × 2 = 100
```

---

# 🔥 Step 2 - Invalid Window Check

Condition chahiye:

```text
product < k
```

So window invalid hogi jab:

```text
product >= k
```

Therefore:

```cpp
while(product >= k)
```

se shrink karenge.

---

# 🔥 Step 3 - Shrink Window

Leftmost element ko product se remove karne ke liye divide:

```cpp
product = product / nums[low];
low++;
```

Example:

```text
nums = [10,5,2]
product = 100
k = 100
```

Invalid:

```text
100 >= 100
```

Left wala `10` remove:

```text
product = 100 / 10
        = 10
```

Window:

```text
10 [5,2]
```

Now:

```text
product = 10 < 100
```

Valid ✅

---

# 🔥 Most Important Concept - Counting Subarrays

Ye question maximum length nahi pooch raha.

Hume:

```text
total number of valid subarrays
```

chahiye.

Window valid hone ke baad:

```cpp
int len = high-low+1;
```

Aur:

```cpp
count = count + len;
```

---

# 🧠 Why `high-low+1`?

Suppose current valid window:

```text
[5,2,6]
 ↑   ↑
low high
```

Current `high` element `6` hai.

`6` par end hone wale valid subarrays:

```text
[6]

[2,6]

[5,2,6]
```

Total:

```text
3
```

Window length bhi:

```text
high-low+1
= 3
```

So:

```cpp
count += high-low+1;
```

Current `high` par end hone wale saare valid subarrays count kar deta hai.

---

# 🔥 Full Example

Given:

```text
nums = [10,5,2,6]
k = 100
```

### high = 0

Window:

```text
[10]
```

Product:

```text
10
```

Valid.

Current `high` par end hone wale:

```text
[10]
```

Count:

```text
1
```

So:

```text
count = 1
```

---

### high = 1

Window:

```text
[10,5]
```

Product:

```text
50
```

Valid.

Current `high` par end hone wale:

```text
[5]
[10,5]
```

Total:

```text
2
```

So:

```text
count = 1 + 2
      = 3
```

---

### high = 2

Window:

```text
[10,5,2]
```

Product:

```text
100
```

Invalid ❌

Shrink:

```text
product = 100 / 10
        = 10
```

New window:

```text
[5,2]
```

Valid.

Current `high` par end hone wale:

```text
[2]
[5,2]
```

Total:

```text
2
```

So:

```text
count = 3 + 2
      = 5
```

---

### high = 3

Add `6`:

```text
[5,2,6]
```

Product:

```text
5 × 2 × 6 = 60
```

Valid.

Current `high` par end hone wale:

```text
[6]
[2,6]
[5,2,6]
```

Total:

```text
3
```

So:

```text
count = 5 + 3
      = 8
```

Final answer:

```text
8
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    int numSubarrayProductLessThanK(vector<int>& nums, int k) {

        if(k <= 1){
            return 0;
        }

        int n=nums.size();
        int low=0;
        int high=0;
        int count=0;
        int product=1;

        while(high<n){

            product=product*nums[high];

            while(product>=k){

                product=product/nums[low];
                low++;
            }

            int len=high-low+1;

            count=count+len;

            high++;
        }

        return count;
    }
};
```

---

# 🔥 Straight Flow

```text
START
  ↓
low = 0, high = 0
  ↓
product = 1, count = 0
  ↓
nums[high] ko product me multiply
  ↓
product >= k ?
  ↓ YES
product /= nums[low]
  ↓
low++
  ↓
jab tak product >= k, shrink
  ↓
WINDOW VALID
  ↓
len = high-low+1
  ↓
count = count + len
  ↓
high++
  ↓
REPEAT
  ↓
return count
```

---

# 🧠 Why `while`, Not `if`?

Suppose:

```text
nums = [10,10,10]
k = 50
```

Window:

```text
[10,10]
```

Product:

```text
100
```

Invalid.

Ek element remove:

```text
100 / 10 = 10
```

Valid.

Kisi aur case me multiple elements remove karne pad sakte hain.

Isliye:

```cpp
while(product >= k)
```

use karte hain.

---

# ⚠️ Important Mistake - Double Multiplication

Once we already do:

```cpp
product = product * nums[high];
```

then condition:

```cpp
while(product * nums[high] >= k)
```

❌ wrong hai.

Because `nums[high]` already product me included hai.

Correct:

```cpp
while(product >= k)
```

---

# ⚠️ Important Difference - `max_len` Nahi Chahiye

Previous questions me:

```cpp
int len = high-low+1;
max_len = max(max_len,len);
```

use kar rahe the.

But yahan question hai:

```text
How many valid subarrays?
```

So:

```cpp
int len = high-low+1;
count = count+len;
```

---

# 🔥 Why `k <= 1` Returns 0?

`nums` me positive integers hain.

Smallest possible product:

```text
1
```

But condition hai:

```text
product < k
```

Agar:

```text
k = 1
```

to chahiye:

```text
product < 1
```

Positive integers ke saath possible nahi hai.

Similarly `k = 0` par bhi possible nahi.

So:

```cpp
if(k <= 1){
    return 0;
}
```

---

# ⏱️ Time Complexity

`high` har element ko ek baar process karta hai.

`low` bhi sirf forward move karta hai.

```text
Time Complexity = O(n)
```

---

# 💾 Space Complexity

Koi map ya extra array nahi use kiya.

```text
Space Complexity = O(1)
```

---

# 🔥 Quick Revision

```text
high se expand
      ↓
product *= nums[high]
      ↓
product >= k ?
      ↓ YES
product /= nums[low]
      ↓
low++
      ↓
valid hone tak shrink
      ↓
count += high-low+1
      ↓
high++
```

---

# ⭐ One-Line Revision

> High se product badhao; product `k` ya usse zyada hote hi low se divide karke shrink karo, aur valid hone ke baad `high-low+1` current high par end hone wale valid subarrays ki count me add karo.

## Pattern

```text
Variable Sliding Window
+
Running Product
+
Shrink Until Product < K
+
Count Subarrays
```
