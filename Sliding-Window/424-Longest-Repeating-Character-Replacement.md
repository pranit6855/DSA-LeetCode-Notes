# LeetCode 424 - Longest Repeating Character Replacement

## 📌 Problem

Hume ek string `s` aur integer `k` diya gaya hai.

Hum maximum:

```text
k characters
```

ko kisi bhi uppercase English character se replace kar sakte hain.

Hume longest substring ki length find karni hai jisme maximum `k` replacements karne ke baad:

```text
saare characters same
```

ho jayein.

---

# 🔹 Example 1

```text
s = "ABAB"
k = 2
```

Hum dono `B` ko `A` se replace kar sakte hain:

```text
ABAB
 ↓ ↓

AAAA
```

Replacements:

```text
2
```

Allowed:

```text
k = 2
```

So complete string valid hai.

Answer:

```text
4
```

---

# 🔹 Example 2

```text
s = "AABABBA"
k = 1
```

Ek valid substring:

```text
"AABA"
```

Frequency:

```text
A → 3
B → 1
```

Agar `B` ko `A` bana dein:

```text
AAAA
```

Sirf:

```text
1 replacement
```

required hai.

Since:

```text
k = 1
```

window valid hai.

Longest possible length:

```text
4
```

So:

```text
Output = 4
```

---

# 🧠 Approach

Ye problem bhi:

```text
Variable Size Sliding Window
+
Frequency Map
```

se solve hogi.

Hum use karenge:

```text
low
high
unordered_map
max_freq
max_len
```

---

# 🔥 Main Concept

Suppose current window:

```text
"AABA"
```

Frequency:

```text
A → 3
B → 1
```

Window size:

```text
4
```

Sabse jyada frequency:

```text
max_freq = 3
```

Agar hume saare characters same banane hain, best choice hai jo character already sabse jyada present hai usko keep karo.

Yahan:

```text
A
```

already `3` baar hai.

Sirf `B` ko replace karna padega.

So:

```text
Required Replacements
=
Window Size - Maximum Frequency
```

Yahan:

```text
4 - 3 = 1
```

Sirf `1` replacement required.

---

# ⭐ Most Important Formula

```text
Required Replacements
=
Window Size - Max Frequency
```

Code:

```cpp
(high-low+1) - max_freq
```

Agar:

```text
Required Replacements <= k
```

window valid hai.

Agar:

```text
Required Replacements > k
```

window invalid hai.

---

# 🔥 Example of Formula

Window:

```text
A A B A C
```

Frequencies:

```text
A → 3
B → 1
C → 1
```

Window size:

```text
5
```

Maximum frequency:

```text
3
```

Required replacements:

```text
5 - 3 = 2
```

Agar:

```text
k = 2
```

Valid ✅

Agar:

```text
k = 1
```

Invalid ❌

because:

```text
2 > 1
```

---

# 🔹 Initial Variables

```cpp
int n=s.size();
int low=0;
int high=0;
int max_freq=0;
int max_len=0;

unordered_map<char,int> mp;
```

Meaning:

```text
n        → string ki length

low      → window ka starting point

high     → window ka ending point

max_freq → kisi ek character ki maximum frequency

max_len  → longest valid window ki length

mp       → character frequencies
```

---

# 🔥 Step 1 - Character Add Karo

Current `high` character ko map me add:

```cpp
mp[s[high]]++;
```

Suppose:

```text
s = "AABA"
```

Characters add hote jayenge:

```text
A
```

Map:

```text
A → 1
```

Next:

```text
A → 2
```

Next `B`:

```text
A → 2
B → 1
```

Next `A`:

```text
A → 3
B → 1
```

---

# 🔥 Step 2 - Maximum Frequency Track Karo

Har new character add karne ke baad:

```cpp
max_freq=max(max_freq,mp[s[high]]);
```

Example:

```text
A → 1
```

Then:

```text
max_freq = 1
```

Next `A`:

```text
A → 2
```

Then:

```text
max_freq = 2
```

Next `B`:

```text
B → 1
```

So:

```text
max_freq = max(2,1)
         = 2
```

Next `A`:

```text
A → 3
```

So:

```text
max_freq = 3
```

---

# ❓ `max_freq` Kya Batata Hai?

Current process me hum highest frequency seen for the maintained window-growth logic track kar rahe hain.

Example:

```text
A → 3
B → 1
```

Then:

```text
max_freq = 3
```

Iska use ye decide karne ke liye hota hai ki current window ko same characters me convert karne ke liye kitne replacements required honge.

---

# 🔥 Step 3 - Window Valid Hai Ya Nahi?

Current window size:

```cpp
high-low+1
```

Required replacements:

```cpp
(high-low+1)-max_freq
```

Check:

```cpp
while((high-low+1)-max_freq > k)
```

Agar required replacements `k` se jyada hain:

```text
window invalid
```

So window shrink karenge.

---

# 🔥 Valid Condition

```text
(window size - max_freq) <= k
```

Example:

```text
window size = 4
max_freq = 3
k = 1
```

Then:

```text
4 - 3 = 1
```

