# LeetCode 852 - Peak Index in a Mountain Array

## 📌 Problem

Hume ek **mountain array** diya gaya hai.

Mountain array ka structure:

```text
strictly increasing
        ↓
      peak
        ↓
strictly decreasing
```

Hume mountain array ke **peak element ka index** return karna hai.

---

# 🔹 What Is a Mountain Array?

Mountain array me:

```text
arr[0] < arr[1] < arr[2] < ... < arr[peak]
```

aur peak ke baad:

```text
arr[peak] > arr[peak+1] > arr[peak+2] > ...
```

hota hai.

Example:

```text
arr = [0,2,3,4,3,2,1]
```

Structure:

```text
0 < 2 < 3 < 4 > 3 > 2 > 1
          ↑
        peak
```

Peak:

```text
4
```

Index:

```text
3
```

Therefore:

```text
Output = 3
```

---

# 🔹 Example

```text
Input:
arr = [0,2,3,4,3,2,1]

Output:
3
```

Array with indexes:

```text
Index:  0 1 2 3 4 5 6
Value:  0 2 3 4 3 2 1
              ↑
            peak
```

---

# 🧠 Main Observation

Array mountain form me hai:

```text
Increasing → Peak → Decreasing
```

To hume sirf ye pata karna hai ki hum abhi:

```text
Increasing part
```

me hain ya:

```text
Decreasing part
```

me.

Iske liye compare karenge:

```cpp
arr[mid]
```

with:

```cpp
arr[mid + 1]
```

---

# 🔥 Main Binary Search Logic

## Case 1 - Increasing Slope

Agar:

```cpp
arr[mid] < arr[mid + 1]
```

to:

```text
mid → mid+1
```

values increase ho rahi hain.

Example:

```text
2 < 3
```

Matlab hum abhi peak ke **left side** par hain.

Visualization:

```text
        /
       /
      /
     ↑ ↑
    mid mid+1
```

Peak right side me hai.

So:

```cpp
low = mid + 1;
```

---

# 🔥 Case 2 - Decreasing Slope

Agar:

```cpp
arr[mid] > arr[mid + 1]
```

to:

```text
mid → mid+1
```

values decrease ho rahi hain.

Example:

```text
4 > 3
```

Matlab hum peak ko cross kar chuke hain ya `mid` khud peak ho sakta hai.

Visualization:

```text
     \
      \
       \
      ↑ ↑
     mid mid+1
```

Peak:

```text
mid
```

par ho sakta hai ya left side me.

So:

```cpp
high = mid;
```

---

# 📊 Main Pattern

```text
arr[mid] < arr[mid+1]
        ↓
Increasing Slope
        ↓
Peak RIGHT
        ↓
low = mid + 1
```

```text
arr[mid] > arr[mid+1]
        ↓
Decreasing Slope
        ↓
Peak LEFT or MID
        ↓
high = mid
```

---

# 🔥 Why `high = mid`?

Ye bahut important hai.

Suppose:

```text
arr = [1,3,2]
```

Initial:

```text
low = 0
high = 2
mid = 1
```

Values:

```text
arr[mid] = 3
arr[mid+1] = 2
```

So:

```text
3 > 2
```

Peak exactly:

```text
index = 1
```

hai.

Agar hum:

```cpp
high = mid - 1;
```

kar denge to index `1` remove ho jayega.

So correct:

```cpp
high = mid;
```

---

# 🔥 Why `low = mid + 1`?

Suppose:

```text
arr[mid] < arr[mid+1]
```

To `mid` peak nahi ho sakta.

Example:

```text
2 < 3
```

Because:

```text
arr[mid+1] > arr[mid]
```

So `mid` ko safely remove kar sakte hain.

Therefore:

```cpp
low = mid + 1;
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    int peakIndexInMountainArray(vector<int>& arr) {

        int low = 0;
        int high = arr.size() - 1;

        while(low < high) {

            int mid = low + (high - low) / 2;

            if(arr[mid] < arr[mid + 1]) {

                // Increasing slope
                // Peak is on the right
                low = mid + 1;
            }

            else {

                // Decreasing slope
                // Peak is at mid or on the left
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
int high = arr.size() - 1;
```

Search last valid index tak hogi.

---

## Step 3 - Loop

```cpp
while(low < high)
```

Hum search space ko continuously reduce karenge.

End me:

```text
low == high
```

hoga.

Ek hi possible peak index bachega.

---

## Step 4 - `mid`

```cpp
int mid = low + (high - low) / 2;
```

Current search space ka middle index.

---

## Step 5 - Compare

```cpp
if(arr[mid] < arr[mid + 1])
```

Agar increasing slope hai:

```cpp
low = mid + 1;
```

Otherwise:

```cpp
high = mid;
```

---

# 🔄 Complete Dry Run

Given:

```text
arr = [0,2,3,4,3,2,1]
```

Array:

```text
Index:  0 1 2 3 4 5 6
Value:  0 2 3 4 3 2 1
```

---

## Initial

```text
low = 0
high = 6
```

---

## Iteration 1

Calculate:

```text
mid = 0 + (6-0)/2
    = 3
```

Values:

```text
arr[3] = 4
arr[4] = 3
```

Compare:

```text
4 < 3
```

False.

So slope decreasing hai.

Peak:

```text
mid or left
```

me hai.

Therefore:

```text
high = mid
high = 3
```

Now:

```text
low = 0
high = 3
```

---

## Iteration 2

Calculate:

```text
mid = 0 + (3-0)/2
    = 1
```

Values:

```text
arr[1] = 2
arr[2] = 3
```

Compare:

```text
2 < 3
```

True.

Slope increasing hai.

Peak right side me hai.

Therefore:

```text
low = mid + 1
low = 2
```

Now:

