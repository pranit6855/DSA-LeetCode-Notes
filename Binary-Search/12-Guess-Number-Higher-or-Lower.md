# LeetCode 374 - Guess Number Higher or Lower

## 📌 Problem

Hume `1` se `n` tak numbers diye gaye hain.

Inme se ek number **secret number** hai.

Hume secret number find karna hai.

LeetCode hume ek function deta hai:

```cpp
guess(num)
```

Ye function batata hai ki current guess secret number ke comparison me kahan hai.

---

# 🔹 `guess(num)` Ka Meaning

Function ke 3 possible results hain.

## 1. `guess(num) == 0`

Matlab:

```text
num == secret
```

Secret number mil gaya.

---

## 2. `guess(num) == 1`

Matlab:

```text
num < secret
```

Tumhara guess secret se chhota hai.

Isliye secret right side me hoga.

---

## 3. `guess(num) == -1`

Matlab:

```text
num > secret
```

Tumhara guess secret se bada hai.

Isliye secret left side me hoga.

---

# 🔹 Example

Suppose:

```text
n = 10
secret = 6
```

Numbers:

```text
1 2 3 4 5 6 7 8 9 10
```

Agar:

```cpp
guess(4)
```

to:

```text
4 < 6
```

So result:

```text
1
```

Agar:

```cpp
guess(8)
```

to:

```text
8 > 6
```

So result:

```text
-1
```

Agar:

```cpp
guess(6)
```

to:

```text
6 == 6
```

So result:

```text
0
```

---

# 🧠 Main Observation

Secret number `1` se `n` ke beech hai.

Ye ek sorted search space ki tarah behave karta hai:

```text
1 2 3 4 5 6 7 8 9 10
```

Hume ek-ek number check karne ki zarurat nahi.

Hum:

```text
mid
```

guess karenge.

Phir `guess(mid)` hume batayega ki next search:

```text
LEFT
```

me karni hai ya:

```text
RIGHT
```

me.

Isliye **Binary Search** use kar sakte hain.

---

# 🔥 Binary Search Approach

Initial:

```cpp
int low = 1;
int high = n;
```

Har iteration me:

```cpp
int mid = low + (high - low) / 2;
```

Then:

```cpp
guess(mid)
```

call karenge.

---

# ✅ Case 1 - `guess(mid) == 0`

Matlab:

```text
mid == secret
```

Answer mil gaya.

```cpp
return mid;
```

---

# ✅ Case 2 - `guess(mid) == 1`

Matlab:

```text
mid < secret
```

Current guess chhota hai.

To secret right side me hoga.

Example:

```text
1 2 3 4 5 6 7 8 9
      ↑         ↑
     mid      possible
```

So:

```cpp
low = mid + 1;
```

---

# ✅ Case 3 - `guess(mid) == -1`

Matlab:

```text
mid > secret
```

Current guess bada hai.

To secret left side me hoga.

So:

```cpp
high = mid - 1;
```

---

# 📊 Main Logic

```text
guess(mid) == 0
        ↓
   Secret mil gaya
        ↓
      return mid
```

```text
guess(mid) == 1
        ↓
 mid < secret
        ↓
      RIGHT
        ↓
low = mid + 1
```

```text
guess(mid) == -1
        ↓
 mid > secret
        ↓
       LEFT
        ↓
high = mid - 1
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    int guessNumber(int n) {

        int low = 1;
        int high = n;

        while(low <= high) {

            int mid = low + (high - low) / 2;

            int result = guess(mid);

            if(result == 0) {

                // Secret number mil gaya
                return mid;
            }

            else if(result == 1) {

                // mid secret se chhota hai
                // Right side me jao
                low = mid + 1;
            }

            else {

                // mid secret se bada hai
                // Left side me jao
                high = mid - 1;
            }
        }

        return -1;
    }
};
```

---

# 🔄 Dry Run

Suppose:

```text
n = 10
secret = 6
```

Initial:

```text
low = 1
high = 10
```

---

## Iteration 1

```text
mid = 1 + (10 - 1) / 2
    = 5
```

Check:

```cpp
guess(5)
```

Secret:

```text
6
```

So:

```text
5 < 6
```

Result:

```text
1
```

Matlab guess chhota hai.

Therefore:

```cpp
low = mid + 1;
```

So:

```text
low = 6
high = 10
```

---

## Iteration 2