And:

```text
1 <= 1
```

Valid ✅

---

# 🔴 Invalid Condition

Example:

```text
window size = 5
max_freq = 3
k = 1
```

Required:

```text
5 - 3 = 2
```

But:

```text
k = 1
```

So:

```text
2 > 1
```

Invalid ❌

Window shrink karni padegi.

---

# 🔥 Step 4 - Window Shrink

Invalid hone par:

```cpp
mp[s[low]]--;
```

Leftmost character ki frequency decrease karenge.

Then:

```cpp
if(mp[s[low]]==0){
    mp.erase(s[low]);
}
```

Frequency zero hui to map se character erase.

Then:

```cpp
low++;
```

Window shrink.

---

# ⚠️ `window_size` Variable Bahar Store Kyu Nahi Karna?

Galat pattern:

```cpp
int window_size=high-low+1;

while(window_size-max_freq > k){

    low++;
}
```

Problem ye hai ki `low++` ke baad actual window size change ho gayi.

Lekin:

```text
window_size
```

variable ki old value same rahegi.

Example:

```text
high = 5
low = 1
```

Initially:

```text
window_size = 5
```

Shrink:

```cpp
low++;
```

Now:

```text
low = 2
```

Actual window size:

```text
5 - 2 + 1 = 4
```

Lekin stored:

```text
window_size = 5 ❌
```

Isliye condition me directly:

```cpp
(high-low+1)
```

use karte hain.

Correct:

```cpp
while((high-low+1)-max_freq > k)
```

Har `low++` ke baad fresh window size calculate hogi.

---

# 🔥 Step 5 - Current Length

Invalid window ko shrink karne ke baad:

```cpp
int res=high-low+1;
```

Current maintained window ki length milti hai.

---

# 🔥 Step 6 - Maximum Length

```cpp
max_len=max(res,max_len);
```

Longest length update karenge.

---

# 🔥 Step 7 - Expand

Finally:

```cpp
high++;
```

Next character par move.

---

# 💻 C++ Code

```cpp
class Solution {
public:
    int characterReplacement(string s, int k) {

        int n=s.size();
        int low=0;
        int high=0;
        int max_freq=0;
        int max_len=0;

        unordered_map<char,int> mp;

        while(high<n){

            mp[s[high]]++;

            max_freq=max(max_freq,mp[s[high]]);

            while((high-low+1)-max_freq > k){

                mp[s[low]]--;

                if(mp[s[low]]==0){
                    mp.erase(s[low]);
                }

                low++;
            }

            int res=high-low+1;

            max_len=max(res,max_len);

            high++;
        }

        return max_len;
    }
};
```

---

# 🧠 Code Line-by-Line

## String Length

```cpp
int n=s.size();
```

String ka total size.

---

## Left Pointer

```cpp
int low=0;
```

Window ka left boundary.

```text
low++ → SHRINK
```

---

## Right Pointer

```cpp
int high=0;
```

Window ka right boundary.

```text
high++ → EXPAND
```

---

## Maximum Frequency

```cpp
int max_freq=0;
```

Maximum frequency track karta hai jo replacement formula me use hoti hai.

---

## Maximum Length

```cpp
int max_len=0;
```

Longest answer store karta hai.

---

## Frequency Map

```cpp
unordered_map<char,int> mp;
```

Characters ki frequency store karta hai.

---

## Add Character

```cpp
mp[s[high]]++;
```

Current character ko window me add karta hai.

---

## Update Maximum Frequency

```cpp
max_freq=max(max_freq,mp[s[high]]);
```

Current character ki frequency previous maximum se compare hoti hai.

---

## Invalid Condition

```cpp
while((high-low+1)-max_freq > k)
```

Agar required replacements allowed `k` se jyada ho gaye:

```text
SHRINK
```

---

## Remove Left Character

```cpp
mp[s[low]]--;
```

Leftmost character ki frequency decrease.

---

## Erase

```cpp
if(mp[s[low]]==0){
    mp.erase(s[low]);
}
```

Frequency `0` hui to map se remove.

---

## Shrink

```cpp
low++;
```

Window ka left boundary right move karta hai.

---

## Current Length

```cpp
int res=high-low+1;
```

Current maintained window ki length.

---

## Maximum Length

```cpp
max_len=max(res,max_len);
```

Longest answer update.

---

## Expand

```cpp
high++;
```

Next character par move.

---

# 🔄 Dry Run

Consider:

```text
s = "AABABBA"
k = 1
```

Initially:

```text
low = 0
high = 0
max_freq = 0
max_len = 0
```

---

## Step 1 - `A`

Add:

```text
A → 1
```

Maximum frequency:

```text
1
```

Window:

```text
"A"
```

Window size:

```text
1
```

Required replacements:

```text
1 - 1 = 0
```

Valid.

So:

```text
max_len = 1
```

---

## Step 2 - `A`

Map:

```text
A → 2
```

Maximum frequency:

```text
2
```

Window:

```text
"AA"
```

Required:

```text
2 - 2 = 0
```

Valid.

```text
max_len = 2
```

---

