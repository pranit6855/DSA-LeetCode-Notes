# LeetCode 1052 - Grumpy Bookstore Owner

## 📌 Problem

Hume do arrays diye gaye hain:

```text
customers
grumpy
```

`customers[i]` batata hai ki `i`th minute me kitne customers aaye.

`grumpy[i]` batata hai owner grumpy tha ya nahi:

```text
grumpy[i] = 0 → customers satisfied ✅
grumpy[i] = 1 → customers unsatisfied ❌
```

Owner ek special technique ko **exactly `minutes` continuous minutes** ke liye use kar sakta hai.

Technique use karne par us window ke `grumpy = 1` wale customers bhi satisfied ho jayenge.

Hume **maximum total satisfied customers** return karne hain.

---

# 🔹 Example

```text
customers = [1,0,1,2,1,1,7,5]
grumpy    = [0,1,0,1,0,1,0,1]

minutes = 3
```

---

# 🧠 Main Idea

Problem ko 2 parts me divide karenge:

```text
1. Already satisfied customers
2. Technique se extra satisfied customers
```

Final answer:

```text
answer = base + max_extra
```

Where:

```text
base      → jo customers already satisfied hain
max_extra → best minutes-size window se extra satisfied customers
```

---

# 1️⃣ Already Satisfied Customers

Jahan:

```text
grumpy[i] == 0
```

wahan customers already satisfied hain.

So:

```cpp
for(int i=0; i<n; i++){
    if(grumpy[i] == 0){
        base += customers[i];
    }
}
```

Example:

```text
customers = [4,2,6,3]
grumpy    = [0,1,1,0]
```

Already satisfied:

```text
4 + 3 = 7
```

So:

```text
base = 7
```

---

# 2️⃣ Technique Se Extra Customers

Technique ka benefit sirf wahan milega jahan:

```text
grumpy[i] == 1
```

Kyunki `grumpy = 0` wale customers already satisfied hain.

Suppose:

```text
customers = [4,2,6,3]
grumpy    = [0,1,1,0]

minutes = 2
```

Window:

```text
[4,2]
```

Grumpy:

```text
[0,1]
```

`4` already satisfied hai.

Technique se extra benefit sirf:

```text
2
```

So:

```text
extra = 2
```

---

# 🔥 Why Fixed Size Sliding Window?

Technique exactly:

```text
minutes
```

continuous minutes ke liye use hogi.

So:

```text
window size = minutes
```

hamesha fixed hai.

Therefore:

```text
Fixed Size Sliding Window
```

use karenge.

---

# 🔥 Variables

```cpp
int n = customers.size();
int low = 0;
int high = minutes - 1;

int base = 0;
int extra = 0;
```

Meaning:

```text
low  → window ka left index

high → window ka right index

base → already satisfied customers

extra → current window se extra satisfied customers
```

---

# 🔥 Step 1 - Calculate Base

```cpp
for(int i=0; i<n; i++){
    if(grumpy[i] == 0){
        base += customers[i];
    }
}
```

Sirf:

```text
grumpy == 0
```

wale customers add honge.

Kyunki wo technique ke bina bhi satisfied hain.

---

# 🔥 Step 2 - First Window

First `minutes` elements me check karenge ki technique use karne se kitne **extra customers** satisfied honge.

```cpp
for(int i=0; i<minutes; i++){
    if(grumpy[i] == 1){
        extra += customers[i];
    }
}
```

Notice:

```text
grumpy == 1
```

wale hi add kar rahe hain.

Kyunki wahi currently unsatisfied hain aur technique unko satisfied karegi.

---

# 🔥 Step 3 - Store First Window

```cpp
int max_extra = extra;
```

First window se jitna extra benefit mila, initially wahi maximum hai.

---

# 🔥 Step 4 - Slide Window

```cpp
while(high < n-1){

    low++;
    high++;
```

Window ko ek position right move kar diya.

Example:

```text
[0 1 2] 3 4 5
```

becomes:

```text
0 [1 2 3] 4 5
```

---

# 🔥 Step 5 - Outgoing Element

Old left element window se bahar gaya.

Lekin usko `extra` se tabhi remove karenge agar:

```text
grumpy[low-1] == 1
```

tha.

Code:

```cpp
if(grumpy[low-1] == 1){
    extra -= customers[low-1];
}
```

Kyun?

Because `extra` me hum sirf `grumpy = 1` wale customers maintain kar rahe hain.

---

# 🔥 Step 6 - Incoming Element

New `high` element window me enter hua.

Agar:

```text
grumpy[high] == 1
```

hai, technique us customer ko extra satisfied karegi.

So:

```cpp
if(grumpy[high] == 1){
    extra += customers[high];
}
```

---

# 🔥 Step 7 - Maximum Extra

Har window ke baad:

```cpp
max_extra = max(max_extra, extra);
```

Hum find kar rahe hain ki technique **kis window me use karne par sabse zyada extra customers satisfied honge**.

---

# 🔄 Small Dry Run

Given:

```text
customers = [4,2,6,3]
grumpy    = [0,1,1,0]

minutes = 2
```

