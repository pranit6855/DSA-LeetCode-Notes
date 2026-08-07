# LeetCode 2024 - Maximize the Confusion of an Exam

## 📌 Problem

Hume ek string `answerKey` di gayi hai jisme sirf:

```text
'T' and 'F'
```

characters hain.

Hum maximum `k` characters change kar sakte hain:

```text
T → F
or
F → T
```

Hume maximum consecutive same answers ki length find karni hai.

---

# 🔹 Example

```text
answerKey = "TTFF"
k = 2
```

Dono `F` ko `T` me change:

```text
TTFF
  ↓↓

TTTT
```

Answer:

```text
4
```

---

# 🧠 Main Idea

Current window me `T` aur `F` ki frequency maintain karenge.

```text
t_count → number of T
f_count → number of F
```

Jo character maximum baar present hai usko same rakhenge.

Baaki characters ko change karenge.

So:

```text
max_freq = max(t_count, f_count)
```

Changes required:

```text
window_size - max_freq
```

---

# 🔥 Example

Current window:

```text
T T F T
```

Counts:

```text
T = 3
F = 1
```

Window size:

```text
4
```

Maximum frequency:

```text
max_freq = 3
```

Required changes:

```text
4 - 3 = 1
```

Sirf `F` ko `T` banana padega.

Agar:

```text
k = 1
```

window valid hai ✅

---

# ✅ Valid / Invalid Condition

Valid:

```text
window_size - max_freq <= k
```

Invalid:

```text
window_size - max_freq > k
```

Invalid hone par window ko left se shrink karenge.

---

# 🔥 Variables

```cpp
int n = answerkey.size();
int low = 0;
int high = 0;
int t_count = 0;
int f_count = 0;
int max_len = 0;
```

Meaning:

```text
low      → left pointer
high     → right pointer
t_count  → current window me T count
f_count  → current window me F count
max_len  → longest valid window
```

---

# 🔥 Step 1 - Expand Window

`high` par jo character hai uska count increase karenge.

```cpp
if(answerkey[high]=='T'){
    t_count++;
}

if(answerkey[high]=='F'){
    f_count++;
}
```

---

# 🔥 Step 2 - Calculate Changes Required

Current window size:

```cpp
high-low+1
```

Maximum frequency:

```cpp
max(t_count,f_count)
```

So changes required:

```cpp
(high-low+1) - max(t_count,f_count)
```

---

# 🔥 Step 3 - Invalid Window → Shrink

Agar:

```cpp
(high-low+1)-max(t_count,f_count) > k
```

to required changes allowed `k` se zyada hain.

So:

```cpp
while((high-low+1)-max(t_count,f_count)>k)
```

window ko shrink karenge.

---

# 🔥 Step 4 - Remove Left Character

Agar outgoing character `T` hai:

```cpp
if(answerkey[low]=='T'){
    t_count--;
}
```

Agar `F` hai:

```cpp
if(answerkey[low]=='F'){
    f_count--;
}
```

Then:

```cpp
low++;
```

Valid hone tak shrink karte rahenge.

---

# 🔥 Important: Condition Directly While Me Kyu?

Wrong approach:

```cpp
int window_size = high-low+1;
int max_freq = max(t_count,f_count);

while(window_size-max_freq > k){
    ...
    low++;
}
```

Problem:

`low++` hone ke baad:

```text
window_size
```

change ho gaya.

Counts decrease hone ke baad:

```text
max_freq
```

bhi change ho sakta hai.

Lekin variables me old values stored hain.

Isliye condition fresh calculate karna better hai:

```cpp
while((high-low+1)-max(t_count,f_count)>k)
```

Ab har shrink ke baad:

```text
high-low+1
```

aur:

```text
max(t_count,f_count)
```

dobara calculate honge.

---

# 🔥 Step 5 - Maximum Length

Window valid hone ke baad:

```cpp
int len = high-low+1;
```

Then:

```cpp
max_len = max(max_len,len);
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    int maxConsecutiveAnswers(string answerkey, int k) {

        int n=answerkey.size();
        int low=0;
        int high=0;
        int t_count=0;
        int f_count=0;
        int max_len=0;

        while(high<n){

            // Incoming character
            if(answerkey[high]=='T'){
                t_count++;
            }

            if(answerkey[high]=='F'){
                f_count++;
            }

            // Invalid window -> shrink
            while((high-low+1)-max(t_count,f_count)>k){

                if(answerkey[low]=='T'){
                    t_count--;
                }

                if(answerkey[low]=='F'){
                    f_count--;
                }

                low++;
            }

            // Valid window
            int len=high-low+1;

            max_len=max(max_len,len);

            high++;
        }

        return max_len;
    }
};
```