## Step 3 - `B`

Map:

```text
A → 2
B → 1
```

Maximum frequency:

```text
2
```

Window:

```text
"AAB"
```

Size:

```text
3
```

Required replacements:

```text
3 - 2 = 1
```

Allowed:

```text
k = 1
```

Valid.

```text
max_len = 3
```

---

## Step 4 - `A`

Map:

```text
A → 3
B → 1
```

Maximum frequency:

```text
3
```

Window:

```text
"AABA"
```

Size:

```text
4
```

Required:

```text
4 - 3 = 1
```

Valid.

```text
max_len = 4
```

---

## Step 5 - `B`

Map:

```text
A → 3
B → 2
```

Window:

```text
"AABAB"
```

Size:

```text
5
```

Required according to formula:

```text
5 - 3 = 2
```

But:

```text
k = 1
```

So:

```text
2 > 1
```

Invalid.

Shrink.

Remove left `A`:

```text
A → 2
B → 2
```

Then:

```text
low++
```

Maintained window length becomes:

```text
4
```

---

# ⭐ Why `max_freq` Ko Shrink Karte Waqt Decrease Nahi Karte?

Code me:

```cpp
max_freq=max(max_freq,mp[s[high]]);
```

hai.

Lekin shrink karte waqt hum:

```cpp
max_freq--;
```

nahi karte.

Ye optimized solution ka important point hai.

`max_freq` ko har shrink par perfectly current banana necessary nahi hai.

Hume ultimately:

```text
longest valid length
```

find karni hai.

`max_freq` ek historical maximum ki tarah behave kar sakta hai.

Agar actual current maximum frequency shrink ke baad kam bhi ho gayi, stale `max_freq` hume koi **naya galat larger answer** create karne nahi deta; ye bas window ko immediately aur shrink na karne de sakta hai.

Jab future me kisi character ki frequency genuinely higher hoti hai:

```cpp
max_freq=max(max_freq,mp[s[high]]);
```

automatically update ho jayega.

Is optimization ki wajah se hume har shrink ke baad poora map scan karke maximum frequency dobara calculate nahi karna padta.

---

# ⭐ Main Formula Again

Is question me sabse important cheez:

```text
Window Size
-
Most Frequent Character Count
=
Characters That Need Replacement
```

Example:

```text
Window = "AABAC"
```

Frequency:

```text
A → 3
B → 1
C → 1
```

So:

```text
Window size = 5
Max frequency = 3
```

Required:

```text
5 - 3 = 2
```

Agar:

```text
k >= 2
```

valid.

Agar:

```text
k < 2
```

invalid.

---

# 🆚 Previous Sliding Window Problems

| Problem | Invalid Condition |
|---|---|
| Longest K Unique | `mp.size() > k` |
| Fruit Into Baskets | `mp.size() > 2` |
| Longest Without Repeating | `mp[s[high]] > 1` |
| Character Replacement | `(window_size - max_freq) > k` |

Baaki basic pattern same hai:

```text
EXPAND
   ↓
invalid?
   ↓
SHRINK
   ↓
answer update
```

---

# 🔥 General Flow

```text
s[high] add karo
        ↓
frequency update
        ↓
max_freq update
        ↓
required = window_size - max_freq
        ↓
required > k ?
        ↓ YES
left character remove
        ↓
low++
        ↓
condition dobara check
        ↓
maintained window length
        ↓
max_len update
        ↓
high++
```

---

# ⏱️ Time Complexity

```text
O(n)
```

`high` maximum `n` baar move karta hai.

`low` bhi maximum `n` baar move karta hai.

Dono sirf forward move karte hain.

So total roughly:

```text
n + n = 2n
```

Big-O:

```text
O(2n) = O(n)
```

`unordered_map` operations average case me:

```text
O(1)
```

hote hain.

So overall average:

```text
O(n)
```

---

# 💾 Space Complexity

String uppercase English letters ki hai:

```text
A-Z
```

Maximum:

```text
26
```

different characters map me aa sakte hain.

So:

```text
O(26)
```

which is constant:

```text
O(1)
```

---

# 📊 Complexity Summary

```text
Time Complexity  → O(n)

Space Complexity → O(1)
```

---

# 🔥 Quick Revision

```text
high se character add
        ↓
frequency++
        ↓
max_freq update
        ↓
window_size - max_freq
        ↓
ye batata hai kitne replacements chahiye
        ↓
> k ?
        ↓
SHRINK using low
        ↓
window length
        ↓
max_len update
```

---

# 🧠 One-Line Revision

```text
Window me jo character sabse jyada baar hai usko keep karo, baaki characters ko replace karna padega; agar required replacements k se jyada ho jayein to window shrink karo.
```

---

# ⭐ Most Important Lines

```cpp
mp[s[high]]++;

max_freq=max(max_freq,mp[s[high]]);

while((high-low+1)-max_freq > k){

    mp[s[low]]--;

    low++;
}

int res=high-low+1;

max_len=max(res,max_len);
```

Aur sabse important formula:

```text
Required Replacements
=
Window Size - Max Frequency
```
