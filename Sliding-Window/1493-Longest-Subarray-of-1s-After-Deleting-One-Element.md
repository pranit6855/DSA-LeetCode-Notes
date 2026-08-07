# LeetCode 1493 - Longest Subarray of 1's After Deleting One Element

## 📌 Problem

Hume binary array `nums` diya hai.

Hume **exactly one element delete** karna hai aur uske baad longest non-empty subarray find karna hai jisme sirf `1`s ho.

### Example

```text
nums = [1,1,0,1]
```

Agar `0` delete kar dein:

```text
[1,1,0,1]
     ❌

→ [1,1,1]
```

Answer:

```text
3
```

---

# 🧠 Main Idea

Window ke andar maximum **1 zero** allow karenge.

```text
zero_count <= 1 → valid window ✅

zero_count > 1  → invalid window ❌
```

Agar 1 se jyada zero aa gaya to `low` se window shrink karenge.

---

# 🔥 Why Allow 1 Zero?

Because hume exactly **one element delete** karna hai.

Example:

```text
[1,1,0,1]
```

Window me:

```text
zero_count = 1
```

Ye valid hai because us single `0` ko delete kar sakte hain:

```text
[1,1,0,1]
     ❌

→ [1,1,1]
```

---

# 🔥 Variables

```cpp
int n = nums.size();
int low = 0;
int high = 0;
int zero_count = 0;
int max_len = 0;
```

Meaning:

```text
low        → window ka left pointer

high       → window ka right pointer

zero_count → current window me kitne zeros hain

max_len    → deletion ke baad maximum 1s
```

---

# 🔥 Step 1 - Expand Window

`high` se elements add karte jayenge.

Agar:

```cpp
nums[high] == 0
```

hai:

```cpp
zero_count++;
```

Code:

```cpp
if(nums[high] == 0){
    zero_count++;
}
```

---

# 🔥 Step 2 - Invalid Window Check

Maximum 1 zero allowed hai.

So:

```cpp
while(zero_count > 1)
```

Agar 1 se jyada zero aa gaya:

```text
window invalid ❌
```

Ab left se shrink karenge.

---

# 🔥 Step 3 - Shrink Window

```cpp
if(nums[low] == 0){
    zero_count--;
}

low++;
```

Agar outgoing element `0` tha:

```text
zero_count--
```

Agar outgoing element `1` tha:

```text
zero_count same
```

`low` har case me aage jayega.

---

# 🔹 Example of Shrinking

Suppose:

```text
[1,1,0,1,0]
```

Current:

```text
zero_count = 2
```

But allowed:

```text
1 zero
```

So invalid ❌

Shrink:

```text
[1,1,0,1,0]
 ↑
low
```

Pehla `1` remove:

```text
zero_count = 2
```

Still invalid.

Next `1` remove:

```text
zero_count = 2
```

Still invalid.

Ab `0` remove:

```text
zero_count = 1
```

Now window valid ✅

---

# 🔥 Most Important Part - Length

Normally sliding window me length hoti hai:

```cpp
high - low + 1
```

Lekin is question me **exactly one element delete karna compulsory hai**.

Isliye:

```text
window size - 1
```

Answer me lenge.

Window size:

```text
high - low + 1
```

Delete one:

```text
(high - low + 1) - 1
```

Simplify:

```text
high - low
```

Therefore:

```cpp
int len = high - low;
```

---

# 🔥 Why `high-low`?

Example:

```text
[1,1,0,1]
```

Indices:

```text
 0 1 2 3
```

Window size:

```text
high-low+1

= 3-0+1
= 4
```

But ek element delete karna hai:

```text
4 - 1 = 3
```

And:

```text
high-low

= 3-0
= 3
```

So directly:

```cpp
int len = high-low;
```

---

# ⚠️ What If There Is No Zero?

Important case:

```text
nums = [1,1,1]
```

Question says:

```text
exactly one element delete karna hai
```

Even though koi zero nahi hai, hume ek `1` delete karna padega.

So:

```text
[1,1,1]
```

becomes:

```text
[1,1]
```

Answer:

```text
2
```

Our formula:

```text
high-low
```

automatically gives:

```text
2
```

So separate handling ki zarurat nahi.

---

# 💻 C++ Code

