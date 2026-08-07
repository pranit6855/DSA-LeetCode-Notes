# LeetCode 1208 - Get Equal Substrings Within Budget

## 📌 Problem

Hume do same-length strings `s` aur `t` aur ek integer `maxCost` diya hai.

Har position par:

```text
s[i] → t[i]
```

convert karne ki cost:

```cpp
abs(s[i] - t[i])
```

hoti hai.

Hume longest continuous substring find karni hai jisko `s` se `t` me convert karne ka total cost:

```text
<= maxCost
```

ho.

---

# 🔹 Example

```text
s = "abcd"
t = "bcdf"

maxCost = 3
```

Har position ki conversion cost:

```text
a → b = 1
b → c = 1
c → d = 1
d → f = 2
```

So problem ko basically aise dekh sakte hain:

```text
cost = [1,1,1,2]
budget = 3
```

Hume longest continuous part chahiye jiska sum:

```text
<= 3
```

ho.

---

# 🧠 Main Idea

Variable size sliding window use karenge.

`high` se window expand karte waqt current conversion cost add karenge:

```cpp
cost += abs(s[high] - t[high]);
```

Agar:

```text
cost > maxCost
```

ho gaya, current window budget ke bahar hai ❌

Tab `low` se window shrink karenge.

---

# 🔥 Variables

```cpp
int n = s.size();
int low = 0;
int high = 0;
int cost = 0;
int max_len = 0;
```

Meaning:

```text
low     → window ka left pointer

high    → window ka right pointer

cost    → current window ka total conversion cost

max_len → longest valid window
```

---

# 🔥 Step 1 - Expand Window

Current position ki conversion cost:

```cpp
abs(s[high] - t[high])
```

Current total cost me add:

```cpp
cost += abs(s[high] - t[high]);
```

Example:

```text
s = a b c d
t = b c d f

cost:

1 1 1 2
```

As `high` moves:

```text
high = 0 → cost = 1
high = 1 → cost = 2
high = 2 → cost = 3
high = 3 → cost = 5
```

---

# 🔥 Step 2 - Budget Check

Valid window:

```text
cost <= maxCost
```

Invalid window:

```text
cost > maxCost
```

So:

```cpp
while(cost > maxCost)
```

use karenge.

---

# 🔥 Step 3 - Shrink Window

Budget cross hone par leftmost position ki conversion cost remove karenge:

```cpp
cost -= abs(s[low] - t[low]);
low++;
```

Jab tak:

```text
cost > maxCost
```

hai, shrink karte rahenge.

---

# 🔹 Example of Shrinking

Given:

```text
cost array = [1,1,1,2]
maxCost = 3
```

Current window:

```text
[1,1,1,2]
```

Total:

```text
cost = 5
```

Invalid ❌

Remove left:

```text
5 - 1 = 4
```

Window:

```text
1 [1,1,2]
```

Still:

```text
4 > 3
```

Invalid ❌

Again remove left:

```text
4 - 1 = 3
```

Window:

```text
1 1 [1,2]
```

Now:

```text
3 <= 3
```

Valid ✅

Shrink stop.

---

# 🔥 Step 4 - Maximum Length

Window valid hone ke baad:

```cpp
int len = high-low+1;
```

Maximum update:

```cpp
max_len = max(max_len,len);
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    int equalSubstring(string s, string t, int maxCost) {

        int n = s.size();
        int low = 0;
        int high = 0;
        int cost = 0;
        int max_len = 0;

        while(high < n){

            // Incoming conversion cost
            cost += abs(s[high] - t[high]);

            // Budget cross -> shrink
            while(cost > maxCost){

                cost -= abs(s[low] - t[low]);
                low++;
            }

            // Valid window
            int len = high-low+1;

            max_len = max(max_len,len);

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
s = "abcd"
t = "bcdf"
maxCost = 3
```

Conversion costs:

```text
[1,1,1,2]
```

### Window 1

```text
[1]
```

```text
cost = 1
```

Valid:

```text
1 <= 3
```

Length:

```text
1
```

---

### Window 2

