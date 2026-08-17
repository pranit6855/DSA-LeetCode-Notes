# LeetCode 1539 - Kth Missing Positive Number

## 📌 Problem

Hume ek sorted array `arr` diya hai jisme **distinct positive integers** hain.

Hume find karna hai:

```text
k-th positive integer
jo array me missing hai.
```

### Important Conditions

```text
1. Array sorted hai.
2. Elements positive integers hain.
3. Elements distinct hain.
4. Hume k-th missing positive number find karna hai.
5. Binary Search use karke O(log n) me solve karna hai.
```

Question:

```text
Kth Missing Positive Number
```

---

# 🔹 Example

```text
arr = [2,3,4,7,11]
k = 5
```

Positive numbers:

```text
1,2,3,4,5,6,7,8,9,10,11...
```

Array me jo numbers nahi hain:

```text
1,5,6,8,9,10...
```

5th missing number:

```text
1st → 1
2nd → 5
3rd → 6
4th → 8
5th → 9
```

So:

```text
Output = 9
```

---

# 🔹 Another Example

```text
arr = [1,2,3,4]
k = 2
```

Missing positive numbers:

```text
5,6,7,8...
```

2nd missing number:

```text
6
```

So:

```text
Output = 6
```

---

# 🧠 Main Observation

Hume directly missing numbers count karne ki zarurat nahi hai.

Hum Binary Search use kar sakte hain because array sorted hai.

Sabse important observation:

Agar array me koi number missing nahi hota, to index `i` par value:

```text
i + 1
```

hoti.

Example:

```text
index:     0   1   2   3   4
expected:  1   2   3   4   5
```

But actual array:

```text
[2,3,4,7,11]
```

Isliye kuch numbers missing hain.

---

# 🔥 Missing Numbers Kaise Count Karein?

Index `i` par:

```cpp
missing = arr[i] - (i + 1);
```

Ye batata hai ki `arr[i]` se pehle kitne positive numbers missing hain.

---

## Example

```text
arr = [2,3,4,7,11]
```

### Index 0

```text
arr[0] = 2

missing = 2 - (0 + 1)
        = 1
```

Missing:

```text
1
```

---

### Index 1

```text
arr[1] = 3

missing = 3 - (1 + 1)
        = 1
```

Abhi bhi:

```text
1
```

missing hai.

---

### Index 2

```text
arr[2] = 4

missing = 4 - (2 + 1)
        = 1
```

---

### Index 3

```text
arr[3] = 7

missing = 7 - (3 + 1)
        = 3
```

Missing numbers:

```text
1,5,6
```

---

### Index 4

```text
arr[4] = 11

missing = 11 - (4 + 1)
        = 6
```

Missing numbers:

```text
1,5,6,8,9,10
```

---

# 📊 Missing Count Table

```text
index:     0   1   2   3   4
arr:       2   3   4   7  11
missing:   1   1   1   3   6
```

Hume:

```text
k = 5
```

chahiye.

So hume first position find karni hai jahan:

```text
missing >= k
```

---

# 🔥 Binary Search

Hum maintain karenge:

```text
low
mid
high
```

Initial:

```text
low = 0
high = arr.size() - 1
```

Har iteration me:

```cpp
int mid = low + (high - low) / 2;
```

Then:

```cpp
int missing = arr[mid] - (mid + 1);
```

---

# 🔥 Case 1

```cpp
missing < k
```

Iska matlab:

```text
Abhi k missing numbers nahi mile.
```

Toh hume array ke **right side** me search karna padega.

Therefore:

```cpp
low = mid + 1;
```

---

# 🔹 Example

```text
arr = [2,3,4,7,11]
k = 5
```

Suppose:

```text
mid = 2
arr[mid] = 4
```

Missing:

```text
missing = 4 - 3
        = 1
```

Compare:

```text
1 < 5
```

Abhi sirf 1 missing number hai.

Hume 5 chahiye.

Therefore:

```text
Minimum/required answer RIGHT side me hai.
```

So:

```cpp
low = mid + 1;
```

---

# 🔥 Case 2

```cpp
missing >= k
```

Iska matlab:

```text
K-th missing number yahan ya left side me ho sakta hai.
```

Therefore:

```cpp
high = mid - 1;
```

Hum left side search karenge.

---

# 🔹 Example

```text
arr = [2,3,4,7,11]
k = 5
```

Suppose:

```text
mid = 4
arr[mid] = 11
```

Missing:

```text
missing = 11 - 5
        = 6
```

Compare:

```text
6 >= 5
```

Matlab 11 tak 5 se zyada missing numbers hain.

So 5th missing number 11 se pehle hai.

Therefore:

