# LeetCode 367 - Valid Perfect Square

## 📌 Problem

Hume ek positive integer `num` diya hai.

Hume check karna hai ki kya `num` **perfect square** hai ya nahi.

Perfect square ka matlab:

```text
Kisi integer ko khud se multiply karne par num aaye.
```

For example:

```text
1 × 1 = 1
2 × 2 = 4
3 × 3 = 9
4 × 4 = 16
5 × 5 = 25
```

So:

```text
1, 4, 9, 16, 25...
```

perfect squares hain.

---

# 🔹 Example 1

```text
Input:
num = 16
```

Check:

```text
4 × 4 = 16
```

So:

```text
Output = true
```

---

# 🔹 Example 2

```text
Input:
num = 14
```

Check:

```text
3 × 3 = 9
4 × 4 = 16
```

`14` kisi integer ke square ke equal nahi hai.

So:

```text
Output = false
```

---

# 🧠 What Is a Perfect Square?

Agar kisi integer `x` ke liye:

```text
x × x = num
```

then `num` perfect square hai.

Example:

```text
25 = 5 × 5
```

So:

```text
25 → Perfect Square
```

But:

```text
20
```

ke liye koi integer `x` nahi hai jiske liye:

```text
x × x = 20
```

So:

```text
20 → Not a Perfect Square
```

---

# 🔥 Brute Force Approach

Simple way:

```text
1 × 1
2 × 2
3 × 3
4 × 4
...
```

Jab tak square `num` se greater na ho jaye.

Example:

```text
num = 16

1² = 1
2² = 4
3² = 9
4² = 16
```

Mil gaya:

```text
4² = 16
```

Answer:

```text
true
```

But ye approach potentially `O(√n)` time leti hai.

Question ko Binary Search se efficiently solve kar sakte hain.

---

# 🔥 Why Binary Search?

Hum possible square roots ke range me search kar rahe hain.

For:

```text
num = 16
```

Possible answer:

```text
1,2,3,4
```

because:

```text
4 × 4 = 16
```

Aur squares increasing order me hote hain:

```text
1² = 1
2² = 4
3² = 9
4² = 16
```

So hum Binary Search use kar sakte hain.

---

# 📌 Main Observation

Hum `1` se `num` tak search kar sakte hain.

```text
low = 0
high = num
```

Har iteration me:

```cpp
int mid = low + (high - low) / 2;
```

Then check:

```text
mid × mid
```

---

# 🔥 Three Cases

Har `mid` par teen possibilities hain.

---

## Case 1 — `mid * mid == num`

Agar:

```text
mid × mid = num
```

to `num` perfect square hai.

Example:

```text
num = 16
mid = 4

4 × 4 = 16
```

So:

```cpp
return true;
```

---

# 🔥 Case 2 — `mid * mid < num`

Agar:

```text
mid × mid < num
```

to `mid` chhota hai.

Example:

```text
num = 20
mid = 4

4 × 4 = 16
```

But:

```text
16 < 20
```

So hume bigger number try karna padega.

Therefore:

```cpp
low = mid + 1;
```

Meaning:

```text
RIGHT →
```

---

# 🔥 Case 3 — `mid * mid > num`

Agar:

```text
mid × mid > num
```

to `mid` bada hai.

Example:

```text
num = 20
mid = 5

5 × 5 = 25
```

But:

```text
25 > 20
```

So hume smaller number try karna padega.

Therefore:

```cpp
high = mid - 1;
```

Meaning:

```text
← LEFT
```

---

# 📊 Main Logic

```text
mid × mid == num
        ↓
      TRUE
```

```text
mid × mid < num
        ↓
     mid small
        ↓
      RIGHT
        ↓
low = mid + 1
```

```text
mid × mid > num
        ↓
     mid large
        ↓
       LEFT
        ↓
high = mid - 1
```

---

# 🔄 Complete Dry Run

Let's take:

```text
num = 16
```

Initial:

```text
low = 0
high = 16
```

---

## Iteration 1

Calculate:

```text
mid = low + (high - low) / 2

    = 0 + (16 - 0) / 2

    = 8
```

So:

```text
mid = 8
```

Square:

```text
8 × 8 = 64
```

Compare:

```text
64 > 16
```

`mid` bada hai.

Therefore:

```cpp
high = mid - 1;
```

So:

```text
low = 0
high = 7
```

---

## Iteration 2

Calculate:

```text
mid = 0 + (7 - 0) / 2
    = 3
```

So:

```text
mid = 3
```

Square:

```text
3 × 3 = 9
```

Compare:

```text
9 < 16
```

`mid` chhota hai.

Therefore:

```cpp
low = mid + 1;
```

So:

```text
low = 4
high = 7
```

