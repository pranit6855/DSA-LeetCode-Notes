# LeetCode 1004 - Max Consecutive Ones III

## 📌 Problem

Hume ek binary array `nums` aur integer `k` diya hai.

```text
nums = [1,1,1,0,0,0,1,1,1,1,0]
k = 2
```

Hum maximum `k` zeros ko `1` me flip kar sakte hain.

Hume maximum consecutive `1`s ki length find karni hai.

---

# 🧠 Main Idea

Agar current window me:

```text
zero_count <= k
```

hai, to window valid hai ✅

Kyunki window ke saare zeros ko flip karke `1` bana sakte hain.

Agar:

```text
zero_count > k
```

ho gaya, window invalid hai ❌

Tab `low` se window shrink karenge jab tak:

```text
zero_count <= k
```

wapas na ho jaye.

---

# 🔹 Example

```text
nums = [1,1,1,0,0]
k = 2
```

Window me:

```text
1 1 1 0 0
      ↑ ↑
```

2 zeros hain.

Hum maximum 2 zeros flip kar sakte hain:

```text
1 1 1 1 1
```

So poori window valid hai.

Length:

```text
5
```

---

# ❌ Invalid Window

Suppose:

```text
nums = [1,1,1,0,0,0]
k = 2
```

Window me:

```text
1 1 1 0 0 0
      ↑ ↑ ↑
```

3 zeros hain.

But:

```text
k = 2
```

Sirf 2 zeros flip kar sakte hain.

So:

```text
zero_count = 3
3 > 2
```

Window invalid ❌

Ab left se shrink karenge.

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
zero_count → current window me zeros ki count
max_len    → longest valid window
```

---

# 🔥 Step 1 - Expand Window

`high` se new element window me add hota hai.

Agar new element `0` hai:

```cpp
if(nums[high] == 0){
    zero_count++;
}
```

Example:

```text
[1 1 0]
     ↑
    high
```

Then:

```text
zero_count = 1
```

---

# 🔥 Step 2 - Check Window Valid Hai Ya Nahi

Condition:

```cpp
while(zero_count > k)
```

Meaning:

```text
allowed zeros se zyada zeros aa gaye
```

So window invalid hai.

Ab shrink karna padega.

---

# 🔥 Step 3 - Shrink From Left

```cpp
if(nums[low] == 0){
    zero_count--;
}

low++;
```

Agar left se jo element remove hua wo `0` tha:

```text
zero_count--
```

Otherwise agar `1` tha:

```text
zero_count same rahega
```

But `low` har case me move karega.

---

# 🔹 Important Example

Suppose:

```text
window = [1,1,1,0,0,0]
k = 2
```

Current:

```text
zero_count = 3
```

Invalid ❌

Left:

```text
[1,1,1,0,0,0]
 ↑
low
```

First `1` remove:

```text
zero_count = 3
```

Still invalid.

Again `1` remove:

```text
zero_count = 3
```

Still invalid.

Again `1` remove:

```text
zero_count = 3
```

Still invalid.

Ab `0` remove:

```text
zero_count = 2
```

Now:

```text
zero_count <= k
```

Window valid ✅

Isi liye `if` nahi, `while` use karte hain:

```cpp
while(zero_count > k)
```

Kyunki valid banane ke liye multiple elements remove karne pad sakte hain.

---

# 🔥 Step 4 - Calculate Length

Window valid hone ke baad:

```cpp
int len = high - low + 1;
```

Then:

```cpp
max_len = max(max_len, len);
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    int longestOnes(vector<int>& nums, int k) {

        int n = nums.size();
        int low = 0;
        int high = 0;
        int zero_count = 0;
        int max_len = 0;

        while(high < n){

            // New zero enter hua
            if(nums[high] == 0){
                zero_count++;
            }

            // Invalid window -> shrink
            while(zero_count > k){

                if(nums[low] == 0){
                    zero_count--;
                }

                low++;
            }

            // Valid window
            int len = high - low + 1;

            max_len = max(max_len, len);

            high++;
        }

        return max_len;
    }
};
```

---

# 🔥 Main Pattern

### Expand

```cpp
if(nums[high] == 0){
    zero_count++;
}
```

### Invalid?

```cpp
while(zero_count > k)
```

### Shrink

```cpp
if(nums[low] == 0){
    zero_count--;
}

low++;
```

### Answer

```cpp
int len = high-low+1;
max_len = max(max_len,len);
```

---

# 🧠 Why Variable Sliding Window?

Window ka size fixed nahi hai.

Kabhi:

```text
[1,1,1,0,0]
```

size `5` valid ho sakta hai.

Lekin next `0` aate hi:

```text
[1,1,1,0,0,0]
```

invalid ho sakta hai.

Tab `low` move karke window chhoti karni padti hai.

Isliye:

```text
Variable Size Sliding Window
```

---

# 🔥 LC 3 Se Connection

### LC 3

Invalid condition:

```text
duplicate character
```

So:

```text
duplicate
   ↓
shrink
```

### LC 1004

Invalid condition:

```text
zero_count > k
```

So:

```text
too many zeros
     ↓
   shrink
```

General pattern same:

```text
EXPAND
   ↓
INVALID?
   ↓
SHRINK
   ↓
VALID
   ↓
UPDATE ANSWER
```

---

# ⏱️ Time Complexity

`high` har element par maximum ek baar move karta hai.

`low` bhi har element par maximum ek baar move karta hai.

Therefore:

```text
Time Complexity = O(n)
```

Nested `while` hone ke baad bhi `O(n²)` nahi hota because `low` backward nahi jata.

---

# 💾 Space Complexity

Sirf variables use kiye hain:

```text
low
high
zero_count
max_len
```

Therefore:

```text
Space Complexity = O(1)
```

---

# ⚠️ Common Mistakes

## 1. Wrong invalid condition

Wrong:

```cpp
while(zero_count >= k)
```

Correct:

```cpp
while(zero_count > k)
```

Because exactly `k` zeros allowed hain.

---

## 2. Har outgoing element par zero_count decrease karna

Wrong:

```cpp
zero_count--;
low++;
```

Correct:

```cpp
if(nums[low] == 0){
    zero_count--;
}

low++;
```

Sirf zero window se nikla tab count decrease hoga.

---

## 3. `if` instead of `while`

Wrong:

```cpp
if(zero_count > k)
```

Better:

```cpp
while(zero_count > k)
```

Window ko valid banane ke liye multiple elements remove karne pad sakte hain.

---

## 4. Invalid window par answer update karna

Pehle:

```text
zero_count <= k
```

ensure karo.

Uske baad:

```cpp
max_len = max(max_len, high-low+1);
```

---

# 🔄 Quick Revision

```text
high se expand
      ↓
nums[high] == 0
      ↓
zero_count++
      ↓
zero_count > k ?
      ↓ YES
low se shrink
      ↓
nums[low] == 0 ?
      ↓ YES
zero_count--
      ↓
low++
      ↓
window valid
      ↓
len = high-low+1
      ↓
max_len update
      ↓
high++
```

---

# ⭐ One-Line Revision

> Window me maximum `k` zeros allow karo; `k` se zyada zeros hote hi left se shrink karo, aur valid window ki maximum length maintain karo.

---

## Pattern

```text
Variable Sliding Window
+
Count Invalid Elements
+
Shrink Until Valid
+
Maximum Length
```
