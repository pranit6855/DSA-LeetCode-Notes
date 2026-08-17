# LeetCode 69 - Sqrt(x)

## 📌 Problem

Hume ek non-negative integer `x` diya hai.

Hume:

```text id="k8m2p4"
sqrt(x)
```

ka **integer part** return karna hai.

Decimal part ignore karna hai.

### Example

```text id="q4n7v2"
x = 8
```

Actual square root:

```text id="m6c1z9"
√8 = 2.828...
```

Integer part:

```text id="r3p8k5"
2
```

So:

```text id="a1v6y3"
Output = 2
```

---

# 🔹 Example 2

```text id="t7m2q9"
x = 16
```

Because:

```text id="j4v8p1"
4 × 4 = 16
```

So:

```text id="x5k3n7"
Output = 4
```

---

# 🔹 Example 3

```text id="c9m4r2"
x = 20
```

Because:

```text id="h7p1v8"
4 × 4 = 16 <= 20
5 × 5 = 25 > 20
```

So integer square root:

```text id="w2q6k9"
4
```

---

# 🧠 Important Observation

Hume actual decimal square root calculate nahi karna.

Hume **largest integer `k`** find karna hai such that:

```text id="n5v8m2"
k * k <= x
```

Example:

```text id="p3q7y1"
x = 20
```

Check:

```text id="g8m4k6"
4 * 4 = 16 <= 20 ✅
5 * 5 = 25 > 20  ❌
```

Therefore:

```text id="r2x9p5"
answer = 4
```

---

# 🔥 Why Binary Search?

Directly:

```cpp id="a7m3q8"
sqrt(x)
```

use karke answer nikala ja sakta hai.

Lekin DSA me hume **Binary Search on Value** pattern seekhna hai.

Yahan koi array nahi hai.

Hum possible answer values me binary search karenge.

For example:

```text id="v9k4m2"
x = 20
```

Possible answers:

```text id="p6q8r1"
0,1,2,3,4,5,6,...,20
```

Inme se hume largest valid value chahiye:

```text id="m2x7k5"
mid * mid <= x
```

---

# 🔥 Search Range

Initially:

```cpp id="z4p8m1"
int low = 0;
int high = x;
```

Why?

Answer:

```text id="s6k2n9"
0 se x ke beech
```

hoga.

---

# 🔥 Main Binary Search Logic

Har iteration me:

```cpp id="q3m7x5"
int mid = low + (high - low) / 2;
```

Then check:

```text id="v8p1k4"
mid * mid
```

---

# ✅ Case 1 - `mid * mid <= x`

Example:

```text id="j6m2r8"
mid = 4
x = 20
```

Then:

```text id="n4q7p3"
4 * 4 = 16
```

And:

```text id="y5k1m9"
16 <= 20
```

So `mid` valid answer hai.

Lekin hume **largest valid answer** chahiye.

Ho sakta hai `5`, `6` etc. me se koi valid ho.

So:

```cpp id="p8m3x6"
ans = mid;
low = mid + 1;
```

Meaning:

```text id="q2v7k4"
mid valid hai
        ↓
answer save karo
        ↓
aur bada valid answer search karo
        ↓
RIGHT
```

---

# ❌ Case 2 - `mid * mid > x`

Example:

```text id="m7q4x1"
mid = 7
x = 20
```

Then:

```text id="c5n8p2"
7 * 7 = 49
```

And:

```text id="v1m6r9"
49 > 20
```

`mid` bahut bada hai.

Aur usse right side ki values aur badi hongi.

So answer left side me hoga.

```cpp id="k3p7x5"
high = mid - 1;
```

---

# 📊 Main Pattern

```text id="g7m2x9"
mid * mid <= x
        ↓
      VALID
        ↓
ans = mid
        ↓
RIGHT
        ↓
low = mid + 1
```

```text id="r4p8k1"
mid * mid > x
        ↓
     TOO LARGE
        ↓
      LEFT
        ↓
high = mid - 1
```

---

# 🔥 Why `ans` Variable?

Kyuki hume **largest valid value** chahiye.

Example:

```text id="c6m1q8"
x = 20
```

Suppose `4` valid mila:

```text id="h3p7v5"
4 * 4 <= 20
```