---

## Iteration 3

Calculate:

```text
mid = 4 + (7 - 4) / 2
    = 5
```

Square:

```text
5 × 5 = 25
```

Compare:

```text
25 > 16
```

`mid` bada hai.

Therefore:

```cpp
high = mid - 1;
```

Now:

```text
low = 4
high = 4
```

---

## Iteration 4

Calculate:

```text
mid = 4 + (4 - 4) / 2
    = 4
```

Square:

```text
4 × 4 = 16
```

Compare:

```text
16 == 16
```

Mil gaya.

Therefore:

```cpp
return true;
```

---

# 📊 Dry Run Table

| Iteration | Low | High | Mid | `mid * mid` | Comparison | Action |
| --------- | --: | ---: | --: | ----------: | ---------- | ------ |
| 1         |   0 |   16 |   8 |          64 | `64 > 16`  | Left   |
| 2         |   0 |    7 |   3 |           9 | `9 < 16`   | Right  |
| 3         |   4 |    7 |   5 |          25 | `25 > 16`  | Left   |
| 4         |   4 |    4 |   4 |          16 | `16 == 16` | Found  |

Final:

```text
16 = 4 × 4
```

So:

```text
Output = true
```

---

# 🔄 Dry Run 2 — Not a Perfect Square

Let's take:

```text
num = 14
```

Initial:

```text
low = 0
high = 14
```

---

## Iteration 1

```text
mid = 7
```

```text
7 × 7 = 49
```

Since:

```text
49 > 14
```

Go left:

```text
high = 6
```

---

## Iteration 2

```text
mid = 3
```

```text
3 × 3 = 9
```

Since:

```text
9 < 14
```

Go right:

```text
low = 4
```

---

## Iteration 3

```text
mid = 5
```

```text
5 × 5 = 25
```

Since:

```text
25 > 14
```

Go left:

```text
high = 4
```

---

## Loop Ends

Now:

```text
low = 4
high = 4
```

Check:

```text
4 × 4 = 16
```

But:

```text
16 != 14
```

Search finishes.

Therefore:

```text
Output = false
```

---

# 🔥 Important Problem Pattern

Ye problem actually:

```text
Binary Search on Possible Answer
```

ka example hai.

Hum target number directly array me search nahi kar rahe.

Instead:

```text
Possible square root
        ↓
mid
        ↓
mid × mid
        ↓
Compare with num
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    bool isPerfectSquare(int num) {

        int low = 0;
        int high = num;

        while (low <= high) {

            int mid = low + (high - low) / 2;

            if (mid * mid == num) {
                return true;
            }
            else if (mid * mid < num) {
                low = mid + 1;
            }
            else {
                high = mid - 1;
            }
        }

        return false;
    }
};
```

---

# 🧠 Code Line By Line

## Step 1 - `low`

```cpp
int low = 0;
```

Possible square root `0` se start kar rahe hain.

---

## Step 2 - `high`

```cpp
int high = num;
```

Worst case me square root `num` tak ho sakta hai.

For example:

```text
num = 1
1 × 1 = 1
```

---

## Step 3 - Loop

```cpp
while (low <= high)
```

Jab tak valid search space hai, Binary Search continue karegi.

---

## Step 4 - `mid`

```cpp
int mid = low + (high - low) / 2;
```

Current possible square root.

---

## Step 5 - Exact Match

```cpp
if (mid * mid == num)
```

Agar square exactly `num` ke equal hai:

```cpp
return true;
```

---

## Step 6 - Square Smaller

```cpp
else if (mid * mid < num)
```

Agar square chhota hai:

```text
mid is too small
```

Therefore:

```cpp
low = mid + 1;
```

---

## Step 7 - Square Bigger

```cpp
else
```

Agar square bada hai:

```text
mid is too large
```

Therefore:

```cpp
high = mid - 1;
```

---

## Step 8 - Not Found

Agar Binary Search finish ho gayi aur exact square nahi mila:

```cpp
return false;
```

---

# ⚠️ Important: Integer Overflow

Ye code:

```cpp
mid * mid
```

large `num` ke liye integer overflow cause kar sakta hai.

Safer approach:

```cpp
long long square = 1LL * mid * mid;
```

Then:

```cpp
if (square == num)
```

Use kar sakte hain.

### Safer Code

```cpp
class Solution {
public:
    bool isPerfectSquare(int num) {

        int low = 0;
        int high = num;

        while (low <= high) {

            int mid = low + (high - low) / 2;

            long long square = 1LL * mid * mid;

            if (square == num) {
                return true;
            }
            else if (square < num) {
                low = mid + 1;
            }
            else {
                high = mid - 1;
            }
        }

        return false;
    }
};
```

