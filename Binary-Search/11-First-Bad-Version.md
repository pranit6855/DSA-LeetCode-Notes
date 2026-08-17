# LeetCode 278 - First Bad Version

## 📌 Problem

Hume `1` se `n` tak versions diye gaye hain.

Har version:

```text
Good
```

ya:

```text
Bad
```

ho sakta hai.

Ek important condition hai:

> Ek baar koi version bad ho gaya, uske baad ke saare versions bhi bad honge.

Example:

```text
Version:  1  2  3  4  5  6  7
Status:   G  G  G  B  B  B  B
```

Hume **first bad version** find karna hai.

Answer:

```text
4
```

---

# 🔹 Example

```text
n = 5
first bad version = 4
```

Versions:

```text
1 → Good
2 → Good
3 → Good
4 → Bad
5 → Bad
```

Output:

```text
4
```

---

# 🧠 Important Observation

Versions ka pattern hamesha:

```text
Good Good Good Bad Bad Bad
```

aisa hoga.

Matlab `Good` ke baad ek boundary aayegi:

```text
Good | Bad
```

Hume isi boundary ko find karna hai.

Example:

```text
1  2  3  4  5  6  7
G  G  G  B  B  B  B
         ↑
    first bad
```

---

# 🔥 Why Binary Search?

Agar hum ek-ek version check karein:

```text
1
2
3
4
...
```

to worst case:

```text
O(n)
```

lagega.

Lekin versions ka pattern sorted/monotonic hai:

```text
false false false true true true
```

Isliye Binary Search use kar sakte hain.

---

# 🔥 Important Function

LeetCode hume ek function deta hai:

```cpp
isBadVersion(version)
```

Ye return karta hai:

```text
false → version good hai
true  → version bad hai
```

Hume is function ke through **first `true`** find karna hai.

So:

```text
Good = false
Bad  = true
```

Problem ko hum convert kar sakte hain:

```text
false false false true true true
                  ↑
             first true
```

---

# 🧠 Main Binary Search Idea

Hum maintain karenge:

```cpp
int low = 1;
int high = n;
```

### Why `low = 1`?

Kyuki version numbering:

```text
1, 2, 3, ..., n
```

se start hoti hai.

Version `0` exist nahi karta.

Isliye:

```text
low = 1
high = n
```

---

# 🔥 Case 1 - `mid` Good Hai

Agar:

```cpp
isBadVersion(mid) == false
```

to `mid` good hai.

Because versions monotonic hain, `mid` se pehle ke versions bhi good honge.

So first bad version:

```text
mid ke RIGHT
```

me hoga.

Therefore:

```cpp
low = mid + 1;
```

---

# 🔹 Example

```text
1  2  3  4  5  6  7
G  G  G  B  B  B  B
      ↑
     mid
```

Agar:

```text
mid = 3
```

aur:

```text
isBadVersion(3) = false
```

to version `3` good hai.

So first bad `3` ho hi nahi sakta.

Therefore:

```text
low = 4
```

---

# 🔥 Case 2 - `mid` Bad Hai

Agar:

```cpp
isBadVersion(mid) == true
```

to `mid` bad hai.

Lekin hume:

```text
FIRST bad version
```

chahiye.

Ho sakta hai:

```text
mid hi first bad ho
```

ya:

```text
mid se pehle koi aur bad version ho
```

So `mid` ko remove nahi kar sakte.

Therefore:

```cpp
high = mid;
```

---

# 🔥 Why `high = mid`, Not `mid - 1`?

Example:

```text
1  2  3  4  5
G  G  G  B  B
         ↑
        mid
```

Suppose:

```text
mid = 4
```

and:

```text
isBadVersion(4) = true
```

Ho sakta hai `4` hi first bad version ho.

Agar likh diya:

```cpp
high = mid - 1;
```

to:

```text
high = 3
```

and version `4` search se remove ho jayega.

Wrong.

Isliye:

```cpp
high = mid;
```

correct hai.

---

# 📊 Main Pattern

```text
mid = Good
    ↓
First Bad RIGHT me
    ↓
low = mid + 1
```

```text
mid = Bad
    ↓
First Bad LEFT / MID me
    ↓
high = mid
```

Simple rule:

```text
GOOD → RIGHT
BAD  → LEFT / MID
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    int firstBadVersion(int n) {

        int low = 1;
        int high = n;

        while(low < high) {

            int mid = low + (high - low) / 2;

            if(isBadVersion(mid)) {

                // mid bad hai
                // first bad mid ya left me ho sakta hai
                high = mid;
            }

            else {

                // mid good hai
                // first bad right me hoga
                low = mid + 1;
            }
        }

        return low;
    }
};
```

---

# 🧠 Code Line By Line

## `low`

```cpp
int low = 1;
```

Version numbering `1` se start hoti hai.

---

## `high`

```cpp
int high = n;
```

Last version `n` hai.

So complete search range:

```text
1 → n
```

---

## `while(low < high)`

```cpp
while(low < high)
```

Hum **first bad version ki exact position** tak search range reduce kar rahe hain.

Eventually:

```text
low == high
```

ho jayega.

Us point par sirf ek possible answer bachega.

---

# 🔄 Dry Run

Suppose:

```text
n = 7
first bad version = 4
```

Pattern:

```text
1  2  3  4  5  6  7
G  G  G  B  B  B  B
```

Initial:

```text
low = 1
high = 7
```

---

## Iteration 1

Calculate:

```text
mid = 1 + (7 - 1) / 2
    = 4
```

Check:

```text
isBadVersion(4)
```

returns:

```text
true
```

So version `4` bad hai.

Lekin `4` first bad ho sakta hai.

Therefore:

```text
high = 4
```

Now:

```text
low = 1
high = 4
```

---

## Iteration 2

```text
mid = 1 + (4 - 1) / 2
    = 2
```

Check:

```text
isBadVersion(2)
```

returns:

```text
false
```

Version `2` good hai.

So first bad `2` ya usse pehle nahi ho sakta.

Therefore:

```text
low = 2 + 1
low = 3
```

Now:

```text
low = 3
high = 4
```

---

## Iteration 3

```text
mid = 3 + (4 - 3) / 2
    = 3
```

Check:

```text
isBadVersion(3)
```

returns:

```text
false
```

Version `3` good hai.

So first bad right me hai:

```text
low = 4
```

Now:

```text
low = 4
high = 4
```

Loop stop.

---

# ✅ Final Answer

```cpp
return low;
```

`low`:

```text
4
```

So:

```text
Output = 4
```

---

# 🔥 Why `return low`?

Hum har step me search range ko first bad boundary ki taraf move kar rahe hain.

End me:

```text
low == high
```

hota hai.

Example:

```text
low = 4
high = 4
```

Sirf version `4` bacha.

Therefore:

```cpp
return low;
```

correct hai.

`return high` bhi same result dega because:

```text
low == high
```

---

# 🧠 Boundary Visualization

```text
Good Good Good | Bad Bad Bad
1    2    3    | 4   5   6
                ↑
           first bad
```

Hum exactly:

```text
Good | Bad
```

wali boundary find kar rahe hain.

---

# 🔥 Lower Bound Se Connection

LC 35 me:

```text
First index where nums[i] >= target
```

find kiya tha.

Yahan:

```text
First version where isBadVersion(i) == true
```

find kar rahe hain.

Dono ka pattern same hai:

```text
Valid position mil gayi
        ↓
Answer candidate
        ↓
LEFT search karo
```

Difference:

### LC 35

```cpp
nums[mid] >= target
```

### LC 278

```cpp
isBadVersion(mid) == true
```

---

# 📊 Lower Bound Pattern

General:

```text
false false false true true true
                  ↑
             first true
```

Agar `mid` false:

```text
low = mid + 1
```

Agar `mid` true:

```text
high = mid
```

Ye **First True / First Valid** pattern hai.

---

# ⚠️ Common Mistakes

## 1. `low = 0`

Wrong:

```cpp
int low = 0;
```

Version `0` exist nahi karta.

Correct:

```cpp
int low = 1;
```

---

## 2. `high = n - 1`

Wrong:

```cpp
int high = n - 1;
```

Version `n` bhi valid hai.

Correct:

```cpp
int high = n;
```

---

## 3. Bad Milne Par `high = mid - 1`

Wrong:

```cpp
high = mid - 1;
```

Because `mid` first bad ho sakta hai.

Correct:

```cpp
high = mid;
```

---

## 4. Good Milne Par `low = mid`

Wrong:

```cpp
low = mid;
```

Because `mid` already good hai.

So first bad `mid` nahi ho sakta.

Correct:

```cpp
low = mid + 1;
```

---

## 5. Target Milne Jaisa `return mid`

Wrong:

```cpp
if(isBadVersion(mid))
    return mid;
```

Kyunki current bad version first bad ho, zaroori nahi.

Example:

```text
G G G B B B
      ↑
     mid
```

Mid bad hai, but usse pehle bhi bad version ho sakta hai.

So answer immediately return nahi karna.

---

# ⏱️ Time Complexity

Har iteration me search range half hota hai.

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

use hote hain.

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
Good Good Good Bad Bad Bad
```

aur poocha ho:

```text
First Bad
```

think:

```text
Boundary Binary Search
```

Specifically:

```text
First True
```

---

# 🧠 One-Line Revision

```text
First bad version find karne ke liye bad milne par mid ko keep karke left jao, aur good milne par right jao.
```

---

# ⭐ Interview Explanation

Interviewer puche:

**"How will you find the first bad version?"**

Bolo:

```text
The versions are monotonic: once a version becomes bad, all
following versions are also bad.

So I can use binary search to find the first bad version.

If the middle version is good, the first bad version must be
on the right, so I move low to mid + 1.

If the middle version is bad, it can itself be the first bad
version, so I keep mid and move high to mid.

When low becomes equal to high, that position is the first
bad version.

The time complexity is O(log n) and the space complexity is O(1).
```

---

# ⭐ Interview Revision Code

```cpp
class Solution {
public:
    int firstBadVersion(int n) {

        int low = 1;
        int high = n;

        while(low < high) {

            int mid = low + (high - low) / 2;

            if(isBadVersion(mid)) {
                high = mid;
            }
            else {
                low = mid + 1;
            }
        }

        return low;
    }
};
```

---

# 🔥 Main Formula

```text
First Bad Version
        ↓
First True
        ↓
Good → RIGHT
Bad  → LEFT / MID
        ↓
low == high
        ↓
return low
```

---

# 📌 Pattern

```text
Binary Search
    ↓
Boundary Search
    ↓
First True
    ↓
First Bad Version
    ↓
LC 278
```