Hum:

```cpp id="z9k2m4"
ans = 4;
```

save karenge.

Phir right me search karenge.

Agar koi aur valid value nahi milta, `ans` ka last valid value hi answer rahega.

---

# 🔥 Dry Run

Given:

```text id="w4p8m1"
x = 20
```

Initial:

```text id="r7k2x5"
low = 0
high = 20
ans = 0
```

---

## Iteration 1

```text id="m3q9v6"
mid = 0 + (20 - 0) / 2
    = 10
```

Check:

```text id="p5k1r8"
10 * 10 = 100
```

```text id="x2m7q4"
100 > 20
```

Too large.

So:

```text id="v9p3k6"
high = mid - 1
high = 9
```

Now:

```text id="a4m8x1"
low = 0
high = 9
```

---

## Iteration 2

```text id="j7q2m5"
mid = 4
```

Check:

```text id="r3k8v1"
4 * 4 = 16
```

```text id="m6p2x9"
16 <= 20
```

Valid.

So:

```text id="q8v4m3"
ans = 4
```

And bigger answer try karna hai:

```text id="z1k7p5"
low = mid + 1
low = 5
```

Now:

```text id="h4m8q2"
low = 5
high = 9
```

---

## Iteration 3

```text id="p7x3m9"
mid = 7
```

Check:

```text id="k2v6q4"
7 * 7 = 49
```

```text id="r8m1p5"
49 > 20
```

Too large.

So:

```text id="n4x7k2"
high = mid - 1
high = 6
```

---

## Iteration 4

```text id="c5m8q1"
low = 5
high = 6

mid = 5
```

Check:

```text id="v2p7m4"
5 * 5 = 25
```

```text id="x9k3r6"
25 > 20
```

Too large.

So:

```text id="q6m1v8"
high = 4
```

Now:

```text id="p3x7k2"
low = 5
high = 4
```

Loop stops.

Last valid answer:

```text id="m8q4v1"
ans = 4
```

So:

```text id="k2p7x5"
Output = 4
```

---

# 🔥 Important Mistake - `ans = mid * mid`

Wrong:

```cpp id="j7m4p2"
ans = mid * mid;
```

Suppose:

```text id="b6q1v8"
mid = 4
```

Then:

```text id="n3x7k5"
mid * mid = 16
```

But question ka answer:

```text id="r2m8p4"
4
```

hai.

So correct:

```cpp id="f5k1q7"
ans = mid;
```

---

# ⚠️ Integer Overflow

Ye question ka bahut important C++ issue hai.

Agar:

```cpp
mid * mid
```

`int` me calculate karoge, to large `x` par overflow ho sakta hai.

Example:

```text id="m8q2v5"
mid = 1073697799
```

Then:

```text id="p4x7k1"
mid * mid
```

bahut bada number banega.

`int` ki limit approximately:

```text id="v6m1q8"
2,147,483,647
```

hai.

Isliye runtime error aa sakta hai:

```text id="k3p9x5"
signed integer overflow
```

---

# ✅ Overflow Ka Solution

Multiplication ko `long long` me karwao:

```cpp id="r7m2q4"
1LL * mid * mid
```

So condition:

```cpp id="x5k8v1"
if(1LL * mid * mid <= x)
```

Correct hai.

---

# 💻 Final Safe Code

```cpp id="q9m4x7"
class Solution {
public:
    int mySqrt(int x) {

        int low = 0;
        int high = x;
        int ans = 0;

        while(low <= high) {

            int mid = low + (high - low) / 2;

            if(1LL * mid * mid <= x) {

                // mid valid hai
                ans = mid;

                // Aur bada valid answer dhundho
                low = mid + 1;
            }

            else {

                // mid bahut bada hai
                high = mid - 1;
            }
        }

        return ans;
    }
};
```

---

# 🧠 `1LL` Kya Karta Hai?

```cpp id="m7x2q5"
1LL * mid * mid
```

me `1LL` ek `long long` value hai.

Isliye multiplication:

```text id="p4m8v1"
long long × int × int
```

ke form me hota hai.

Result `long long` me calculate hota hai.

So overflow avoid ho jata hai.

---

# 🔹 Example: `x = 0`

