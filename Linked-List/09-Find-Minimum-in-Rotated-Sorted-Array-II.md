# LeetCode 154 - Find Minimum in Rotated Sorted Array II

## 📌 Problem

Hume ek integer array `nums` diya hai jo originally **ascending sorted** tha.

Array ko kisi unknown position par rotate kiya gaya hai.

Is version me **duplicate elements allowed hain**.

Hume array ka:

```text id="q4m5t7"
minimum element
```

find karna hai.

---

# 🔹 Important Conditions

```text id="7hv28f"
1. Array originally sorted tha.
2. Array rotate hua hai.
3. Duplicate elements allowed hain.
4. Hume minimum value return karni hai.
```

Required approach:

```text id="s2x8k4"
Binary Search
```

Lekin duplicates ki wajah se ek extra case handle karna padega.

---

# 🔹 Example

```text id="rj3m9a"
nums = [2,2,2,0,1]
```

Minimum:

```text id="p9w2xk"
0
```

So:

```text id="5t5pc3"
Output = 0
```

---

# 🔹 Another Example

```text id="m4q7xz"
nums = [1,1,1,0,1]
```

Minimum:

```text id="y7v1sj"
0
```

---

# 🧠 LC 153 Se Difference

## LC 153

```text id="c8f3q1"
Find Minimum in Rotated Sorted Array
```

Duplicate elements nahi the.

Isliye sirf 2 cases:

```text id="0t8xhf"
nums[mid] > nums[high]
```

or:

```text id="7z1y9p"
nums[mid] < nums[high]
```

enough the.

---

## LC 154

Duplicates allowed hain.

So ek extra case bhi possible hai:

```text id="k6m3qr"
nums[mid] == nums[high]
```

Yehi LC 154 ka main new concept hai.

---

# 🔥 Main Idea

Hum maintain karenge:

```text id="d6m9cr"
low
mid
high
```

Har iteration me:

```cpp id="9d8jv5"
nums[mid]
```

ko:

```cpp id="h8r2m4"
nums[high]
```

se compare karenge.

Total 3 cases:

```text id="w5k2xj"
1. nums[mid] > nums[high]

2. nums[mid] < nums[high]

3. nums[mid] == nums[high]
```

---

# 🔥 Case 1 - `nums[mid] > nums[high]`

Example:

```text id="2d4sx8"
nums = [3,4,5,1,2]
```

Suppose:

```text id="k7y4p2"
mid = 2
```

Then:

```text id="m8t5c1"
nums[mid] = 5
nums[high] = 2
```

So:

```text id="q1z7mh"
5 > 2
```

True.

---

## Iska Meaning

`mid` se `high` tak sorted order break ho raha hai:

```text id="j8m5r2"
5 → 2
```

Rotation point aur minimum **right side** me hai.

Example:

```text id="7a4m9d"
[3,4,5 | 1,2]
        ↑
      break
```

Therefore:

```cpp id="g2h6q1"
low = mid + 1;
```

---

# 📌 Case 1 Formula

```text id="2p7k5x"
nums[mid] > nums[high]
        ↓
Minimum RIGHT
        ↓
low = mid + 1
```

---

# 🔥 Case 2 - `nums[mid] < nums[high]`

Example:

```text id="z6r3py"
nums = [5,1,2,3,4]
```

Suppose:

```text id="q8m2hs"
mid = 2
```

Then:

```text id="w5j6kd"
nums[mid] = 2
nums[high] = 4
```

So:

```text id="r4b8tc"
2 < 4
```

True.

---

## Iska Meaning

`mid` se `high` tak part sorted hai:

```text id="f1n6cz"
[2,3,4]
```

Minimum:

```text id="s9p4ya"
mid
```

par ho sakta hai.

Ya:

```text id="n5m2xr"
mid ke LEFT
```

me ho sakta hai.

Therefore `mid` ko eliminate nahi karna.

So:

```cpp id="e7q3wv"
high = mid;
```

---

# 📌 Case 2 Formula

```text id="t3x7pn"
nums[mid] < nums[high]
        ↓
Minimum LEFT / MID
        ↓
high = mid
```

---

# 🔥 Case 3 - `nums[mid] == nums[high]`