```cpp
class Solution {
public:
    int longestSubarray(vector<int>& nums) {

        int n = nums.size();

        int low = 0;
        int high = 0;

        int zero_count = 0;
        int max_len = 0;

        while(high < n){

            // Incoming zero
            if(nums[high] == 0){
                zero_count++;
            }

            // More than one zero -> invalid
            while(zero_count > 1){

                if(nums[low] == 0){
                    zero_count--;
                }

                low++;
            }

            // Exactly one element delete karna hai
            int len = high - low;

            max_len = max(max_len, len);

            high++;
        }

        return max_len;
    }
};
```

---

# 🔄 Dry Run

Given:

```text
nums = [1,1,0,1]
```

Start:

```text
low = 0
high = 0
zero_count = 0
```

### Window 1

```text
[1]
```

```text
zero_count = 0
len = high-low = 0
```

---

### Window 2

```text
[1,1]
```

```text
zero_count = 0
len = 1
```

Why `1`?

Because exactly one element delete karna compulsory hai:

```text
[1,1]
 ↓ delete one

[1]
```

---

### Window 3

```text
[1,1,0]
```

```text
zero_count = 1
```

Valid.

Delete `0`:

```text
[1,1]
```

So:

```text
len = high-low
    = 2
```

---

### Window 4

```text
[1,1,0,1]
```

```text
zero_count = 1
```

Delete `0`:

```text
[1,1,1]
```

Length:

```text
3
```

Therefore:

```text
max_len = 3
```

Answer:

```text
3
```

---

# 🔥 LC 1004 vs LC 1493

Both ka sliding-window logic almost same hai.

## LC 1004

Maximum `k` zeros **flip** kar sakte the.

Condition:

```cpp
while(zero_count > k)
```

Answer:

```cpp
int len = high-low+1;
```

Because zero delete nahi ho raha, flip ho raha hai.

---

## LC 1493

Exactly one element **delete** karna hai.

Condition:

```cpp
while(zero_count > 1)
```

Answer:

```cpp
int len = high-low;
```

Because window me se exactly one element delete hoga.

---

# 🧠 Difference Yaad Rakho

```text
LC 1004
-------
k zeros FLIP
      ↓
zero_count <= k
      ↓
length = high-low+1
```

```text
LC 1493
-------
1 element DELETE
      ↓
zero_count <= 1
      ↓
length = high-low
```

---

# ⏱️ Time Complexity

`high` poore array par ek baar move karta hai.

`low` bhi sirf forward move karta hai.

Therefore:

```text
Time Complexity = O(n)
```

Nested `while` hone ke baad bhi `O(n²)` nahi hai.

---

# 💾 Space Complexity

Sirf variables use ho rahe hain.

```text
Space Complexity = O(1)
```

---

# ⚠️ Common Mistakes

### 1. `zero_count > 1` ki jagah `>= 1`

Wrong:

```cpp
while(zero_count >= 1)
```

Correct:

```cpp
while(zero_count > 1)
```

Ek zero allowed hai because usko delete kar sakte hain.

---

### 2. Length `high-low+1`

Wrong:

```cpp
int len = high-low+1;
```

Ye deletion ko account nahi karta.

Correct:

```cpp
int len = high-low;
```

Because exactly one element delete karna hai.

---

### 3. All Ones Case

```text
[1,1,1]
```

Answer `3` nahi hoga.

Exactly one delete compulsory:

```text
[1,1]
```

Answer:

```text
2
```

`high-low` automatically is case ko handle karta hai.

---

# 🔥 Quick Revision

```text
high se expand
      ↓
nums[high] == 0 ?
      ↓
zero_count++
      ↓
zero_count > 1 ?
      ↓ YES
low se shrink
      ↓
nums[low] == 0 ?
      ↓
zero_count--
      ↓
low++
      ↓
window valid
      ↓
len = high-low
      ↓
max_len update
      ↓
high++
```

---

# ⭐ One-Line Revision

> Window me maximum 1 zero allow karo; second zero aate hi left se shrink karo, aur exactly one deletion ko account karne ke liye valid window ki length `high-low` lo.

## Pattern

```text
Variable Sliding Window
+
At Most One Zero
+
Shrink Until Valid
+
Delete One Element
+
Maximum Length
```
