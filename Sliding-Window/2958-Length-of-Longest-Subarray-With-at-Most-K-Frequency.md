# LeetCode 2958 - Length of Longest Subarray With at Most K Frequency

## 📌 Problem

Hume integer array `nums` aur integer `k` diya hai.

Hume **longest continuous subarray** find karna hai jisme har element maximum `k` times present ho.

### Example

```text
nums = [1,2,3,1,2,3,1,2]
k = 2
```

Valid window:

```text
[1,2,3,1,2,3]
```

Frequency:

```text
1 → 2
2 → 2
3 → 2
```

Har element maximum `2` times hai, so valid ✅

Next `1` add karne par:

```text
[1,2,3,1,2,3,1]
```

Frequency:

```text
1 → 3 ❌
2 → 2
3 → 2
```

`1` ki frequency `k` se zyada ho gayi, so window invalid.

---

# 🧠 Main Idea

Current window ke elements ki frequency maintain karne ke liye:

```cpp
unordered_map<int,int> mp;
```

use karenge.

Rule:

```text
frequency <= k → valid ✅

frequency > k  → invalid ❌
```

Invalid hone par `low` se window shrink karenge.

---

# 🔥 Variables

```cpp
int n=nums.size();
int low=0;
int high=0;
int max_len=0;

unordered_map<int,int> mp;
```

Meaning:

```text
low     → window ka left pointer
high    → window ka right pointer
max_len → longest valid window
mp      → current window ke elements ki frequency
```

---

# 🔥 Step 1 - Expand Window

`high` wala element current window me add:

```cpp
mp[nums[high]]++;
```

Example:

```text
[1,2,3,1]
```

Map:

```text
1 → 2
2 → 1
3 → 1
```

---

# 🔥 Step 2 - Frequency Limit Check

Newly added element ki frequency check karenge:

```cpp
mp[nums[high]]
```

Agar:

```text
mp[nums[high]] > k
```

ho gaya, current window invalid hai.

So:

```cpp
while(mp[nums[high]] > k)
```

se shrink karenge.

---

# 🔥 Step 3 - Shrink Window

Leftmost element ki frequency decrease:

```cpp
mp[nums[low]]--;
```

Agar frequency `0` ho gayi:

```cpp
if(mp[nums[low]]==0){
    mp.erase(nums[low]);
}
```

Then:

```cpp
low++;
```

---

# 🔹 Shrinking Example

Given:

```text
nums = [1,2,3,1,2,3,1]
k = 2
```

Current map:

```text
1 → 3 ❌
2 → 2
3 → 2
```

Current:

```text
[1,2,3,1,2,3,1]
 ↑           ↑
low         high
```

Newly added `1` ki frequency:

```text
3 > 2
```

So left se remove:

```text
1 → 3
```

becomes:

```text
1 → 2
```

Then:

```text
low++
```

New window:

```text
1 [2,3,1,2,3,1]
```

Frequency:

```text
1 → 2
2 → 2
3 → 2
```

Window valid again ✅

---

# 🔥 Step 4 - Calculate Length

Window valid hone ke baad:

```cpp
int len=high-low+1;
```

Maximum length:

```cpp
max_len=max(len,max_len);
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    int maxSubarrayLength(vector<int>& nums, int k) {

        int n=nums.size();
        int low=0;
        int high=0;
        int max_len=0;

        unordered_map<int,int> mp;

        while(high<n){

            // Incoming element
            mp[nums[high]]++;

            // Frequency k se jyada -> shrink
            while(mp[nums[high]]>k){

                mp[nums[low]]--;

                if(mp[nums[low]]==0){
                    mp.erase(nums[low]);
                }

                low++;
            }

            // Valid window
            int len=high-low+1;

            max_len=max(len,max_len);

            high++;
        }

        return max_len;
    }
};
```

---

# 🔥 Straight Flow

```text
START
  ↓
high se element add
  ↓
mp[nums[high]]++
  ↓
frequency > k ?
  ↓
YES → low se remove
  ↓
mp[nums[low]]--
  ↓
frequency 0 → erase
  ↓
low++
  ↓
jab tak frequency > k, shrink
  ↓
WINDOW VALID
  ↓
len = high-low+1
  ↓
max_len update
  ↓
high++
  ↓
REPEAT
```

---

# 🧠 Why `while` Instead of `if`?

Suppose:

```text
nums = [1,2,3,2]
k = 1
```

Current:

```text
[1,2,3,2]
```

Frequency:

```text
1 → 1
2 → 2 ❌
3 → 1
```

First `1` remove karne par:

```text
[2,3,2]
```

Still:

```text
2 → 2 ❌
```

So ek removal enough nahi tha.

Old `2` bhi remove karna padega:

```text
[3,2]
```

Now:

```text
2 → 1
3 → 1
```

Valid ✅

Isliye:

```cpp
while(mp[nums[high]]>k)
```

use karte hain.

---

# 🧠 Why `mp[nums[high]]` Check?

Duplicate/frequency violation **newly added `high` element** ki wajah se aati hai.

Example:

```text
[1,2,3,1,2,3,1]
            ↑
           high
```

New `1` add hua:

```text
1 → 3
```

So check:

```cpp
mp[nums[high]]
```

karna enough hai.

---

# 🔥 LC 1695 Se Connection

## LC 1695 - Maximum Erasure Value

Har element unique hona chahiye:

```text
frequency <= 1
```

Invalid:

```text
frequency > 1
```

## LC 2958

Har element maximum `k` times allowed:

```text
frequency <= k
```

Invalid:

```text
frequency > k
```

So LC 2958 ko LC 1695 ka generalized version samajh sakte hain:

```text
LC 1695 → maximum frequency = 1

LC 2958 → maximum frequency = k
```

---

# ⏱️ Time Complexity

`high` har element ko ek baar add karta hai.

`low` bhi sirf forward move karta hai.

```text
Time Complexity = O(n)
```

Average case with `unordered_map`.

---

# 💾 Space Complexity

Worst case me map me multiple distinct elements ho sakte hain:

```text
Space Complexity = O(n)
```

---

# ⚠️ Common Mistakes

## 1. Wrong condition

Wrong:

```cpp
while(mp[nums[high]] >= k)
```

Correct:

```cpp
while(mp[nums[high]] > k)
```

Exactly `k` occurrences allowed hain.

---

## 2. Wrong duplicate check

Don't check:

```cpp
mp[nums[low]] > k
```

Violation newly-added `high` element ki wajah se hui hai.

Use:

```cpp
mp[nums[high]] > k
```

---

## 3. Wrong erase syntax

Wrong:

```cpp
mp.erase(mp[nums[low]]);
```

Correct:

```cpp
mp.erase(nums[low]);
```

`erase()` ko **key** dete hain.

---

## 4. Answer Before Shrinking

Pehle window ko valid banao:

```cpp
while(mp[nums[high]]>k)
```

Uske baad:

```cpp
int len=high-low+1;
max_len=max(len,max_len);
```

---

# ⭐ One-Line Revision

> High se element add karo; agar uski frequency `k` se zyada ho jaye to low se shrink karo jab tak frequency `<= k` na ho, phir maximum window length update karo.

## Pattern

```text
Variable Sliding Window
+
Frequency Map
+
At Most K Frequency
+
Shrink Until Valid
+
Maximum Length
```