---

# 🔥 Optimization

Hum actually `0` se `num` tak search karne ki zarurat nahi hai.

For `num > 1`, square root maximum `num / 2` ke around hoga.

But simple Binary Search approach ke liye:

```cpp
int high = num;
```

easy to understand hai.

The important concept is the Binary Search logic.

---

# ❌ Common Mistakes

## 1. `low = mid`

Wrong:

```cpp
low = mid;
```

Agar `mid * mid < num`, `mid` already check ho chuka hai and too small hai.

So:

```cpp
low = mid + 1;
```

---

## 2. `high = mid`

Wrong:

```cpp
high = mid;
```

Agar `mid * mid > num`, `mid` already too large hai.

So:

```cpp
high = mid - 1;
```

---

## 3. Exact Square Milne Par Continue Karna

Agar:

```text
mid × mid == num
```

to answer immediately mil gaya.

So:

```cpp
return true;
```

---

## 4. `mid * mid` Overflow

Large values ke case me:

```cpp
mid * mid
```

overflow kar sakta hai.

Safer:

```cpp
long long square = 1LL * mid * mid;
```

---

# 🧠 Pattern Recognition

Agar question me:

```text
Find whether number is a perfect square
+
Need efficient solution
```

dikhe, think:

```text
Possible square roots
        ↓
Binary Search
        ↓
mid × mid
        ↓
Compare with num
```

---

# 🔥 Main Pattern

```text
mid × mid == num
        ↓
      TRUE
```

```text
mid × mid < num
        ↓
   mid too small
        ↓
      RIGHT
        ↓
low = mid + 1
```

```text
mid × mid > num
        ↓
   mid too large
        ↓
       LEFT
        ↓
high = mid - 1
```

---

# 📌 Binary Search Flow

```text
Start
  ↓
low = 0
high = num
  ↓
Calculate mid
  ↓
Calculate mid × mid
  ↓
 ┌─────────────────────┐
 │                     │
 ↓                     ↓
== num               != num
 ↓                     ↓
true              Compare
                       ↓
              ┌────────┴────────┐
              ↓                 ↓
          < num              > num
              ↓                 ↓
           RIGHT              LEFT
              ↓                 ↓
      low = mid + 1      high = mid - 1
```

---

# ⏱️ Time Complexity

Binary Search har iteration me search space approximately half karti hai.

Therefore:

```text
Time Complexity = O(log n)
```

---

# 💾 Space Complexity

Hum sirf:

```text
low
high
mid
square
```

variables use kar rahe hain.

Koi extra data structure nahi hai.

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

**"How do you check whether a number is a perfect square using Binary Search?"**

Bolo:

```text
I use Binary Search on the possible square root.

For every mid, I calculate mid * mid and compare it
with the given number.

If mid * mid equals num, the number is a perfect square.

If mid * mid is smaller than num, mid is too small,
so I search on the right side.

If mid * mid is greater than num, mid is too large,
so I search on the left side.

If the search finishes without finding an exact square,
I return false.

The time complexity is O(log n) and the space complexity
is O(1).
```

---

# ⭐ Interview Revision Code

```cpp
class Solution {
public:
    bool isPerfectSquare(int num) {

        int low = 0;
        int high = num;

        while (low <= high) {

            int mid = low + (high - low) / 2;

            long long square = 1LL * mid * mid;

            if (square == num) {
                return true;
            }
            else if (square < num) {
                low = mid + 1;
            }
            else {
                high = mid - 1;
            }
        }

        return false;
    }
};
```

---

# 🔄 Quick Revision

```text
Perfect Square
      ↓
Search possible square root
      ↓
Binary Search
      ↓
mid × mid
      ↓
== num → TRUE
< num  → RIGHT
> num  → LEFT
      ↓
Not found → FALSE
```

---

# 🧠 One-Line Revision

```text
Possible square root par Binary Search lagao aur mid² ko num se compare karke left/right move karo.
```

---

# 🔥 Main Formula

```text
square = mid × mid
```

Then:

```text
square == num
→ return true
```

```text
square < num
→ low = mid + 1
```

```text
square > num
→ high = mid - 1
```

---

# 📌 Pattern

```text
Binary Search
    ↓
Search Possible Answer
    ↓
Possible Square Root
    ↓
mid × mid
    ↓
Compare with num
    ↓
TRUE / LEFT / RIGHT
```

---

# 🔗 Related Binary Search Problems

This problem is part of the same Binary Search progression:

```text
69  → Sqrt(x)
367 → Valid Perfect Square
1539 → Kth Missing Positive Number
```

`69` me hum **largest valid square root** find karte hain, while `367` me hum check karte hain ki **exact square root exist karta hai ya nahi**.