```cpp
high = mid - 1;
```

---

# 📌 Main Logic

```text
missing < k
      ↓
k-th missing abhi nahi mila
      ↓
RIGHT
      ↓
low = mid + 1
```

```text
missing >= k
      ↓
k-th missing yahan ya LEFT me
      ↓
LEFT
      ↓
high = mid - 1
```

---

# 🔄 Complete Dry Run

Given:

```text
arr = [2,3,4,7,11]
k = 5
```

Array:

```text
Index:  0  1  2  3   4
Value:  2  3  4  7  11
```

---

## Initial

```text
low = 0
high = 4
```

---

## Iteration 1

Calculate:

```text
mid = low + (high - low) / 2

mid = 0 + (4 - 0) / 2
    = 2
```

So:

```text
arr[mid] = arr[2] = 4
```

Calculate missing:

```text
missing = arr[mid] - (mid + 1)

        = 4 - 3

        = 1
```

Compare:

```text
1 < 5
```

So:

```text
k-th missing number RIGHT side me hai.
```

Update:

```cpp
low = mid + 1;
```

Therefore:

```text
low = 3
high = 4
```

---

## Iteration 2

```text
low = 3
high = 4
```

Calculate:

```text
mid = 3 + (4 - 3) / 2
    = 3
```

So:

```text
arr[mid] = 7
```

Missing:

```text
missing = 7 - (3 + 1)
        = 7 - 4
        = 3
```

Compare:

```text
3 < 5
```

Still:

```text
5 missing numbers nahi mile.
```

Therefore:

```cpp
low = mid + 1;
```

Now:

```text
low = 4
high = 4
```

---

## Iteration 3

```text
low = 4
high = 4
```

Calculate:

```text
mid = 4
```

So:

```text
arr[mid] = 11
```

Missing:

```text
missing = 11 - (4 + 1)
        = 11 - 5
        = 6
```

Compare:

```text
6 >= 5
```

Ab required number mil gaya / boundary cross ho gayi.

Therefore:

```cpp
high = mid - 1;
```

So:

```text
low = 4
high = 3
```

Now:

```text
low > high
```

Loop stop.

---

# 🔥 Why `return low + k`?

Ye is question ka sabse important part hai.

Binary Search ke end me:

```text
low = 4
k = 5
```

Answer:

```text
low + k
= 4 + 5
= 9
```

But ye formula kaise aaya?

Maan lo answer `x` hai.

`1` se `x` tak total:

```text
x
```

positive numbers hain.

Inme se `low` numbers array me already present hain.

So missing numbers:

```text
x - low
```

Hume exactly `k` missing numbers chahiye:

```text
x - low = k
```

Therefore:

```text
x = low + k
```

So:

```cpp
return low + k;
```

Correct answer:

```text
9
```

---

# 📊 Complete Dry Run Table

| Iteration | Low | High | Mid | `arr[mid]` | Missing | Condition | Action |
| --------- | --: | ---: | --: | ---------: | ------: | --------- | ------ |
| 1         |   0 |    4 |   2 |          4 |       1 | `1 < 5`   | Right  |
| 2         |   3 |    4 |   3 |          7 |       3 | `3 < 5`   | Right  |
| 3         |   4 |    4 |   4 |         11 |       6 | `6 >= 5`  | Left   |

End:

```text
low = 4
high = 3
```

Answer:

```text
low + k
= 4 + 5
= 9
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    int findKthPositive(vector<int>& arr, int k) {

        int low = 0;
        int high = arr.size() - 1;

        while (low <= high) {

            int mid = low + (high - low) / 2;

            int missing = arr[mid] - (mid + 1);

            if (missing >= k) {

                high = mid - 1;
            }
            else {

                low = mid + 1;
            }
        }

        return low + k;
    }
};
```

---

# 🧠 Code Line By Line

## Step 1 - `low`

```cpp
int low = 0;
```

Search array ke first index se start hogi.

---

## Step 2 - `high`

```cpp
int high = arr.size() - 1;
```

Last valid index:

```text
n - 1
```

hoga.

---

## Step 3 - Loop

```cpp
while (low <= high)
```

Jab tak valid search range hai, Binary Search chalegi.

---

## Step 4 - `mid`

```cpp
int mid = low + (high - low) / 2;
```

Middle index calculate karte hain.

---

## Step 5 - Missing Count

```cpp
int missing = arr[mid] - (mid + 1);
```

Ye batata hai ki `arr[mid]` tak kitne positive integers missing hain.

---

## Step 6 - Move Left

```cpp
if (missing >= k) {
    high = mid - 1;
}
```

Agar `k` missing numbers mil chuke hain, answer left side me ho sakta hai.