**Ye LC 154 ka most important case hai.**

Example:

```text id="k2z6wd"
nums = [2,2,2,0,1,2]
```

Suppose:

```text id="h7p3na"
mid = 2
high = 5
```

Values:

```text id="w4m8cx"
nums[mid] = 2
nums[high] = 2
```

Therefore:

```text id="q6r2vx"
nums[mid] == nums[high]
```

---

# ❓ Problem Kya Hai?

Ab hume ye decide nahi kar sakte ki minimum:

```text id="h2x7pm"
LEFT
```

me hai ya:

```text id="v6k3qa"
RIGHT
```

me.

Because dono boundary values same hain.

Example:

```text id="s4k1np"
[2,2,2,0,1,2]
     ↑       ↑
    mid     high
```

Sirf:

```text id="x8y5rm"
2 == 2
```

dekh kar direction determine nahi kar sakte.

---

# 🔥 Solution - `high--`

Jab:

```text id="s5r2vj"
nums[mid] == nums[high]
```

to hum ek duplicate element safely remove kar sakte hain.

So:

```cpp id="m7c4ax"
high--;
```

---

# ❓ `high--` Safe Kyu Hai?

Suppose:

```text id="p3m8yd"
nums[mid] == nums[high]
```

Dono ki value same hai.

Agar `high` wala element minimum hota, to `mid` par bhi same value present hai.

So minimum ki value lose nahi hogi.

Isliye ek occurrence remove karna safe hai.

Important:

```text id="s8v2kn"
Direction decide nahi kar rahe.
Sirf duplicate occurrence hata rahe hain.
```

---

# 📌 Case 3 Formula

```text id="q4y8mv"
nums[mid] == nums[high]
        ↓
Direction unclear
        ↓
Duplicate remove
        ↓
high--
```

---

# 🧠 All 3 Cases Together

```text id="m2j6tp"
nums[mid] > nums[high]
        ↓
Minimum RIGHT
        ↓
low = mid + 1
```

```text id="v7k3bx"
nums[mid] < nums[high]
        ↓
Minimum LEFT / MID
        ↓
high = mid
```

```text id="a9f5cq"
nums[mid] == nums[high]
        ↓
Cannot decide direction
        ↓
high--
```

---

# 💻 C++ Code

```cpp id="3zv7mk"
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

            else if(nums[mid] < nums[high]) {

                // Minimum mid ya left side me hai
                high = mid;
            }

            else {

                // nums[mid] == nums[high]
                // Direction clear nahi hai
                high--;
            }
        }

        return nums[low];
    }
};
```

---

# 🧠 Code Line By Line

## Step 1

```cpp id="q7n4mx"
int low = 0;
```

Search first index se start hogi.

---

## Step 2

```cpp id="a5k8pd"
int high = nums.size() - 1;
```

Last valid index:

```text id="f4v2nz"
n - 1
```

---

## Step 3

```cpp id="u1m6rx"
while(low < high)
```

Hum minimum ki position ko continuously narrow kar rahe hain.

Jab:

```text id="g8r3cs"
low == high
```

hoga, ek possible minimum position bachegi.

---

# 🔥 Why `while(low < high)`?

LC 153 ki tarah yahan bhi hum:

```text id="r9m4kv"
minimum ki exact position
```

tak search space reduce kar rahe hain.

End me:

```text id="p2s6yj"
low == high
```

and:

```cpp id="d5q8rx"
return nums[low];
```

correct answer deta hai.

---

# 🔄 Dry Run 1

Given:

```text id="b7n3qw"
nums = [2,2,2,0,1]
```

Array:

```text id="x4m8kp"
Index:  0 1 2 3 4
Value:  2 2 2 0 1
```

Initial:

```text id="m8v2cs"
low = 0
high = 4
```

---

## Iteration 1

```text id="t6q3zm"
mid = 0 + (4-0)/2
    = 2
```

Values:

```text id="y7r5bx"
nums[mid] = 2
nums[high] = 1
```

Compare:

```text id="n3k9fp"
2 > 1
```

True.

So:

```text id="v5m2qz"
Minimum RIGHT side me hai.
```

Update:

```cpp id="c8j4ry"
low = mid + 1;
```

