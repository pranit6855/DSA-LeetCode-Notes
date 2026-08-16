# LeetCode 162 - Find Peak Element

## 📌 Problem

Hume ek integer array `nums` diya gaya hai.

Hume kisi bhi **peak element** ka index return karna hai.

### Peak Element Kya Hai?

Peak element wo element hai jo apne adjacent elements se bada ho.

For an index `i`:

```text
nums[i] > nums[i-1]
AND
nums[i] > nums[i+1]
```

hona chahiye.

Question me **koi bhi valid peak** return kar sakte hain.

---

# 🔹 Example

```text
nums = [1,2,3,1]
```

Array:

```text
Index:  0 1 2 3
Value:  1 2 3 1
```

Yahan:

```text
1 < 2 < 3 > 1
        ↑
      peak
```

So:

```text
Output = 2
```

---

# 🔹 Another Example

```text
nums = [1,2,1,3,5,6,4]
```

Is array me multiple peaks hain:

```text
1 < 2 > 1
```

Index:

```text
1
```

Aur:

```text
1 < 3 < 5 < 6 > 4
                ↑
              peak
```

Index:

```text
5
```

Question me koi bhi peak valid hai.

So:

```text
Output can be 1
OR
Output can be 5
```

depending on the binary search path.

---

# 🧠 Important Observation

Usually Binary Search sorted array par use hota hai.

But is problem me array:

```text
NOT SORTED
```

hai.

Phir bhi Binary Search possible hai.

Why?

Because hum elements ki exact value search nahi kar rahe.

Hum **slope / direction** dekh rahe hain.

Har step me compare karenge:

```cpp
nums[mid]
```

with:

```cpp
nums[mid + 1]
```

---

# 🔥 Main Idea

Do cases honge.

### Case 1

```cpp
nums[mid] < nums[mid + 1]
```

Meaning:

```text
mid → mid+1
```

values increase ho rahi hain.

Example:

```text
2 < 3
```

Slope:

```text
      /
     /
    /
```

Yani hum **upar ja rahe hain**.

Agar slope increase ho rahi hai, to peak right side me guaranteed hai.

So:

```cpp
low = mid + 1;
```

---

### Case 2

```cpp
nums[mid] > nums[mid + 1]
```

Meaning:

```text
mid → mid+1
```

values decrease ho rahi hain.

Example:

```text
3 > 1
```

Slope:

```text
\
 \
  \
```

Yani hum peak cross kar chuke ho sakte hain.

Peak:

```text
mid
```

par ho sakta hai,

ya:

```text
mid ke LEFT
```

me ho sakta hai.

So:

```cpp
high = mid;
```

---

# 📊 Main Logic

```text
nums[mid] < nums[mid+1]
        ↓
     Slope UP
        ↓
   Peak RIGHT
        ↓
   low = mid + 1
```

```text
nums[mid] > nums[mid+1]
        ↓
    Slope DOWN
        ↓
 Peak LEFT or MID
        ↓
    high = mid
```

Ye is problem ka main pattern hai.

---

# 💻 C++ Code

```cpp
class Solution {
public:
    int findPeakElement(vector<int>& nums) {

        int low = 0;
        int high = nums.size() - 1;

        while(low < high) {

            int mid = low + (high - low) / 2;

            if(nums[mid] < nums[mid + 1]) {

                // Slope increasing
                // Peak right side me hai
                low = mid + 1;
            }

            else {

                // Slope decreasing
                // Peak mid ya left side me hai
                high = mid;
            }
        }

        return low;
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

Search last index tak hogi.

---

## Step 3 - Loop

```cpp
while(low < high)
```

Hum search space ko continuously chhota karenge.

End me:

```text
low == high
```

ho jayega.

Tab sirf ek possible peak index bachega.

---

# 🔥 Why `while(low < high)`?

Suppose:

```text
low = 2
high = 2
```

Sirf ek element bacha hai.

Us point par answer fix ho chuka hai.

Isliye loop:

```cpp
while(low < high)
```

use karta hai.

---

# 🔥 Why Compare `mid` and `mid+1`?

Hume ye pata lagana hai ki slope:

```text
UP
```

ja rahi hai ya:

```text
DOWN
```

So:

```cpp
nums[mid] < nums[mid + 1]
```

check karke direction pata chal jati hai.

---

# 🔥 Case 1 Deep Understanding

Suppose:

```text
nums = [1,2,3,4,5]
```

Aur:

```text
mid = 2
```

Then:

```text
nums[mid] = 3
nums[mid+1] = 4
```

So:

```text
3 < 4
```

Slope increasing hai:

```text
1 2 3 4 5
    ↑ ↑
   mid
