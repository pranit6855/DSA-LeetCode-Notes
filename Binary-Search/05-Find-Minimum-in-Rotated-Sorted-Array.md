# LeetCode 153 - Find Minimum in Rotated Sorted Array

## 📌 Problem

Hume ek array `nums` diya hai jo originally **ascending sorted** tha.

But array ko kisi unknown position par rotate kar diya gaya hai.

Hume array ka **minimum element** find karna hai.

### Important Conditions

```text
1. Array originally sorted tha.
2. Array rotate hua hai.
3. All elements are unique.
4. Answer minimum element hai.
```

Question ki required complexity:

```text
O(log n)
```

Isliye hume:

```text
Binary Search
```

use karna hai.

---

# 🔹 Example

```text
nums = [3,4,5,1,2]
```

Array originally:

```text
[1,2,3,4,5]
```

rotate hone ke baad:

```text
[3,4,5,1,2]
```

Minimum:

```text
1
```

So:

```text
Output = 1
```

---

# 🔹 Another Example

```text
nums = [4,5,6,7,0,1,2]
```

Minimum:

```text
0
```

So:

```text
Output = 0
```

---

# 🧠 What Is a Rotated Sorted Array?

Suppose original sorted array:

```text
[0,1,2,4,5,6,7]
```

Agar isko rotate karein:

```text
[4,5,6,7,0,1,2]
```

To array ab completely sorted nahi dikhta.

Lekin important observation:

```text
Array me do sorted parts hain.
```

Example:

```text
[4,5,6,7] [0,1,2]
```

First part sorted:

```text
4 < 5 < 6 < 7
```

Second part sorted:

```text
0 < 1 < 2
```

Problem ye hai ki hume ye pata karna hai:

```text
Minimum kis side hai?
```

---

# 🔥 Main Observation

Hum Binary Search me:

```text
low
mid
high
```

maintain karenge.

Har iteration me `mid` ko `high` se compare karenge:

```cpp
nums[mid] > nums[high]
```

Ye comparison hume batata hai ki minimum kis side hai.

---

# 🔥 Case 1

```cpp
nums[mid] > nums[high]
```

Example:

```text
nums = [4,5,6,7,0,1,2]
```

Suppose:

```text
mid = 3
nums[mid] = 7

high = 6
nums[high] = 2
```

So:

```text
7 > 2
```

True.

### Iska meaning

`mid` aur `high` ke beech values sorted order me nahi hain.

Normally sorted array me:

```text
smaller → bigger
```

hota hai.

But yahan:

```text
7 → 2
```

hai.

Matlab rotation point `mid` ke right side me hai.

Aur rotation point hi minimum hai.

So:

```text
Minimum → RIGHT side
```

Therefore:

```cpp
low = mid + 1;
```

---

# 🔹 Visualization

```text
[4,5,6,7 | 0,1,2]
         ↑       ↑
        mid    high
```

Values:

```text
nums[mid]  = 7
nums[high] = 2
```

Since:

```text
7 > 2
```

minimum right me:

```text
[4,5,6,7 | 0,1,2]
             ↑
          minimum
```

So:

```cpp
low = mid + 1;
```

---

# 🔥 Case 2

```cpp
nums[mid] <= nums[high]
```

Example:

```text
nums = [6,7,0,1,2,4,5]
```

Suppose:

```text
mid = 3
nums[mid] = 1

high = 6
nums[high] = 5
```

So:

```text
1 < 5
```

### Iska meaning

`mid` se `high` tak part sorted hai:

```text
[1,2,4,5]
```

Since this part sorted hai, minimum:

```text
mid
```

par ho sakta hai.

Ya:

```text
mid ke LEFT
```

me ho sakta hai.

So hume `mid` ko remove nahi karna.

Therefore:

```cpp
high = mid;
```

---

# 🔥 Important: `high = mid`, NOT `high = mid - 1`

Ye question ka bahut important concept hai.

Suppose:

```text
nums = [4,5,6,1,2,3]
```

Agar:

```text
mid = 3
nums[mid] = 1
```

Then:

```text
1
```

khud minimum ho sakta hai.

Agar hum likhen:

```cpp
high = mid - 1;
```

to:

```text
1
```

ko search se remove kar denge.

Ye wrong hoga.

Isliye:

```cpp
high = mid;
```

use karte hain.

Meaning:

```text
Minimum mid ho sakta hai
OR
minimum mid ke left me ho sakta hai.
```

---

# 📊 Main Logic

```text
nums[mid] > nums[high]
        ↓
Minimum RIGHT me
        ↓
low = mid + 1
```

```text
nums[mid] <= nums[high]
        ↓
Minimum LEFT ya MID me
        ↓
high = mid
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    int findMin(vector<int>& nums) {

        int low = 0;
        int high = nums.size() - 1;

        while(low < high) {

            int mid = low + (high - low) / 2;

            if(nums[mid] > nums[high]) {

                // Minimum right side me hai
                low = mid + 1;
            }

            else {

                // Minimum mid ya left side me hai
                high = mid;
            }
        }

        return nums[low];
    }
};
```

---

# 🧠 Code Line By Line

## Step 1 - `low`

```cpp
int low = 0;
```

Search first index se start hogi.

---

## Step 2 - `high`

```cpp
int high = nums.size() - 1;
```

Last valid index:

```text
n - 1
```

hota hai.

---

## Step 3 - Loop

```cpp
while(low < high)
```

Ye tab tak chalega jab tak:

```text
low
```

aur:

```text
high
```

different hain.

Jab:

```text
low == high
```

ho jata hai, sirf ek element bachta hai.

Wahi minimum hota hai.

---

# 🔥 Why `while(low < high)`?

Suppose:

```text
low = 4
high = 4
```

Search space:

```text
[0]
```

Sirf ek element bacha hai.

Ab aur Binary Search karne ki zarurat nahi.

So loop stop.

Therefore:

```cpp
while(low < high)
```

---

# 🔥 Why `return nums[low]`?

Loop ke end me:

```text
low == high
```

hoga.

So:

```text
nums[low]
```

aur:

```text
nums[high]
```

same element hai.

Aur wahi minimum hai.

Therefore:

```cpp
return nums[low];
```

---

# 🔄 Complete Dry Run

Given:

```text
nums = [4,5,6,7,0,1,2]
```

Array:

```text
Index:  0 1 2 3 4 5 6
Value:  4 5 6 7 0 1 2
```

---

## Initial

```text
low = 0
high = 6
```

```text
[4,5,6,7,0,1,2]
 ↑           ↑
low         high
```

---

## Iteration 1

Calculate:

```text
mid = low + (high-low)/2

mid = 0 + (6-0)/2
    = 3
```

So:

```text
nums[mid] = nums[3] = 7
nums[high] = nums[6] = 2
```

Compare:

```text
7 > 2
```

True.

Therefore:

```text
Minimum RIGHT side me hai.
```

Update:

```cpp
low = mid + 1;
```

So:

```text
low = 4
high = 6
```

Search space:

```text
[0,1,2]
 ↑     ↑
low   high
```

---

## Iteration 2

```text
mid = 4 + (6-4)/2
    = 5
```

Values:

```text
nums[mid] = 1
nums[high] = 2
```

Compare:

```text
1 > 2
```

False.

So:

```text
Minimum mid ya left me hai.
```

Therefore:

```cpp
high = mid;
```

So:

```text
low = 4
high = 5
```

Search space:

```text
[0,1]
 ↑   ↑
low high
```

---

## Iteration 3

```text
mid = 4 + (5-4)/2
    = 4
```

Values:

```text
nums[mid] = 0
nums[high] = 1
```

Compare:

```text
0 > 1
```

False.

So:

```cpp
high = mid;
```

Therefore:

```text
low = 4
high = 4
```

Now:

```text
low == high
```

Loop stop.

---

# ✅ Final Answer

```text
nums[low]
= nums[4]
= 0
```

So:

```text
Output = 0
```

---

# 🔥 Dry Run 2

Given:

```text
nums = [5,6,7,1,2,3,4]
```

Initial:

```text
low = 0
high = 6
```

---

## Iteration 1

```text
mid = 3
```

Values:

```text
nums[mid] = 1
nums[high] = 4
```

Compare:

```text
1 > 4
```

False.

So:

```text
high = mid
```

Now:

```text
low = 0
high = 3
```

---

## Iteration 2

```text
mid = 1
```

Values:

```text
nums[mid] = 6
nums[high] = 1
```

Compare:

```text
6 > 1
```

True.

So:

```text
low = mid + 1
```

Therefore:

```text
low = 2
high = 3
```

---

## Iteration 3

```text
mid = 2
```

Values:

```text
nums[mid] = 7
nums[high] = 1
```

Compare:

```text
7 > 1
```

True.

So:

```text
low = mid + 1
```

Therefore:

```text
low = 3
high = 3
```

Loop stop.

Final:

```text
nums[3] = 1
```

Answer:

```text
1
```

---

# 🔥 Dry Run 3 - Array Almost Normal

```text
nums = [1,2,3,4,5]
```

Yahan array rotated nahi hai.

Minimum:

```text
1
```

Initial:

```text
low = 0
high = 4
```

### Iteration 1

```text
mid = 2
nums[mid] = 3
nums[high] = 5
```

```text
3 > 5
```

False.

So:

```text
high = 2
```

---

### Iteration 2

```text
low = 0
high = 2
mid = 1
```

```text
nums[mid] = 2
nums[high] = 3
```

```text
2 > 3
```

False.

So:

```text
high = 1
```

---

### Iteration 3

```text
low = 0
high = 1
mid = 0
```

```text
nums[mid] = 1
nums[high] = 2
```

```text
1 > 2
```

False.

So:

```text
high = 0
```

Now:

```text
low = 0
high = 0
```

Answer:

```text
nums[0] = 1
```

---

# 🧠 Why Compare With `nums[high]`?

Ye important intuition hai.

Hum `mid` ko `high` se compare karte hain because `high` current search range ka **rightmost element** hai.

### If:

```text
nums[mid] > nums[high]
```

then:

```text
mid → high
```

part me rotation break hai.

Example:

```text
[4,5,6,7 | 0,1,2]
       ↑        ↑
      mid      high
```

```text
7 > 2
```

So minimum right me.

---

### If:

```text
nums[mid] <= nums[high]
```

then:

```text
mid → high
```

part sorted hai.

Example:

```text
[6,7,0 | 1,2,4,5]
       ↑           ↑
      mid         high
```

```text
1 < 5
```

So right part sorted hai.

Minimum `mid` ya left me ho sakta hai.

Therefore:

```text
high = mid
```

---

# 🔥 One Important Mental Picture

Always imagine:

```text
[ sorted part | sorted part ]
              ↑
           minimum
```

Rotation ke baad array me ek **break point** hota hai.

Example:

```text
[4,5,6,7 | 0,1,2]
          ↑
        break
```

Break ke baad jo first element hai:

```text
0
```

wahi minimum hai.

Binary Search ka kaam:

```text
Break point ko locate karna.
```

---

# ❌ Common Mistakes

## 1. `high = mid - 1`

Wrong:

```cpp
high = mid - 1;
```

Jab:

```text
nums[mid] <= nums[high]
```

kyunki `mid` khud minimum ho sakta hai.

Correct:

```cpp
high = mid;
```

---

## 2. `low = mid`

Wrong:

```cpp
low = mid;
```

Agar:

```text
nums[mid] > nums[high]
```

then `mid` definitely minimum nahi hai.

So:

```cpp
low = mid + 1;
```

correct hai.

---

## 3. `while(low <= high)`

Is problem ke standard approach me:

```cpp
while(low < high)
```

use karna easier hai.

Kyunki hum answer ko ek single index tak narrow kar rahe hain.

End me:

```text
low == high
```

and:

```text
nums[low]
```

answer hai.

---

## 4. `return -1`

LC 153 me:

```text
-1
```

return nahi karna.

Question minimum element maang raha hai.