Therefore:

```text id="e7p3ak"
low = 3
high = 4
```

---

## Iteration 2

```text id="m5z8cx"
mid = 3 + (4-3)/2
    = 3
```

Values:

```text id="y2q6rv"
nums[mid] = 0
nums[high] = 1
```

Compare:

```text id="n8v4kp"
0 > 1
```

False.

So:

```text id="p6c2mx"
0 < 1
```

Minimum mid ya left me hai.

```cpp id="g4r7vn"
high = mid;
```

Therefore:

```text id="k3x8bm"
low = 3
high = 3
```

Loop stop.

Answer:

```text id="r7m4qy"
nums[3] = 0
```

---

# ✅ Output

```text id="s9n2kv"
0
```

---

# 🔄 Dry Run 2 - Duplicate Case

Given:

```text id="n4p8xy"
nums = [1,1,1,0,1]
```

Initial:

```text id="q3m7vz"
low = 0
high = 4
```

---

## Iteration 1

```text id="w5c2kr"
mid = 2
```

Values:

```text id="j8m4px"
nums[mid] = 1
nums[high] = 1
```

So:

```text id="a6r2bn"
nums[mid] == nums[high]
```

Ye duplicate case hai.

Direction clear nahi hai.

So:

```cpp id="v7m3kc"
high--;
```

Therefore:

```text id="h5x9qz"
high = 3
```

Now:

```text id="p2k8lm"
low = 0
high = 3
```

---

## Iteration 2

```text id="q6v3wr"
mid = 1
```

Values:

```text id="s2m8kp"
nums[mid] = 1
nums[high] = 0
```

Compare:

```text id="z5r1xn"
1 > 0
```

True.

So minimum right side me:

```cpp id="e3k7vq"
low = mid + 1;
```

Therefore:

```text id="w6p2ya"
low = 2
high = 3
```

---

## Iteration 3

```text id="x8m4qk"
mid = 2
```

Values:

```text id="p7n3cr"
nums[mid] = 1
nums[high] = 0
```

Again:

```text id="b6r9vz"
1 > 0
```

So:

```text id="m4k2px"
low = mid + 1
low = 3
```

Now:

```text id="g8v5nc"
low = high = 3
```

Answer:

```text id="q2m7rx"
nums[3] = 0
```

---

# 🔥 Duplicate Kahin Bhi Ho Sakta Hai

Ye important hai.

Suppose:

```text id="8m3qvz"
nums = [2,2,3,4,1,2,2]
```

Duplicates hain:

```text id="w6k4px"
2,2
```

Lekin har iteration me `high--` nahi hoga.

`high--` **sirf tab** hoga jab current comparison:

```cpp id="s4r8kn"
nums[mid] == nums[high]
```

ho.

Otherwise:

```text id="a6p2mz"
>  → right
<  → left/mid
```

normal binary-search logic chalega.

---

# 🔥 Important Clarification

Ye galat assumption hai:

```text id="j5m8vx"
"Array me duplicate hai → high--"
```

Correct rule:

```text id="r2n7pk"
nums[mid] == nums[high]
        ↓
high--
```

Duplicate kahin aur hai to koi special action zaroori nahi.

Example:

```text id="z6q4mv"
nums = [1,2,2,3,0]
```

Agar:

```text id="n7m2xb"
nums[mid] = 2
nums[high] = 0
```

then:

```text id="h4v8qk"
2 > 0
```

So normal:

```cpp id="p3m7wr"
low = mid + 1;
```

---

# 🔥 Why Not `low++` in Equal Case?

Equal case:

```text id="m7q3vb"
nums[mid] == nums[high]
```

me hum generally:

```cpp id="s4n8kx"
high--;
```

karte hain.

Kyunki minimum potentially left side me bhi ho sakta hai.

`low++` karna left side ke possible minimum ko unnecessarily remove kar sakta hai.

So safe reduction:

```text id="p6m2vr"
high--
```

---

# 🔥 Why Not `high = mid` in Equal Case?

Agar:

```text id="x8q4mz"
nums[mid] == nums[high]
```

then direction clear nahi hai.

Agar directly:

```cpp id="k6p3vy"
high = mid;
```