```text
[1,1]
```

```text
cost = 2
```

Valid.

Length:

```text
2
```

---

### Window 3

```text
[1,1,1]
```

```text
cost = 3
```

Valid.

Length:

```text
3
```

So:

```text
max_len = 3
```

---

### Window 4

```text
[1,1,1,2]
```

```text
cost = 5
```

But:

```text
maxCost = 3
```

Invalid ❌

Shrink:

```text
5 - 1 = 4
```

Still invalid.

Shrink again:

```text
4 - 1 = 3
```

Valid.

Current window:

```text
[1,2]
```

Length:

```text
2
```

Maximum still:

```text
3
```

So answer:

```text
3
```

---

# 🧠 Most Important Observation

Is question ko strings ka question samajhne ki jagah:

```text
Longest subarray with sum <= maxCost
```

samjho.

Difference bas itna hai ki array directly nahi diya.

Har position ka value hum calculate kar rahe hain:

```cpp
abs(s[i] - t[i])
```

---

# 🔥 Why `abs()`?

Characters internally numeric values represent karte hain.

Example:

```text
'a' = 97
'b' = 98
```

So:

```cpp
abs('a' - 'b')
```

becomes:

```text
abs(97 - 98)
= 1
```

Similarly:

```cpp
abs('d' - 'f')
```

```text
abs(100 - 102)
= 2
```

---

# 🔥 Sliding Window Pattern

### Expand

```cpp
cost += abs(s[high] - t[high]);
```

### Invalid?

```cpp
while(cost > maxCost)
```

### Shrink

```cpp
cost -= abs(s[low] - t[low]);
low++;
```

### Answer

```cpp
int len = high-low+1;
max_len = max(max_len,len);
```

---

# 🧠 LC 1004 Se Connection

### LC 1004

Invalid condition:

```text
zero_count > k
```

Then:

```text
shrink
```

### LC 1208

Invalid condition:

```text
cost > maxCost
```

Then:

```text
shrink
```

General pattern:

```text
EXPAND
   ↓
INVALID CONDITION?
   ↓
SHRINK
   ↓
VALID
   ↓
MAX LENGTH
```

---

# ⏱️ Time Complexity

`high` poori string par ek baar move karta hai.

`low` bhi sirf forward move karta hai.

```text
Time Complexity = O(n)
```

Nested `while` ke baad bhi `O(n²)` nahi hai because `low` kabhi backward nahi jata.

---

# 💾 Space Complexity

Koi map ya extra array use nahi kiya.

```text
Space Complexity = O(1)
```

---

# ⚠️ Common Mistakes

## 1. Wrong condition

Wrong:

```cpp
while(cost >= maxCost)
```

Correct:

```cpp
while(cost > maxCost)
```

Because exactly `maxCost` spend karna allowed hai.

---

## 2. Outgoing cost remove na karna

Window shrink karte waqt:

```cpp
cost -= abs(s[low] - t[low]);
```

karna compulsory hai.

Uske baad:

```cpp
low++;
```

---

## 3. `abs()` bhool jana

Wrong:

```cpp
s[high] - t[high]
```

Difference negative bhi ho sakta hai.

Correct:

```cpp
abs(s[high] - t[high])
```

---

## 4. Invalid window ka length lena

Pehle:

```cpp
while(cost > maxCost)
```

se window valid banao.

Uske baad:

```cpp
int len = high-low+1;
```

calculate karo.

---

# 🔥 Quick Revision

```text
high se expand
      ↓
abs(s[high]-t[high])
      ↓
cost me add
      ↓
cost > maxCost ?
      ↓ YES
low ki cost remove
      ↓
low++
      ↓
valid hone tak shrink
      ↓
len = high-low+1
      ↓
max_len update
      ↓
high++
```

---

# ⭐ One-Line Revision

> Har index ki conversion cost current window me add karo; total cost budget se zyada hote hi left se shrink karo, aur valid window ki maximum length maintain karo.

## Pattern

```text
Variable Sliding Window
+
Running Cost
+
Shrink When Cost > Budget
+
Maximum Length
```