## Base

`grumpy = 0` positions:

```text
4 and 3
```

So:

```text
base = 4 + 3
     = 7
```

---

## Window 1

```text
customers = [4,2]
grumpy    = [0,1]
```

Only grumpy customer:

```text
2
```

So:

```text
extra = 2
max_extra = 2
```

---

## Window 2

```text
customers = [2,6]
grumpy    = [1,1]
```

Both are grumpy.

Extra:

```text
2 + 6 = 8
```

So:

```text
max_extra = 8
```

---

## Window 3

```text
customers = [6,3]
grumpy    = [1,0]
```

Only `6` is grumpy.

So:

```text
extra = 6
```

Maximum still:

```text
max_extra = 8
```

---

# ✅ Final Answer

```text
base = 7
max_extra = 8
```

Therefore:

```text
answer = base + max_extra

       = 7 + 8

       = 15
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    int maxSatisfied(vector<int>& customers, vector<int>& grumpy, int minutes) {

        int n = customers.size();
        int low = 0;
        int high = minutes - 1;

        int base = 0;
        int extra = 0;

        // Already satisfied customers
        for(int i=0; i<n; i++){
            if(grumpy[i] == 0){
                base += customers[i];
            }
        }

        // First window
        for(int i=0; i<minutes; i++){
            if(grumpy[i] == 1){
                extra += customers[i];
            }
        }

        int max_extra = extra;

        // Remaining windows
        while(high < n-1){

            low++;
            high++;

            // Outgoing
            if(grumpy[low-1] == 1){
                extra -= customers[low-1];
            }

            // Incoming
            if(grumpy[high] == 1){
                extra += customers[high];
            }

            max_extra = max(max_extra, extra);
        }

        return base + max_extra;
    }
};
```

---

# 🔥 Most Important Concept

Do alag sums maintain ho rahe hain:

```text
base
 ↓
grumpy == 0 wale
 ↓
already satisfied
```

and:

```text
extra
 ↓
current window ke grumpy == 1 wale
 ↓
technique se extra satisfied
```

Finally:

```cpp
return base + max_extra;
```

---

# 🧠 Why `extra` Me Sirf `grumpy == 1`?

Suppose:

```text
customers = [4,2]
grumpy    = [0,1]
```

`4` customers already satisfied hain.

Agar technique use kari:

```text
4 → already satisfied
2 → newly satisfied
```

Technique ka **extra benefit**:

```text
2
```

hai, `6` nahi.

Isliye:

```cpp
if(grumpy[i] == 1){
    extra += customers[i];
}
```

---

# 🔥 Sliding Window Pattern

First window:

```cpp
for(int i=0; i<minutes; i++){
    if(grumpy[i] == 1){
        extra += customers[i];
    }
}
```

Then:

```cpp
while(high < n-1){

    low++;
    high++;

    if(grumpy[low-1] == 1){
        extra -= customers[low-1];
    }

    if(grumpy[high] == 1){
        extra += customers[high];
    }

    max_extra = max(max_extra, extra);
}
```

Same fixed-window pattern:

```text
OLD → remove
NEW → add
MAX → update
```

---

# ⏱️ Time Complexity

First loop:

```text
O(n)
```

Sliding window:

```text
O(n)
```

Total:

```text
O(n)
```

---

# 💾 Space Complexity

Koi extra array/map use nahi kiya.

```text
Space Complexity = O(1)
```

---

# ⚠️ Common Mistakes

### 1. `extra` me sab customers add karna

Wrong:

```cpp
extra += customers[i];
```

Correct:

```cpp
if(grumpy[i] == 1){
    extra += customers[i];
}
```

Because `grumpy = 0` wale already `base` me included hain.

---

### 2. Outgoing ko condition ke bina remove karna

Wrong:

```cpp
extra -= customers[low-1];
```

Correct:

```cpp
if(grumpy[low-1] == 1){
    extra -= customers[low-1];
}
```

---

### 3. Final me sirf `max_extra` return karna

Wrong:

```cpp
return max_extra;
```

Correct:

```cpp
return base + max_extra;
```

Because answer me already satisfied customers bhi include honge.

---

# 🔥 Quick Revision

```text
grumpy == 0
      ↓
base me add
      ↓

First minutes-size window
      ↓
grumpy == 1
      ↓
extra me add
      ↓
max_extra = extra
      ↓
window slide
      ↓
old grumpy == 1 → subtract
      ↓
new grumpy == 1 → add
      ↓
max_extra update
      ↓
return base + max_extra
```

---

# 🧠 One-Line Revision

```text
Pehle grumpy=0 wale already satisfied customers count karo, phir fixed sliding window se find karo ki technique kis minutes-size window me use karne par maximum grumpy=1 customers extra satisfied honge.
```

---

# ⭐ Interview Revision

```text
base = already satisfied

extra = current window ke unsatisfied customers
        jo technique se satisfied honge

max_extra = best window ka extra

answer = base + max_extra
```

Pattern:

```text
Fixed Sliding Window + Base Sum + Maximum Extra Sum
```