```text
low = 2
high = 3
```

---

## Iteration 3

Calculate:

```text
mid = 2 + (3-2)/2
    = 2
```

Values:

```text
arr[2] = 3
arr[3] = 4
```

Compare:

```text
3 < 4
```

True.

Slope increasing hai.

Peak right side me hai.

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

---

# ✅ Final Answer

```text
low = 3
```

Therefore:

```text
arr[3] = 4
```

Peak index:

```text
3
```

So:

```text
Output = 3
```

---

# 🔥 Second Example

```text
arr = [0,2,1]
```

Array:

```text
0 < 2 > 1
    ↑
  peak
```

Initial:

```text
low = 0
high = 2
```

### Iteration 1

```text
mid = 1
```

Values:

```text
arr[1] = 2
arr[2] = 1
```

Since:

```text
2 > 1
```

slope decreasing hai.

So:

```text
high = mid
high = 1
```

Now:

```text
low = 0
high = 1
```

### Iteration 2

```text
mid = 0
```

Values:

```text
arr[0] = 0
arr[1] = 2
```

Since:

```text
0 < 2
```

slope increasing hai.

So:

```text
low = mid + 1
low = 1
```

Now:

```text
low = high = 1
```

Answer:

```text
1
```

---

# 🧠 Mountain Visualization

Mountain ko actual mountain ki tarah imagine karo:

```text
              Peak
               /\
              /  \
             /    \
            /      \
           /        \
```

### Left Side

```text
arr[mid] < arr[mid+1]
```

Tum upar chadh rahe ho.

```text
UP → RIGHT
```

### Right Side

```text
arr[mid] > arr[mid+1]
```

Tum neeche aa rahe ho.

```text
DOWN → LEFT / MID
```

---

# 🔥 LC 162 Se Connection

LC 162:

```text
Find Peak Element
```

me bhi same slope logic use kiya tha.

```text
nums[mid] < nums[mid+1]
        ↓
RIGHT

nums[mid] > nums[mid+1]
        ↓
LEFT / MID
```

LC 852 me difference ye hai ki array explicitly **mountain array** hai:

```text
Increasing
    ↓
One Peak
    ↓
Decreasing
```

Therefore peak definitely exists and is unique.

---

# 📊 LC 162 vs LC 852

| Feature    | LC 162                       | LC 852                     |
| ---------- | ---------------------------- | -------------------------- |
| Array      | General                      | Mountain                   |
| Sorted     | No                           | No, but mountain structure |
| Peak       | Any valid peak               | Unique peak                |
| Comparison | `nums[mid]` vs `nums[mid+1]` | `arr[mid]` vs `arr[mid+1]` |
| Increasing | Go right                     | Go right                   |
| Decreasing | Go left/mid                  | Go left/mid                |
| Complexity | `O(log n)`                   | `O(log n)`                 |

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

Because `mid` can be the peak.

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

Because when:

```text
arr[mid] < arr[mid+1]
```

`mid` definitely peak nahi hai.

---

## 3. `arr[mid] <= arr[mid+1]`

Mountain array strictly increasing/decreasing hai, so:

```cpp
arr[mid] < arr[mid + 1]
```

use karna enough hai.

---

## 4. `while(low <= high)`

Is pattern me standard:

```cpp
while(low < high)
```

use karna easier hai because hum ek single peak index tak search space reduce kar rahe hain.

End:

```text
low == high
```

par answer mil jata hai.

---

# ⏱️ Time Complexity

Har iteration me search space approximately half hota hai.

Therefore:

```text
Time Complexity = O(log n)
```

---

# 💾 Space Complexity

Extra data structure use nahi hota.

Sirf:

```text
low
high
mid
```

variables hain.

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
Mountain Array
+
Peak Index
+
O(log n)
```

Immediately think:

```text
Binary Search on Slope
```

Main condition:

```cpp
arr[mid] < arr[mid + 1]
```

---

# 🧠 Interview Explanation

Interviewer puche:

**"How will you find the peak in a mountain array?"**

Bolna:

```text
The array first increases and then decreases, so I can use
binary search based on the slope.

I compare arr[mid] with arr[mid + 1].

If arr[mid] is smaller, the array is still increasing, so the
peak must be on the right side.

If arr[mid] is greater, we are on the decreasing side, so the
peak can be at mid or on the left side.

Therefore, I move low or high accordingly until low becomes equal
to high.

The time complexity is O(log n) and the space complexity is O(1).
```

---

# 🔄 Quick Revision

```text
Mountain Array
      ↓
low = 0
high = n - 1
      ↓
mid
      ↓
arr[mid] < arr[mid+1] ?
       /             \
     YES              NO
      ↓                ↓
   Slope UP          Slope DOWN
      ↓                ↓
 Peak RIGHT       Peak LEFT/MID
      ↓                ↓
low = mid + 1    high = mid
      ↓                ↓
        low == high
             ↓
       return low
```

---

# 🧠 One-Line Revision

```text
Mountain array me arr[mid] < arr[mid+1] ho to peak right me hai, warna peak mid ya left me hai.
```

---

# ⭐ Interview Revision Code

```cpp
class Solution {
public:
    int peakIndexInMountainArray(vector<int>& arr) {

        int low = 0;
        int high = arr.size() - 1;

        while(low < high) {

            int mid = low + (high - low) / 2;

            if(arr[mid] < arr[mid + 1]) {
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
Mountain Array
      +
Compare mid and mid+1
      ↓
UP   → RIGHT
DOWN → LEFT / MID
      ↓
low == high
      ↓
Peak Index
```

---

# 📌 Pattern

```text
Binary Search
    ↓
Binary Search on Slope
    ↓
Mountain Array
    ↓
Peak Index
    ↓
LC 852
```