kar diya, to search space unnecessarily shrink ho sakta hai without enough information.

Safe choice:

```cpp id="t9m4bx"
high--;
```

Sirf ek duplicate occurrence remove karo.

---

# 🧠 LC 153 vs LC 154

## LC 153

```text id="p7q2mz"
No duplicates
```

So:

```cpp id="r3m8vx"
if(nums[mid] > nums[high])
    low = mid + 1;
else
    high = mid;
```

---

## LC 154

```text id="x6k3qv"
Duplicates allowed
```

So:

```cpp id="h5m9zr"
if(nums[mid] > nums[high])
    low = mid + 1;

else if(nums[mid] < nums[high])
    high = mid;

else
    high--;
```

### Main Difference:

```text id="j8p4mz"
LC 153 → 2 cases

LC 154 → 3 cases
```

---

# 📊 Complexity

## Best / Average Type Behavior

Binary-search style reduction often gives:

```text id="v4m7qx"
O(log n)
```

---

## Worst Case

Duplicates ki wajah se repeatedly:

```cpp id="n2r6pk"
high--;
```

ho sakta hai.

Example:

```text id="c5m8vx"
[2,2,2,2,2,2,2,1,2]
```

Hume ek-ek duplicate remove karna pad sakta hai.

So worst case:

```text id="j7q3mz"
O(n)
```

---

# 💾 Space Complexity

Sirf:

```text id="r8m4vx"
low
high
mid
```

variables use ho rahe hain.

Therefore:

```text id="p3q7mz"
Space Complexity = O(1)
```

---

# 📊 Complexity Summary

```text id="m6v2qx"
Time Complexity
Best/Average → O(log n)
Worst Case   → O(n)

Space Complexity → O(1)
```

---

# 🧠 Pattern Recognition

Question me agar dikhe:

```text id="q5m8vx"
Rotated Sorted Array
+
Minimum
+
Duplicates allowed
```

Immediately think:

```text id="v3r7mz"
LC 154 Pattern
```

Then:

```text id="h6p2qx"
Compare nums[mid] with nums[high]
```

and remember **3 cases**.

---

# 🔥 Quick Revision

```text id="x4m7qz"
nums[mid] > nums[high]
        ↓
Minimum RIGHT
        ↓
low = mid + 1
```

```text id="p8r2mv"
nums[mid] < nums[high]
        ↓
Minimum LEFT / MID
        ↓
high = mid
```

```text id="k3v6qx"
nums[mid] == nums[high]
        ↓
Cannot decide direction
        ↓
high--
```

---

# 🧠 One-Line Revision

```text id="z7m4qx"
Rotated sorted array with duplicates me mid aur high compare karo; greater ho to right, smaller ho to left/mid, aur equal ho to direction unclear hone ki wajah se high-- karo.
```

---

# ⭐ Interview Explanation

Interviewer puche:

**"How is this different from LC 153?"**

Bolo:

```text id="v5q2mx"
The main difference is that duplicates are allowed in this version.

When nums[mid] is greater than nums[high], the minimum must be
on the right.

When nums[mid] is smaller than nums[high], the minimum can be at
mid or on the left.

But when nums[mid] equals nums[high], we cannot determine which
side contains the minimum because duplicate values hide the
rotation point.

So we safely reduce the search space by decrementing high.

Because duplicates can force us to remove one element at a time,
the worst-case time complexity becomes O(n), while the space
complexity remains O(1).
```

---

# ⭐ Interview Revision Code

```cpp id="f7m3qx"
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
            else if(nums[mid] < nums[high]) {
                high = mid;
            }
            else {
                high--;
            }
        }

        return nums[low];
    }
};
```

---

# 🔥 Main Formula

```text id="m7q4vx"
Rotated Sorted Array
        +
Duplicates
        ↓
Compare nums[mid] and nums[high]
        ↓
>  → RIGHT
<  → LEFT / MID
=  → high--
        ↓
low == high
        ↓
Minimum = nums[low]
```

---

# 📌 Pattern

```text id="p8m3qx"
Binary Search
    ↓
Rotated Sorted Array
    ↓
Find Minimum
    ↓
Duplicates
    ↓
LC 154
```