```

Agar hum right jaate hain, eventually ya to:

```text
peak
```

milega, ya array ke end par peak milega.

So:

```cpp
low = mid + 1;
```

safe hai.

---

# 🔥 Case 2 Deep Understanding

Suppose:

```text
nums = [1,3,5,4,2]
```

Aur:

```text
mid = 2
```

Then:

```text
nums[mid] = 5
nums[mid+1] = 4
```

So:

```text
5 > 4
```

Slope decrease ho rahi hai:

```text
1 3 5 4 2
    ↑ ↓
   peak
```

Iska matlab peak:

```text
mid
```

par ho sakta hai.

Isliye `mid` ko remove nahi kar sakte.

Correct:

```cpp
high = mid;
```

Not:

```cpp
high = mid - 1;
```

---

# 🔥 Why `high = mid`, Not `mid - 1`?

Example:

```text
nums = [1,3,2]
```

Initial:

```text
low = 0
high = 2
mid = 1
```

Values:

```text
nums[mid] = 3
nums[mid+1] = 2
```

So:

```text
3 > 2
```

Peak exactly:

```text
index 1
```

par hai.

Agar:

```cpp
high = mid - 1;
```

karoge:

```text
high = 0
```

to index `1` ko remove kar doge.

Wrong.

Therefore:

```cpp
high = mid;
```

---

# 🔥 Why `low = mid + 1`?

Suppose:

```text
nums[mid] < nums[mid+1]
```

To:

```text
nums[mid]
```

peak nahi ho sakta.

Because uska right neighbour usse bada hai.

Example:

```text
2 < 3
```

So `mid` ko safely eliminate kar sakte hain.

Therefore:

```cpp
low = mid + 1;
```

---

# 🔄 Complete Dry Run

Given:

```text
nums = [1,2,3,1]
```

Array:

```text
Index:  0 1 2 3
Value:  1 2 3 1
```

---

## Initial

```text
low = 0
high = 3
```

---

## Iteration 1

Calculate:

```text
mid = 0 + (3-0)/2
    = 1
```

Values:

```text
nums[mid] = 2
nums[mid+1] = 3
```

Compare:

```text
2 < 3
```

True.

Slope:

```text
UP
```

So peak right side me hai.

```cpp
low = mid + 1;
```

Therefore:

```text
low = 2
high = 3
```

---

## Iteration 2

Now:

```text
mid = 2 + (3-2)/2
    = 2
```

Values:

```text
nums[mid] = 3
nums[mid+1] = 1
```

Compare:

```text
3 < 1
```

False.

Therefore:

```text
3 > 1
```

Slope:

```text
DOWN
```

Peak `mid` ya left me hai.

So:

```cpp
high = mid;
```

Therefore:

```text
low = 2
high = 2
```

Now:

```text
low == high
```

Loop stop.

Return:

```cpp
return low;
```

So:

```text
Output = 2
```

---

# 🔥 Second Dry Run

```text
nums = [1,2,1,3,5,6,4]
```

Initial:

```text
low = 0
high = 6
```

### Iteration 1

```text
mid = 3
```

Values:

```text
nums[3] = 3
nums[4] = 5
```

```text
3 < 5
```

Slope UP.

So:

```text
low = 4
```

---

### Iteration 2

```text
low = 4
high = 6

mid = 5
```

Values:

```text
nums[5] = 6
nums[6] = 4
```

```text
6 > 4
```

Slope DOWN.

So:

```text
high = 5
```

---

### Iteration 3

```text
low = 4
high = 5

mid = 4
```

Values:

```text
nums[4] = 5
nums[5] = 6
```

```text
5 < 6
```

Slope UP.

So:

```text
low = 5
```

Now:

```text
low = high = 5
```

Return:

```text
5
```

And:

```text
nums[5] = 6
```

6 is a peak because:

```text
5 < 6 > 4
```

---

# 🧠 Mountain Analogy

Array ko mountain ki tarah imagine karo.

## Increasing Slope

```text
nums[mid] < nums[mid+1]

       /
      /
     /
```

Tum upar ja rahe ho.

Peak:

```text
RIGHT
```

---

## Decreasing Slope

```text
nums[mid] > nums[mid+1]

    \
     \
      \