```text id="n3q7m2"
low = 0
high = 0
```

```text id="v8p1k4"
mid = 0
```

Check:

```text id="j5m9x2"
0 * 0 <= 0
```

Valid.

```text id="q4k7m1"
ans = 0
```

Return:

```text id="x6p2v8"
0
```

---

# 🔹 Example: `x = 1`

```text id="r8m4q1"
low = 0
high = 1
```

Possible answer:

```text id="p2x7k5"
1
```

Because:

```text id="v3m8q2"
1 * 1 = 1
```

So:

```text id="k6p1x9"
Output = 1
```

---

# 🔥 Why Right When Valid?

Suppose:

```text id="m4q8v1"
x = 20
mid = 4
```

We know:

```text id="p7x2k5"
4 * 4 <= 20
```

So `4` valid hai.

But question asks:

```text id="r1m6q9"
largest valid integer
```

Maybe `5` valid ho.

So:

```text id="y8v3p4"
RIGHT search
```

is required.

That's why:

```cpp id="n5k2x7"
low = mid + 1;
```

---

# 🔥 Why Left When Too Large?

Suppose:

```text id="q4m7v2"
mid = 7
x = 20
```

```text id="p8x1k5"
49 > 20
```

7 answer nahi ho sakta.

Aur 7 se right wali values aur badi hongi:

```text id="m3q9v6"
8,9,10...
```

unke squares aur bade honge.

So:

```text id="z2k7p4"
LEFT
```

search karna hai.

Therefore:

```cpp id="x6m1q8"
high = mid - 1;
```

---

# 🧠 Binary Search Pattern

Ye normal target search se thoda different hai.

Normal Binary Search:

```text id="k5m2v8"
target mil gaya
→ return
```

Yahan:

```text id="q7p1x4"
valid value mil gayi
→ answer save karo
→ better/larger answer search karo
```

So ye:

```text id="r3m8q2"
Maximum Valid Value
```

pattern hai.

---

# 🔥 Pattern Recognition

Question me agar:

```text id="v6q1m9"
Answer ek number hai
+
Possible answers ordered hain
+
Condition check kar sakte ho
+
Valid ke baad bhi better answer possible hai
```

to:

```text id="p4x7k2"
Binary Search on Value
```

socho.

LC 69 me:

```text id="m8q3v1"
Valid:
mid² <= x

Invalid:
mid² > x
```

---

# 📊 Complexity

Har iteration me search range half hota hai.

```text id="j4m9x2"
Time Complexity = O(log x)
```

Extra variables:

```text id="p6q1v8"
low
high
mid
ans
```

So:

```text id="r3m7k5"
Space Complexity = O(1)
```

---

# 🧠 One-Line Revision

```text id="x8m2q4"
Largest integer mid find karo jiska square x se chhota ya equal ho; valid mile to right jao aur invalid mile to left.
```

---

# ⭐ Interview Explanation

```text id="k7p3m9"
I treat the possible square-root values as the binary-search space.

For each mid, I check whether mid squared is less than or equal
to x.

If it is valid, mid can be the answer, but a larger valid value
may exist, so I store mid and search the right side.

If mid squared is greater than x, mid is too large, so I search
the left side.

I use 1LL in the multiplication to avoid integer overflow.

The time complexity is O(log x) and the space complexity is O(1).
```

---

# ⭐ Interview Revision Code

```cpp id="m2q7v4"
class Solution {
public:
    int mySqrt(int x) {

        int low = 0;
        int high = x;
        int ans = 0;

        while(low <= high) {

            int mid = low + (high - low) / 2;

            if(1LL * mid * mid <= x) {
                ans = mid;
                low = mid + 1;
            }
            else {
                high = mid - 1;
            }
        }

        return ans;
    }
};
```

---

# 🔥 Main Formula

```text id="p8m3q6"
Binary Search on Value
        ↓
Check mid² <= x
        ↓
Valid → save + RIGHT
Invalid → LEFT
        ↓
Largest Valid Value
        ↓
Integer Square Root
```

---

# 📌 Pattern

```text id="q4m7x1"
Binary Search
    ↓
Search on Value
    ↓
Maximum Valid Value
    ↓
Sqrt(x)
    ↓
LC 69
```
