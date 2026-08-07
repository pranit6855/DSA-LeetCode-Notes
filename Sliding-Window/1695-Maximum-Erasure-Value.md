# LeetCode 1695 - Maximum Erasure Value

## 📌 Problem

Hume ek positive integer array `nums` diya hai.

Hume ek **continuous subarray** choose karna hai jisme:

```text
saare elements unique hone chahiye
```

Aur us unique subarray ka **maximum sum** return karna hai.

---

# 🔹 Example

```text
nums = [4,2,4,5,6]
```

Possible subarrays:

```text
[4,2]       → unique ✅ → sum = 6

[4,2,4]     → 4 repeat ❌

[2,4,5,6]   → unique ✅ → sum = 17
```

So answer:

```text
17
```

---

# 🧠 Main Idea

Ye **LC 3 - Longest Substring Without Repeating Characters** jaisa hai.

Difference:

```text
LC 3
→ unique characters
→ maximum LENGTH

LC 1695
→ unique numbers
→ maximum SUM
```

Current window me numbers ki frequency maintain karne ke liye:

```cpp
unordered_map<int,int> mp;
```

Aur current window ka sum:

```cpp
int sum = 0;
```

maintain karenge.

---

# 🔥 Variables

```cpp
int n = nums.size();
int low = 0;
int high = 0;
int sum = 0;
int max_sum = 0;

unordered_map<int,int> mp;
```

Meaning:

```text
low     → window ka left pointer

high    → window ka right pointer

sum     → current window ka sum

max_sum → maximum unique subarray sum

mp      → current window me har number ki frequency
```

---

# 🔥 Step 1 - Expand Window

`high` wala element current window me add karenge.

Frequency:

```cpp
mp[nums[high]]++;
```

Sum:

```cpp
sum += nums[high];
```

---

# 🔥 Step 2 - Duplicate Check

Agar newly added number ki frequency:

```text
> 1
```

ho gayi, number repeat ho gaya.

Condition:

```cpp
while(mp[nums[high]] > 1)
```

Matlab:

```text
current window invalid ❌
```

Ab left se shrink karna hai.

---

# 🔥 Example

```text
nums = [4,2,4]
```

First:

```text
[4]
```

Map:

```text
4 → 1
```

Sum:

```text
4
```

Next:

```text
[4,2]
```

Map:

```text
4 → 1
2 → 1
```

Sum:

```text
6
```

Valid ✅

Next `4` add:

```text
[4,2,4]
```

Map:

```text
4 → 2 ❌
2 → 1
```

Sum:

```text
10
```

`4` duplicate hai.

So window invalid.

---

# 🔥 Step 3 - Shrink Window

Duplicate hone par left se elements remove karenge.

Frequency decrease:

```cpp
mp[nums[low]]--;
```

Current sum se outgoing element remove:

```cpp
sum -= nums[low];
```

Then:

```cpp
low++;
```

---

# 🔄 Example of Shrinking

Current:

```text
[4,2,4]
 ↑   ↑
low high
```

Map:

```text
4 → 2
2 → 1
```

Sum:

```text
10
```

Duplicate:

```text
4 → 2
```

Left wala `4` remove:

```cpp
mp[4]--;
```

Map:

```text
4 → 1
2 → 1
```

Sum:

```text
10 - 4 = 6
```

Window:

```text
4 [2,4]
```

Now:

```text
4 → 1
```

Duplicate khatam ✅

Current valid window:

```text
[2,4]
```

Sum:

```text
6
```

---

# 🔥 Step 4 - Maximum Sum

Duplicate remove hone ke baad window valid hai.

So:

```cpp
max_sum = max(max_sum, sum);
```

---

# 🔥 Continue Example

Array:

```text
[4,2,4,5,6]
```

Duplicate remove karne ke baad:

```text
[2,4]
```

Sum:

```text
6
```

Next `5`:

```text
[2,4,5]
```

Sum:

```text
2 + 4 + 5 = 11
```

All unique ✅

```text
max_sum = 11
```

Next `6`:

```text
[2,4,5,6]
```

Sum:

```text
2 + 4 + 5 + 6 = 17
```

All unique ✅

So:

```text
max_sum = 17
```

Answer:

```text
17
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    int maximumUniqueSubarray(vector<int>& nums) {

        int n = nums.size();
        int low = 0;
        int high = 0;

        int sum = 0;
        int max_sum = 0;

        unordered_map<int,int> mp;

        while(high < n){

            // Incoming element
            mp[nums[high]]++;
            sum += nums[high];

            // Duplicate -> shrink
            while(mp[nums[high]] > 1){

                mp[nums[low]]--;

                sum -= nums[low];

                if(mp[nums[low]] == 0){
                    mp.erase(nums[low]);
                }

                low++;
            }

            // Valid unique window
            max_sum = max(max_sum, sum);

            high++;
        }

        return max_sum;
    }
};
```