```

Tum neeche aa rahe ho.

Peak:

```text
LEFT or MID
```

---

# 🔥 Core Formula

```text
UP → RIGHT
DOWN → LEFT / MID
```

Code:

```cpp
if(nums[mid] < nums[mid + 1]) {
    low = mid + 1;
}
else {
    high = mid;
}
```

---

# 🧠 Why Binary Search Works Even Though Array Is Not Sorted?

Ye important interview question hai.

Array sorted nahi hai, but hume:

```text
exact target
```

search nahi karna.

Hum:

```text
slope direction
```

use kar rahe hain.

Har step me:

```text
UP
```

milne par right guaranteed safe hai.

Aur:

```text
DOWN
```

milne par left/mid guaranteed safe hai.

Isliye half search space eliminate kar sakte hain.

---

# 🔥 Normal Binary Search vs Peak Binary Search

## Normal Binary Search

```text
Sorted Array
     ↓
Compare nums[mid] with target
     ↓
Left / Right
```

## Peak Search

```text
Unsorted Array
     ↓
Compare nums[mid] with nums[mid+1]
     ↓
Check slope
     ↓
UP → RIGHT
DOWN → LEFT/MID
```

---

# ⚠️ Common Mistakes

## 1. `high = mid - 1`

Wrong:

```cpp
high = mid - 1;
```

Correct:

```cpp
high = mid;
```

Because `mid` itself can be peak.

---

## 2. `low = mid`

Wrong:

```cpp
low = mid;
```

Correct:

```cpp
low = mid + 1;
```

Because if:

```text
nums[mid] < nums[mid+1]
```

then `mid` definitely peak nahi hai.

---

## 3. `mid == target` Jaisa Kuch Karna

Ye target searching problem nahi hai.

Ye:

```text
Peak searching
```

problem hai.

So:

```cpp
nums[mid] == target
```

check nahi karna.

---

## 4. Global Maximum Dhundhna

Question global maximum nahi pooch raha.

Example:

```text
[1,2,1,3,5,6,4]
```

Peaks:

```text
2
6
```

dono valid hain.

So hume sirf:

```text
ANY peak
```

chahiye.

---

# ⏱️ Time Complexity

Har iteration me search space roughly half hota hai.

Therefore:

```text
Time Complexity = O(log n)
```

---

# 💾 Space Complexity

Extra data structure use nahi hota.

Only:

```text
low
high
mid
```

variables.

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

# 🔥 Pattern Recognition

Question me agar dikhe:

```text
Find Peak
+
O(log n)
```

ya:

```text
Array
+
Adjacent comparison
+
Peak
```

Think:

```text
Binary Search on Slope
```

Main comparison:

```cpp
nums[mid] < nums[mid+1]
```

---

# 🧠 Interview Explanation

Interviewer puche:

**"How can you use binary search on an unsorted array?"**

Bolna:

```text
The array is not sorted, but I can still use binary search because
I am looking at the slope between nums[mid] and nums[mid + 1].

If nums[mid] < nums[mid + 1], the slope is increasing, so a peak
must exist on the right side.

Otherwise, the slope is decreasing, so a peak can be at mid or
somewhere on the left side.

Therefore, I can eliminate half of the search space in every
iteration.

The time complexity is O(log n) and the space complexity is O(1).
```

---

# 🔄 Quick Revision

```text
Find Peak
    ↓
low = 0
high = n - 1
    ↓
mid
    ↓
nums[mid] < nums[mid+1] ?
      /              \
    YES               NO
     ↓                 ↓
  Slope UP          Slope DOWN
     ↓                 ↓
 Peak RIGHT       Peak LEFT/MID
     ↓                 ↓
low = mid + 1     high = mid
     ↓                 ↓
       low == high
            ↓
       return low
```

---

# 🧠 One-Line Revision

```text
Agar nums[mid] < nums[mid+1] hai to hum increasing slope par hain aur peak right me hai; warna peak mid ya left me hai.
```

---

# ⭐ Interview Revision Code

```cpp
class Solution {
public:
    int findPeakElement(vector<int>& nums) {

        int low = 0;
        int high = nums.size() - 1;

        while(low < high) {

            int mid = low + (high - low) / 2;

            if(nums[mid] < nums[mid + 1]) {

                low = mid + 1;
            }

            else {

                high = mid;
            }
        }

        return low;
    }
};
```

---

# 🔥 Main Formula

```text
Peak Element
      +
Compare mid and mid+1
      ↓
UP → RIGHT
DOWN → LEFT/MID
      ↓
Binary Search
      ↓
low == high
      ↓
Peak Index
```

### Remember:

```text
nums[mid] < nums[mid+1]
        ↓
low = mid + 1
```

```text
nums[mid] > nums[mid+1]
        ↓
high = mid
```

---

# 📌 Pattern

```text
Binary Search
    ↓
Binary Search on Slope
    ↓
Find Peak Element
    ↓
LC 162
```
