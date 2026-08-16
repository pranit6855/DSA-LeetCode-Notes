# LeetCode 33 - Search in Rotated Sorted Array

## 📌 Problem

Hume ek integer array `nums` diya hai jo originally sorted tha, lekin kisi unknown point par rotate kar diya gaya.

Hume given `target` ka index find karna hai.

Agar target array me nahi hai:

```text id="j7wqpd"
-1
```

return karna hai.

### Important Conditions

```text id="c4kh4g"
1. Array originally sorted tha.
2. Array rotate hua hai.
3. Array me duplicate elements nahi hain.
4. Target ka index return karna hai.
5. Required time complexity = O(log n)
```

---

# 🔹 Example

```text id="k4fzzs"
nums = [4,5,6,7,0,1,2]
target = 0
```

Array:

```text id="8o2f26"
Index:  0 1 2 3 4 5 6
Value:  4 5 6 7 0 1 2
                  ↑
                target
```

Target `0` ka index:

```text id="8sbh3h"
4
```

So:

```text id="szq3mi"
Output = 4
```

---

# 🔹 Example 2

```text id="js57qa"
nums = [4,5,6,7,0,1,2]
target = 3
```

`3` array me present nahi hai.

So:

```text id="qomv5d"
Output = -1
```

---

# 🧠 Why Normal Binary Search Does Not Directly Work?

Normal sorted array:

```text id="h9j2gq"
[1,2,3,4,5,6,7]
```

me:

```text
nums[mid] < target
```

ho to directly right ja sakte hain.

Lekin rotated array:

```text id="2l7f8s"
[4,5,6,7,0,1,2]
```

poora sorted nahi hai.

Isliye sirf:

```text id="q2p7y8"
nums[mid] < target
```

dekhkar left/right decide nahi kar sakte.

---

# 🔥 Main Observation

Har iteration me current search space ke:

```text id="w53q5g"
LEFT HALF
```

ya:

```text id="ef1yja"
RIGHT HALF
```

me se **at least one half sorted hota hai**.

Example:

```text id="l5b8m1"
[4,5,6,7,0,1,2]
 ↑     ↑       ↑
low   mid     high
```

Left half:

```text id="0a2lv8"
[4,5,6,7]
```

sorted hai.

Right half:

```text id="w01l5j"
[0,1,2]
```

bhi sorted hai.

Hum pehle identify karenge:

```text id="f7bqu8"
Kaunsa half sorted hai?
```

Then check karenge:

```text id="0zvpyh"
Target us sorted half ke range me hai ya nahi?
```

---

# 🔥 Overall Approach

```text id="m7xg9e"
low, mid, high
       ↓
target == nums[mid] ?
       ↓
YES → return mid
       ↓
NO
       ↓
Which half is sorted?
       ↓
LEFT SORTED / RIGHT SORTED
       ↓
Target sorted half ke range me?
       ↓
YES → us half me search
NO  → opposite half me search
```

---

# 🔹 Step 1 - Target Check

Sabse pehle:

```cpp id="e8n3hs"
if(nums[mid] == target) {
    return mid;
}
```

Agar middle value target hai, answer mil gaya.

---

# 🔹 Step 2 - Left Half Sorted Hai?

Check:

```cpp id="pq4r6n"
if(nums[low] <= nums[mid])
```

Agar true:

```text id="x6a6s8"
LEFT HALF SORTED
```

Example:

```text id="7x6g1n"
[4,5,6,7,0,1,2]
 ↑     ↑
low   mid
```

Values:

```text id="a7p9y0"
4 <= 7
```

True.

So:

```text id="t4x6te"
[4,5,6,7]
```

sorted half hai.

---

# 🔥 Step 3 - Target Left Sorted Half Me Hai?

Condition:

```cpp id="5k8f1q"
if(nums[low] <= target && target < nums[mid])
```

Meaning:

```text id="5hrx3z"
target >= left boundary
AND
target < mid
```

Agar true:

```text id="9m2ydj"
target left sorted half ke andar hai.
```

So right part eliminate:

```cpp id="0g2w5j"
high = mid - 1;
```

---

# 🔹 Example

```text id="m7a5h3"
nums = [4,5,6,7,0,1,2]
target = 5
```

Suppose:

```text id="j3ucp5"
low = 0
mid = 3
```

Then:

```text id="da1tsr"
nums[low] = 4
nums[mid] = 7
```

Check:

```text id="q5y3km"
4 <= 5 < 7
```

True.

Target left sorted half me hai:

```text id="jgd8my"
[4,5,6,7]
```

So:

```text id="4f7br0"
high = mid - 1
```

---

# 🔥 If Target Is NOT In Left Sorted Half

Suppose:

```text id="bdjpve"
nums = [4,5,6,7,0,1,2]
target = 0
```

Condition:

```text id="6l9q48"
4 <= 0 < 7
```

False.

Target left sorted part me nahi hai.

So target right side me hoga.

```cpp id="d4s0hz"
low = mid + 1;
```

---

# 🔹 Step 4 - Right Half Sorted

Agar:

```cpp id="d7e0f2"
nums[low] <= nums[mid]
```

false hai, then:

```text id="v2o8hv"
RIGHT HALF SORTED
```

Example:

```text id="0yxz0v"
[6,7,0,1,2,4,5]
 ↑     ↑       ↑
low   mid     high
```

Suppose:

```text id="cn6onp"
nums[low] = 6
nums[mid] = 1
```

Because:

```text id="5m9c7r"
6 <= 1
```

false.

So right half:

```text id="p0c2fj"
[1,2,4,5]
```

sorted hai.

---

# 🔥 Step 5 - Target Right Sorted Half Me Hai?

Condition:

```cpp id="q0d1pn"
if(nums[mid] < target && target <= nums[high])
```

Meaning:

```text id="j2o2xm"
target > mid
AND
target <= high
```

Agar true:

```text id="zz5shb"
target right sorted half me hai.
```

So:

```cpp id="3h9h4w"
low = mid + 1;
```

---

# 🔹 Example

```text id="9h3x3z"
nums = [6,7,0,1,2,4,5]
target = 4
```

Suppose:

```text id="7n94at"
mid = 3
```

Then:

```text id="4i8fd6"
nums[mid] = 1
nums[high] = 5
```

Check:

```text id="x9c4kg"
1 < 4 <= 5
```

True.

Target right sorted half me hai:

```text id="2e0d2j"
[1,2,4,5]
```

So:

```text id="v7ud8f"
low = mid + 1
```

---

# 🔥 If Target Is NOT In Right Sorted Half

Then target left side me hoga.

So:

```cpp id="w0b03o"
high = mid - 1;
```

---

# 📊 Complete Decision Logic

```text id="p1r9tu"
nums[mid] == target
        ↓
      return mid
```

Otherwise:

```text id="i0x0n3"
nums[low] <= nums[mid]
        ↓
   LEFT SORTED
        ↓
nums[low] <= target < nums[mid] ?
      /                 \
    YES                  NO
     ↓                    ↓
high = mid - 1      low = mid + 1
```

Otherwise:

```text id="2i1l9v"
RIGHT SORTED
        ↓
nums[mid] < target <= nums[high] ?
      /                 \
    YES                  NO
     ↓                    ↓
low = mid + 1       high = mid - 1
```

---

# 💻 C++ Code

```cpp id="wq3b8v"
class Solution {
public:
    int search(vector<int>& nums, int target) {

        int low = 0;
        int high = nums.size() - 1;

        while(low <= high) {

            int mid = low + (high - low) / 2;

            // Target found
            if(nums[mid] == target) {
                return mid;
            }

            // Left half is sorted
            if(nums[low] <= nums[mid]) {

                // Target lies in left sorted half
                if(nums[low] <= target && target < nums[mid]) {
                    high = mid - 1;
                }

                // Target lies in right half
                else {
                    low = mid + 1;
                }
            }

            // Right half is sorted
            else {

                // Target lies in right sorted half
                if(nums[mid] < target && target <= nums[high]) {
                    low = mid + 1;
                }

                // Target lies in left half
                else {
                    high = mid - 1;
                }
            }
        }

        return -1;
    }
};
```