---

# 🔥 Why Map?

Map current window me har number ki frequency batata hai.

Example:

```text
window = [4,2,5]
```

Map:

```text
4 → 1
2 → 1
5 → 1
```

All unique.

But:

```text
window = [4,2,4]
```

Map:

```text
4 → 2 ❌
2 → 1
```

`4 → 2` se pata chala ki duplicate aa gaya.

---

# 🔥 Why `sum` Maintain Karte Hain?

Har baar poori window ka sum dobara calculate karna inefficient hoga.

Instead:

### Incoming

```cpp
sum += nums[high];
```

### Outgoing

```cpp
sum -= nums[low];
```

So `sum` hamesha current window ka sum represent karega.

---

# 🔥 Why `while`, Not `if`?

Suppose:

```text
nums = [1,2,3,2]
```

Current:

```text
[1,2,3,2]
```

`2` duplicate hai.

Map:

```text
1 → 1
2 → 2
3 → 1
```

Pehle `1` remove:

```text
[2,3,2]
```

Still:

```text
2 → 2
```

Duplicate abhi bhi hai.

Ab old `2` remove:

```text
[3,2]
```

Now:

```text
2 → 1
```

Valid.

Isliye:

```cpp
while(mp[nums[high]] > 1)
```

use karna hai.

---

# 🔥 `erase()` Kyu?

Shrink ke time:

```cpp
mp[nums[low]]--;
```

Agar frequency `0` ho gayi:

```cpp
if(mp[nums[low]] == 0){
    mp.erase(nums[low]);
}
```

Map se wo key completely remove kar dete hain.

Example:

```text
5 → 1
```

Remove karne ke baad:

```text
5 → 0
```

Current window me `5` present nahi hai.

So:

```cpp
mp.erase(5);
```

---

# 🧠 LC 3 vs LC 1695

```text
LC 3
--------------------------------
String
Unique characters
Map frequency
Duplicate → shrink
Answer → max length
```

```text
LC 1695
--------------------------------
Integer array
Unique numbers
Map frequency
Duplicate → shrink
Answer → max sum
```

Main sliding-window logic same hai.

---

# 🔥 General Pattern

```text
high se element add
        ↓
frequency++
        ↓
sum me add
        ↓
duplicate?
        ↓ YES
low se remove
        ↓
frequency--
sum se subtract
low++
        ↓
duplicate khatam hone tak shrink
        ↓
valid unique window
        ↓
max_sum update
        ↓
high++
```

---

# ⏱️ Time Complexity

`high` har element ko ek baar add karta hai.

`low` har element ko maximum ek baar remove karta hai.

So:

```text
Time Complexity = O(n)
```

Nested `while` ke baad bhi `O(n²)` nahi hai.

---

# 💾 Space Complexity

Map current unique elements store karta hai.

Worst case:

```text
O(n)
```

So:

```text
Space Complexity = O(n)
```

---

# ⚠️ Common Mistakes

## 1. Duplicate hone ke baad sum update na karna

Wrong:

```cpp
mp[nums[low]]--;
low++;
```

Correct:

```cpp
mp[nums[low]]--;
sum -= nums[low];
low++;
```

Outgoing element ko sum se bhi remove karna hai.

---

## 2. `if` instead of `while`

Wrong:

```cpp
if(mp[nums[high]] > 1)
```

Correct:

```cpp
while(mp[nums[high]] > 1)
```

Duplicate remove karne ke liye multiple elements hatane pad sakte hain.

---

## 3. Length maximize karna

Is question me:

```cpp
high-low+1
```

maximize nahi karna.

Hume:

```cpp
sum
```

maximize karna hai.

So:

```cpp
max_sum = max(max_sum, sum);
```

---

## 4. Wrong erase syntax

Wrong:

```cpp
mp.erase(mp[nums[low]]);
```

Correct:

```cpp
mp.erase(nums[low]);
```

`erase()` ko **key** dete hain, frequency nahi.

---

# 🔥 Quick Revision

```text
nums[high] add
      ↓
mp[nums[high]]++
      ↓
sum += nums[high]
      ↓
duplicate?
      ↓ YES
mp[nums[low]]--
sum -= nums[low]
low++
      ↓
duplicate khatam
      ↓
max_sum = max(max_sum,sum)
      ↓
high++
```

---

# ⭐ One-Line Revision

> High se numbers add karo aur sum maintain karo; duplicate aate hi low se elements remove karo jab tak window unique na ho jaye, phir current sum se maximum update karo.

## Pattern

```text
Variable Sliding Window
+
Frequency Map
+
Unique Elements
+
Running Sum
+
Maximum Sum
```