```text
mid = 6 + (10 - 6) / 2
    = 8
```

Check:

```cpp
guess(8)
```

Because:

```text
8 > 6
```

result:

```text
-1
```

So secret left side me hai.

Therefore:

```cpp
high = mid - 1;
```

Now:

```text
low = 6
high = 7
```

---

## Iteration 3

```text
mid = 6 + (7 - 6) / 2
    = 6
```

Check:

```cpp
guess(6)
```

Since:

```text
6 == 6
```

result:

```text
0
```

Secret mil gaya.

Return:

```text
6
```

---

# ✅ Final Answer

```text
Output = 6
```

---

# 🔥 LC 704 Se Connection

Ye question basically **normal Binary Search** ka variation hai.

LC 704 me:

```cpp
if(nums[mid] == target)
```

compare karte the.

Yahan:

```cpp
guess(mid)
```

hume comparison ka result de raha hai.

### LC 704

```text
nums[mid] < target
→ RIGHT

nums[mid] > target
→ LEFT
```

### LC 374

```text
guess(mid) == 1
→ RIGHT

guess(mid) == -1
→ LEFT
```

So underlying pattern same hai:

```text
Binary Search
+
Direction from comparison
```

---

# 🧠 Why `low = 1`?

Versions ki tarah yahan bhi search space:

```text
1 → n
```

hai.

Number `0` search space ka part nahi hai.

Therefore:

```cpp
int low = 1;
int high = n;
```

---

# 🧠 Why `while(low <= high)`?

Yahan hum secret number ko directly search kar rahe hain.

Agar:

```text
low == high
```

to ek number abhi bhi check karna baaki ho sakta hai.

Example:

```text
low = 6
high = 6
```

Hume `6` ko guess karna hai.

Isliye:

```cpp
while(low <= high)
```

use karte hain.

---

# ⚠️ Common Mistakes

## 1. `low = 0`

Wrong:

```cpp
int low = 0;
```

Correct:

```cpp
int low = 1;
```

Because search `1` se start hoti hai.

---

## 2. `high = n - 1`

Wrong:

```cpp
int high = n - 1;
```

Version/number `n` bhi possible answer hai.

Correct:

```cpp
int high = n;
```

---

## 3. `guess(mid) == 1` Ka Meaning Ulta Karna

Correct:

```text
1 → mid < secret → RIGHT
```

and:

```text
-1 → mid > secret → LEFT
```

---

## 4. `low = mid`

Wrong:

```cpp
low = mid;
```

Correct:

```cpp
low = mid + 1;
```

Because `mid` already checked ho chuka hai aur woh secret nahi tha.

Same:

```cpp
high = mid - 1;
```

because `mid` already checked ho chuka hai.

---

# ⏱️ Time Complexity

Har iteration me roughly half numbers eliminate hote hain.

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
result
```

variables hain.

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

Question me agar:

```text
1 se n
+
secret number
+
function comparison deta hai
+
O(log n)
```

to think:

```text
Binary Search with Feedback
```

---

# 🧠 One-Line Revision

```text
guess(mid) ka result use karke decide karo ki secret left me hai, right me hai, ya mid hi answer hai.
```

---

# ⭐ Interview Explanation

```text
The secret number lies between 1 and n, so I maintain a binary
search range from 1 to n.

I calculate the middle value and use the guess function.

If guess(mid) returns 0, I found the secret number.

If it returns 1, mid is smaller than the secret, so I search
the right half.

If it returns -1, mid is larger than the secret, so I search
the left half.

Since the search space is halved in every iteration, the time
complexity is O(log n) and the space complexity is O(1).
```

---

# ⭐ Interview Revision Code

```cpp
class Solution {
public:
    int guessNumber(int n) {

        int low = 1;
        int high = n;

        while(low <= high) {

            int mid = low + (high - low) / 2;

            int result = guess(mid);

            if(result == 0) {
                return mid;
            }

            else if(result == 1) {
                low = mid + 1;
            }

            else {
                high = mid - 1;
            }
        }

        return -1;
    }
};
```

---

# 🔥 Main Formula

```text
guess(mid)
    ↓
0  → FOUND
1  → RIGHT
-1 → LEFT
    ↓
Binary Search
    ↓
O(log n)
```

---

# 📌 Pattern

```text
Binary Search
    ↓
Feedback Based Search
    ↓
Guess Number
    ↓
LC 374
```