---

# 🧠 Code Line By Line

## Step 1 - `low`

```cpp id="ehg8yg"
int low = 0;
```

Search first index se start hogi.

---

## Step 2 - `high`

```cpp id="8qdo8d"
int high = nums.size() - 1;
```

Last valid index:

```text id="e0uhk7"
size - 1
```

hota hai.

---

# 🔥 Why `while(low <= high)`?

Yahan hume target ko directly search karna hai.

Suppose:

```text id="e0w4ah"
nums = [1,3]
target = 3
```

Initially:

```text id="x7b9mv"
low = 0
high = 1
```

First iteration:

```text id="x3l55d"
mid = 0
nums[mid] = 1
```

Target bada hai.

So:

```text id="y5lyjh"
low = 1
```

Now:

```text id="r9eonp"
low = 1
high = 1
```

Index `1` par target hai:

```text id="se2jhg"
nums[1] = 3
```

Isliye `low == high` case ko check karna zaroori hai.

Therefore:

```cpp id="66v4bp"
while(low <= high)
```

use karte hain.

---

# 🔥 LC 153 Me `<` Kyu Tha?

LC 153 me:

```text id="xyq50p"
Find Minimum
```

kar rahe the.

Search space ko ek single possible answer tak narrow kar rahe the.

End me:

```text id="m0snyp"
low == high
```

hote hi answer fixed tha.

So:

```cpp id="ijzlxw"
while(low < high)
```

---

# 🔥 LC 33 Me `<=`

LC 33 me:

```text id="12i7ua"
Target Search
```

kar rahe hain.

`low == high` par ek actual element abhi check hona baaki ho sakta hai.

So:

```cpp id="g3tswr"
while(low <= high)
```

---

# 🔄 Complete Dry Run

Given:

```text id="q0kg85"
nums = [4,5,6,7,0,1,2]
target = 0
```

Array:

```text id="q5zfea"
Index:  0 1 2 3 4 5 6
Value:  4 5 6 7 0 1 2
```

---

## Iteration 1

Initial:

```text id="w6djxm"
low = 0
high = 6
```

Middle:

```text id="m8gj5k"
mid = 3
```

Value:

```text id="e9t6nz"
nums[mid] = 7
```

Target:

```text id="v1e8dh"
0
```

Check:

```text id="6s6e9t"
7 == 0 ? No
```

---

### Left Half Sorted?

```text id="thz6rm"
nums[low] <= nums[mid]

4 <= 7
```

Yes.

So left half:

```text id="nsjj6c"
[4,5,6,7]
```

sorted hai.

---

### Target Left Me?

```text id="c1j4ih"
4 <= 0 < 7
```

False.

Therefore:

```text id="f7q2t0"
target right side me hai.
```

Update:

```text id="n2c6n7"
low = mid + 1
    = 4
```

Now:

```text id="o9iafp"
low = 4
high = 6
```

---

# Iteration 2

```text id="y4ft86"
mid = 4 + (6-4)/2
    = 5
```

Values:

```text id="k57t0p"
nums[mid] = 1
```

Target:

```text id="9x04fj"
0
```

Check:

```text id="d6d4s8"
1 == 0 ? No
```

---

### Left Half Sorted?

```text id="g7h3ny"
nums[low] <= nums[mid]

0 <= 1
```

Yes.

So:

```text id="x8jyj1"
[0,1]
```

sorted hai.

---

### Target Left Me?

```text id="r0d3hn"
0 <= 0 < 1
```

True.

So target left side me hai.

```text id="upgqg1"
high = mid - 1
    = 4
```

Now:

```text id="zyw3k4"
low = 4
high = 4
```

---

# Iteration 3

```text id="v9uq1t"
mid = 4
```

```text id="2q2u1f"
nums[4] = 0
```

Target:

```text id="7wqg3m"
0
```

So:

```text id="7bbxpm"
nums[mid] == target
```

True.

Return:

```text id="0hx9fx"
4
```

---

# ✅ Final Answer

```text id="g6p2pq"
4
```

---

# 🔥 Second Dry Run

```text id="4y9eh0"
nums = [6,7,0,1,2,4,5]
target = 4
```

Initial:

```text id="m4m8ad"
low = 0
high = 6
```

### Iteration 1

```text id="2v7q1w"
mid = 3
nums[mid] = 1
```

`1 != 4`.

Check left sorted:

```text id="0m2d0l"
nums[low] <= nums[mid]

6 <= 1
```

False.

So:

```text id="iyw6m7"
RIGHT HALF SORTED
```

Right half:

```text id="8a7b0q"
[1,2,4,5]
```

Check target:

```text id="gj8n9j"
1 < 4 <= 5
```

True.

Therefore:

```text id="18e9ar"
low = mid + 1
    = 4
```

---

### Iteration 2

Now:

```text id="p2d7n2"
low = 4
high = 6
```

```text id="zj6qkp"
mid = 5
nums[mid] = 4
```

Target found.

Return:

```text id="tkpo4h"
5
```

---

# 🔥 Why Sorted Half Identify Karna Zaroori Hai?

Example:

```text id="3n43cx"
[4,5,6,7,0,1,2]
```

Agar:

```text id="bo7qdo"
mid = 3
nums[mid] = 7
target = 0
```

Sirf ye dekhkar:

```text id="a8t87m"
7 > 0
```

normal binary search bol sakta hai:

```text id="oazm06"
LEFT
```

But actual target right side me hai.

Isliye pehle:

```text id="ya9dqa"
LEFT SORTED?
```

identify karna important hai.

---

# 🧠 Important Intuition

Rotated array me:

```text id="2e8u9y"
At least one half sorted hota hai.
```

Sorted half milte hi hum normal sorted-array logic use kar sakte hain.

Example:

```text id="j4v7k3"
[4,5,6,7 | 0,1,2]
```

Agar left sorted hai:

```text id="r0p3o6"
4 <= target < 7
```

to target left me.

Otherwise right me.

---

# ⚠️ Common Mistakes

## 1. `while(low < high)`

LC 33 me wrong for standard implementation:

```cpp id="w4y5wx"
while(low < high)
```

Correct:

```cpp id="f6w9fj"
while(low <= high)
```

Because `low == high` par last element check karna pad sakta hai.

---

## 2. `high = mid`

Yahan wrong:

```cpp id="pj7b2h"
high = mid;
```

Target-search problem hai aur `mid` already check ho chuka hai.

Correct:

```cpp id="5m1c8a"
high = mid - 1;
```

---

## 3. `low = mid`

Wrong:

```cpp id="d73pjk"
low = mid;
```

Correct:

```cpp id="8unx5p"
low = mid + 1;
```

---

## 4. Left Sorted Condition Galat Karna

Correct:

```cpp id="o6m2g7"
nums[low] <= nums[mid]
```

Ye batata hai:

```text id="e8r6ox"
LEFT HALF SORTED
```

---

## 5. Target Range Wrong Check

Left sorted:

```cpp id="z1w0o6"
nums[low] <= target && target < nums[mid]
```

Right sorted:

```cpp id="b0t7p6"
nums[mid] < target && target <= nums[high]
```

Boundaries carefully use karna.

---

## 6. Target Milte Hi Search Continue Karna

Correct:

```cpp id="v6kwm5"
if(nums[mid] == target) {
    return mid;
}
```

Target mil gaya to directly return.

---

# 🧠 LC 153 vs LC 33

Dono rotated-array problems hain, but objective different hai.

## LC 153

```text id="unl0rt"
Find Minimum
```

Pattern:

```text id="4mpw7u"
nums[mid] > nums[high]
→ right

otherwise
→ left/mid
```

And:

```cpp id="9pgoq8"
while(low < high)
```

---

## LC 33

```text id="yq5h1b"
Find Target
```

Pattern:

```text id="g8v12e"
Find sorted half
        ↓
Check target range
        ↓
Choose side
```

And:

```cpp id="f8r7jf"
while(low <= high)
```

---

# 🔥 Pattern Recognition

Question me agar dikhe:

```text id="w0q4he"
Sorted Array
+
Rotated
+
Unique Elements
+
Search Target
+
O(log n)
```

Immediately:

```text id="o5dz5t"
Rotated Sorted Array Search
```

Steps:

```text id="yp4zda"
1. Find mid
2. Check target
3. Identify sorted half
4. Check target range
5. Eliminate one half
```

---

# ⏱️ Time Complexity

Har iteration me approximately half search space eliminate hota hai.

Therefore:

```text id="y8ok0w"
Time Complexity = O(log n)
```

---

# 💾 Space Complexity

Sirf:

```text id="g7t7ed"
low
high
mid
```

variables.

No extra data structure.

Therefore:

```text id="ppnqgk"
Space Complexity = O(1)
```

---

# 📊 Complexity

```text id="0wlq3g"
Time Complexity  → O(log n)

Space Complexity → O(1)
```

---

# 🧠 Interview Explanation

Interviewer puche:

**"How do you search in a rotated sorted array?"**

Bolna:

```text id="m8g0ym"
Since the array is rotated, I cannot directly apply normal
binary search.

However, at every step at least one half of the current search
space is sorted.

I first calculate mid and check whether nums[mid] is the target.

If not, I identify which half is sorted.

If the left half is sorted, I check whether the target lies
between nums[low] and nums[mid]. If yes, I search left;
otherwise I search right.

Similarly, if the right half is sorted, I check whether the
target lies between nums[mid] and nums[high].

This allows me to eliminate half of the search space at every
iteration, giving O(log n) time and O(1) space.
```

---

# 🔄 Quick Revision

```text id="r3ae9c"
Rotated Sorted Array
        ↓
      Find mid
        ↓
nums[mid] == target?
    ↓ YES
 return mid
        ↓ NO
Which half is sorted?
        ↓
┌───────────────┴───────────────┐
↓                               ↓
LEFT SORTED                  RIGHT SORTED
↓                               ↓
Target left range?            Target right range?
 /       \                      /       \
YES      NO                    YES      NO
 ↓        ↓                     ↓        ↓
high    low                    low     high
=mid-1  =mid+1                =mid+1   =mid-1
```

---

# 🧠 One-Line Revision

```text id="e1n4w2"
Rotated sorted array me target search karne ke liye pehle sorted half identify karo, phir check karo target us sorted half ke range me hai ya nahi.
```

---

# ⭐ Interview Revision Code

```cpp id="7h1n5d"
class Solution {
public:
    int search(vector<int>& nums, int target) {

        int low = 0;
        int high = nums.size() - 1;

        while(low <= high) {

            int mid = low + (high - low) / 2;

            if(nums[mid] == target) {
                return mid;
            }

            if(nums[low] <= nums[mid]) {

                if(nums[low] <= target && target < nums[mid]) {
                    high = mid - 1;
                }
                else {
                    low = mid + 1;
                }
            }

            else {

                if(nums[mid] < target && target <= nums[high]) {
                    low = mid + 1;
                }
                else {
                    high = mid - 1;
                }
            }
        }

        return -1;
    }
};
```

---

# 🔥 Main Formula

```text id="irj89k"
Rotated Sorted Array
       +
Find Sorted Half
       +
Check Target Range
       +
Eliminate One Half
       ↓
Binary Search
       ↓
O(log n)
```

### Remember:

```text id="n4t1w8"
LEFT SORTED:
nums[low] <= nums[mid]
```

```text id="k5q9ub"
TARGET LEFT:
nums[low] <= target < nums[mid]
```

```text id="8kn5jy"
RIGHT SORTED:
left half sorted nahi hai
```

```text id="h9g4zq"
TARGET RIGHT:
nums[mid] < target <= nums[high]
```

---

# 📌 Pattern

```text id="s3vplq"
Binary Search
    ↓
Rotated Sorted Array
    ↓
Find Sorted Half
    ↓
Search Target
    ↓
LC 33
```