Valid input me minimum hamesha exist karta hai.

Correct:

```cpp
return nums[low];
```

---

# 🔥 LC 153 vs Normal Binary Search

## Normal Binary Search - LC 704

Target search kar rahe the:

```text
nums[mid] == target
        ↓
return mid
```

---

## LC 153

Minimum search kar rahe hain:

```text
nums[mid] > nums[high]
        ↓
minimum RIGHT

otherwise
        ↓
minimum LEFT / MID
```

So ye **target searching** nahi hai.

Ye:

```text
Binary Search on Position of Minimum
```

hai.

---

# 🧠 Pattern Recognition

Question me agar ye dikhe:

```text
Sorted Array
+
Rotated
+
All elements unique
+
Find minimum
+
O(log n)
```

Immediately:

```text
Rotated Sorted Array
        ↓
Binary Search
        ↓
Compare nums[mid] with nums[high]
```

---

# 📌 Main Pattern

```text
nums[mid] > nums[high]
        ↓
Minimum RIGHT
        ↓
low = mid + 1
```

```text
nums[mid] <= nums[high]
        ↓
Minimum LEFT or MID
        ↓
high = mid
```

---

# 🔥 Important Difference

Yaad rakh:

```text
nums[mid] > nums[high]
```

ka matlab **minimum LEFT nahi**.

Actually:

```text
Minimum RIGHT
```

hai.

Aur:

```text
nums[mid] <= nums[high]
```

ka matlab:

```text
Minimum LEFT or MID
```

hai.

---

# ⏱️ Time Complexity

Har iteration me search space roughly half hota hai.

Therefore:

```text
Time Complexity = O(log n)
```

---

# 💾 Space Complexity

Sirf:

```text
low
high
mid
```

variables hain.

No extra array/map.

Therefore:

```text
Space Complexity = O(1)
```

---

# 📊 Complexity

```text
Time Complexity  → O(log n)

Space Complexity → O(1)
```

---

# 🧠 Interview Explanation

Agar interviewer pooche:

**"How do you find minimum in a rotated sorted array in O(log n)?"**

Bolo:

```text
The array was originally sorted but has been rotated.

At each step, I compare the middle element with the last
element of the current search range.

If nums[mid] is greater than nums[high], the rotation point
and therefore the minimum must be on the right side, so I move
low to mid + 1.

Otherwise, the right portion from mid to high is sorted, so
the minimum can be at mid or on the left side. Therefore,
I move high to mid.

When low becomes equal to high, only one possible minimum
remains, so I return nums[low].

The time complexity is O(log n) and the space complexity is O(1).
```

---

# ⭐ Interview Revision Code

```cpp
class Solution {
public:
    int findMin(vector<int>& nums) {

        int low = 0;
        int high = nums.size() - 1;

        while(low < high) {

            int mid = low + (high - low) / 2;

            if(nums[mid] > nums[high]) {
                low = mid + 1;
            }
            else {
                high = mid;
            }
        }

        return nums[low];
    }
};
```

---

# 🔄 Quick Revision

```text
Rotated Sorted Array
        ↓
low = 0
high = n - 1
        ↓
mid
        ↓
nums[mid] > nums[high]?
     /             \
   YES              NO
    ↓                ↓
minimum RIGHT    minimum LEFT/MID
    ↓                ↓
low = mid + 1    high = mid
        ↓
   low == high
        ↓
   nums[low]
```

---

# 🧠 One-Line Revision

```text
Rotated sorted array me minimum find karne ke liye nums[mid] ko nums[high] se compare karo; mid > high ho to right jao, warna mid ko include rakhte hue left jao.
```

---

# 🔥 Main Formula

```text
Rotated Sorted Array
        +
Compare nums[mid] with nums[high]
        ↓
mid > high → RIGHT
mid <= high → LEFT / MID
        ↓
low == high
        ↓
Minimum = nums[low]
```

---

# 📌 Pattern

```text
Binary Search
    ↓
Rotated Sorted Array
    ↓
Find Minimum
    ↓
LC 153
```