---

## Step 7 - Move Right

```cpp
else {
    low = mid + 1;
}
```

Agar missing numbers `k` se kam hain, answer right side me hoga.

---

## Step 8 - Return Answer

```cpp
return low + k;
```

Binary Search ke baad `low` boundary position batata hai.

`k` add karne se actual k-th missing positive number milta hai.

---

# ❌ Common Mistakes

## 1. `missing > k` use karna

Wrong:

```cpp
if (missing > k)
```

Correct:

```cpp
if (missing >= k)
```

Because agar:

```text
missing = k
```

ho gaya, toh hume left jana hai.

Example:

```text
missing = 3
k = 3
```

`3 > 3` false hai, but `3 >= 3` true hai.

---

## 2. Missing Formula Galat Likhna

Wrong:

```cpp
arr[mid] - mid
```

Correct:

```cpp
arr[mid] - (mid + 1)
```

Because array index `0` se start hota hai but positive numbers `1` se.

---

## 3. `return arr[low]`

Wrong:

```cpp
return arr[low];
```

Answer array ke andar hona zaroori nahi hai.

Example:

```text
arr = [2,3,4,7,11]
k = 5

answer = 9
```

`9` array me nahi hai.

Correct:

```cpp
return low + k;
```

---

# 🧠 Why Binary Search Works?

Missing count:

```text
index:     0   1   2   3   4
missing:   1   1   1   3   6
```

Notice:

```text
1 → 1 → 1 → 3 → 6
```

Missing count kabhi decrease nahi hota.

It is **monotonic increasing/non-decreasing**.

Isi wajah se Binary Search laga sakte hain.

---

# 🔥 Pattern Recognition

Agar question me dikhe:

```text
Sorted Array
+
Positive Integers
+
Missing Elements
+
Find k-th Missing
+
O(log n)
```

Immediately think:

```text
Binary Search
      ↓
Count Missing
      ↓
Find First Position
where missing >= k
```

---

# 📌 Main Pattern

```text
missing = arr[mid] - (mid + 1)
```

Then:

```text
missing < k
    ↓
RIGHT
    ↓
low = mid + 1
```

```text
missing >= k
    ↓
LEFT
    ↓
high = mid - 1
```

Finally:

```text
answer = low + k
```

---

# ⏱️ Time Complexity

Binary Search me har iteration me search space approximately half hota hai.

Therefore:

```text
Time Complexity = O(log n)
```

---

# 💾 Space Complexity

Hum sirf kuch variables use kar rahe hain:

```text
low
high
mid
missing
```

Koi extra array/map nahi bana rahe.

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

**"How do you find the k-th missing positive number in O(log n)?"**

Bolo:

```text
The array is sorted, so I use Binary Search.

For an index i, if there were no missing numbers,
the expected value would be i + 1.

Therefore, the number of missing positive integers
before arr[i] is:

arr[i] - (i + 1)

I use Binary Search to find the first position where
the number of missing elements becomes greater than
or equal to k.

If missing is less than k, I move right.
Otherwise, I move left.

After Binary Search, the answer is low + k.

The time complexity is O(log n) and the space complexity
is O(1).
```

---

# ⭐ Interview Revision Code

```cpp
class Solution {
public:
    int findKthPositive(vector<int>& arr, int k) {

        int low = 0;
        int high = arr.size() - 1;

        while (low <= high) {

            int mid = low + (high - low) / 2;

            int missing = arr[mid] - (mid + 1);

            if (missing >= k) {
                high = mid - 1;
            }
            else {
                low = mid + 1;
            }
        }

        return low + k;
    }
};
```

---

# 🔄 Quick Revision

```text
K-th Missing Positive Number
          ↓
      Sorted Array
          ↓
     Binary Search
          ↓
missing = arr[mid] - (mid + 1)
          ↓
   ┌───────────────┐
   ↓               ↓
missing < k     missing >= k
   ↓               ↓
 RIGHT             LEFT
   ↓               ↓
low = mid + 1   high = mid - 1
          ↓
    Binary Search End
          ↓
      low + k
          ↓
       Answer
```

---

# 🧠 One-Line Revision

```text
Missing count nikalo → missing >= k ki first boundary find karo → answer = low + k
```

---

# 🔥 Main Formula

```text
Missing Count
= arr[mid] - (mid + 1)
```

```text
missing < k
→ low = mid + 1
```

```text
missing >= k
→ high = mid - 1
```

```text
Answer
= low + k
```

---

# 📌 Pattern

```text
Binary Search
    ↓
Sorted Array
    ↓
Count Missing Elements
    ↓
Find First Position
where missing >= k
    ↓
Return low + k
```