---

# 🔄 Small Dry Run

Given:

```text
answerKey = "TTFTF"
k = 1
```

Start:

```text
low = 0
high = 0
```

### Window

```text
[T]
```

Counts:

```text
T = 1
F = 0
```

Changes:

```text
1 - 1 = 0
```

Valid ✅

---

### Window

```text
[TT]
```

Counts:

```text
T = 2
F = 0
```

Changes:

```text
2 - 2 = 0
```

Valid ✅

---

### Window

```text
[TTF]
```

Counts:

```text
T = 2
F = 1
```

Changes:

```text
3 - 2 = 1
```

Since:

```text
1 <= k
```

Valid ✅

We can change:

```text
TTF
  ↓
TTT
```

Length:

```text
3
```

---

### Window

```text
[TTFT]
```

Counts:

```text
T = 3
F = 1
```

Changes:

```text
4 - 3 = 1
```

Valid ✅

Length:

```text
4
```

---

### Next `F`

```text
[TTFTF]
```

Counts:

```text
T = 3
F = 2
```

Changes:

```text
5 - 3 = 2
```

But:

```text
k = 1
```

So invalid ❌

Now `low` se shrink karenge until required changes `<= k`.

---

# 🧠 Why `window_size - max_freq`?

Suppose:

```text
window = "TTTFF"
```

Counts:

```text
T = 3
F = 2
```

Majority:

```text
T = 3
```

`T` ko change karne ki zarurat nahi.

Sirf:

```text
2 F
```

change karne hain.

Mathematically:

```text
window_size - max_freq

= 5 - 3

= 2
```

So `2` replacements required.

---

# 🔥 LC 1004 Se Connection

## LC 1004

Window me maximum `k` zeros allowed:

```text
zero_count > k
      ↓
shrink
```

## LC 2024

Window ko same banane ke liye maximum `k` replacements allowed:

```text
window_size - max_freq > k
              ↓
            shrink
```

Dono me same pattern:

```text
EXPAND
   ↓
INVALID?
   ↓
SHRINK
   ↓
VALID
   ↓
MAX LENGTH
```

---

# 🔥 LC 424 Se Connection

Ye problem **Longest Repeating Character Replacement (LC 424)** ke almost same pattern par based hai.

LC 424 me multiple uppercase characters ho sakte hain.

LC 2024 me sirf:

```text
T
F
```

hain.

Isliye yahan sirf:

```text
t_count
f_count
```

maintain karna enough hai.

---

# ⏱️ Time Complexity

`high` poori string par ek baar move karta hai.

`low` bhi sirf forward move karta hai.

```text
Time Complexity = O(n)
```

---

# 💾 Space Complexity

Sirf variables maintain kar rahe hain:

```text
t_count
f_count
low
high
max_len
```

So:

```text
Space Complexity = O(1)
```

---

# ⚠️ Common Mistakes

## 1. `max_freq` ki jagah minimum frequency use karna

Hum majority character ko same rakhte hain.

So:

```cpp
max(t_count,f_count)
```

use hoga.

---

## 2. Invalid condition wrong karna

Correct:

```cpp
while((high-low+1)-max(t_count,f_count)>k)
```

Because:

```text
required replacements > allowed replacements
```

tabhi window invalid hai.

---

## 3. Shrink ke time count decrease na karna

Outgoing character ko frequency se remove karna compulsory hai:

```cpp
if(answerkey[low]=='T'){
    t_count--;
}

if(answerkey[low]=='F'){
    f_count--;
}
```

Then:

```cpp
low++;
```

---

## 4. Old `window_size` use karna

Avoid:

```cpp
int window_size=high-low+1;
```

agar usi stored value ko shrinking ke dauran use kar rahe ho.

Better:

```cpp
while((high-low+1)-max(t_count,f_count)>k)
```

taaki fresh window size/count use ho.

---

# 🔥 Quick Revision

```text
high se T/F add
       ↓
T_count / F_count++
       ↓
max_freq = max(T,F)
       ↓
required changes =
window_size - max_freq
       ↓
changes > k ?
       ↓ YES
low se shrink
       ↓
outgoing T/F count--
       ↓
low++
       ↓
valid window
       ↓
max_len update
```

---

# ⭐ One-Line Revision

> Current window me majority character ko same rakho aur baaki characters ko replace karo; agar required replacements `k` se zyada ho jayein to left se shrink karo.

## Pattern

```text
Variable Sliding Window
+
Character Frequency
+
Window Size - Maximum Frequency
+
Shrink Until Valid
+
Maximum Length
```
