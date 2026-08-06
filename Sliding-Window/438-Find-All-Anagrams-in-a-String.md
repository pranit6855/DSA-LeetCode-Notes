# LeetCode 438 - Find All Anagrams in a String

## 📌 Problem

Hume do strings di gayi hain:

```text
s
p
```

Hume `s` ke andar un sabhi substrings ke **starting indices** return karne hain jinke characters aur unki frequencies `p` ke same hain.

---

# 🔹 Example

```text
s = "cbaebabacd"
p = "abc"
```

`p` ki length:

```text
k = 3
```

Isliye `s` me size `3` ki fixed window chalayenge.

Possible windows:

```text
[cba]ebabacd   ✅ index = 0

c[bae]babacd   ❌

cb[aeb]abacd   ❌

cba[eba]bacd   ❌

cbae[bab]acd   ❌

cbaeb[aba]cd   ❌

cbaeba[bac]d   ✅ index = 6

cbaebab[acd]   ❌
```

Answer:

```text
[0,6]
```

---

# 🧠 Main Idea

Hum do frequency maps maintain karenge:

```cpp
unordered_map<char,int> mp1;
unordered_map<char,int> mp2;
```

Hamare code me:

```text
mp1 → s ki CURRENT WINDOW ki frequency

mp2 → p ki REQUIRED frequency
```

Example:

```text
p = "abc"
```

Then:

```text
mp2:

a → 1
b → 1
c → 1
```

Agar current window:

```text
"cba"
```

Then:

```text
mp1:

c → 1
b → 1
a → 1
```

Dono maps same hain:

```cpp
mp1 == mp2
```

Therefore current window valid hai.

Uska starting index:

```cpp
ans.push_back(low);
```

---

# 🔥 Why Fixed Size Sliding Window?

`p` ki length:

```cpp
int k=p.size();
```

Agar `p` ki length `3` hai, to matching substring ki length bhi exactly `3` hogi.

Therefore:

```text
Window size = k
```

hamesha fixed rahega.

So ye:

```text
Fixed Size Sliding Window
```

problem hai.

---

# 🔥 Initial Variables

```cpp
int k=p.size();
int n=s.size();
int low=0;
int high=k-1;

vector<int> ans;
```

Meaning:

```text
k    → window size

n    → s ki length

low  → current window ka starting index

high → current window ka ending index

ans  → matching windows ke starting indices
```

---

# 🔥 Edge Case

```cpp
if(k>n){
    return ans;
}
```

Suppose:

```text
s = "ab"
p = "abcd"
```

Then:

```text
p.size() = 4
s.size() = 2
```

2-size string ke andar 4-size window possible nahi hai.

So empty vector return:

```text
[]
```

---

# 🔥 Step 1 - First Window Ka Map

```cpp
for(int i=0;i<k;i++){
    mp1[s[i]]++;
}
```

Example:

```text
s = "cbaebabacd"
k = 3
```

First window:

```text
[cba]ebabacd
```

Map:

```text
mp1:

c → 1
b → 1
a → 1
```

Ye current window ki frequency hai.

---

# 🔥 Step 2 - `p` Ka Map

```cpp
for(int i=0;i<k;i++){
    mp2[p[i]]++;
}
```

Given:

```text
p = "abc"
```

Map:

```text
mp2:

a → 1
b → 1
c → 1
```

Ye hamari **requirement** hai.

Easy way:

```text
mp1 = CURRENT
mp2 = REQUIRED
```

---

# 🔥 Step 3 - First Window Check

```cpp
if(mp1==mp2){
    ans.push_back(low);
}
```

First window:

```text
"cba"
```

Current:

```text
mp1:

c → 1
b → 1
a → 1
```

Required:

```text
mp2:

a → 1
b → 1
c → 1
```

Frequencies same hain.

So:

```text
mp1 == mp2 ✅
```

Current starting index:

```text
low = 0
```

Therefore:

```cpp
ans.push_back(low);
```

Now:

```text
ans = [0]
```

---

# ❓ `low` Ko Store Kyu Karte Hain?

Question hume matching substring nahi, uska:

```text
starting index
```

return karne bol raha hai.

Current window:

```text
low ........ high
```

So window ka starting index:

```text
low
```

hai.

Isliye:

```cpp
ans.push_back(low);
```

---

# 🔥 Step 4 - Window Slide

Remaining windows:

```cpp
while(high<n-1)
```

Window ko right move:

```cpp
low++;
high++;
```

Example:

Before:

```text
[c b a] e b a b a c d
 ↑   ↑
low high
```

After:

```text
c [b a e] b a b a c d
   ↑   ↑
  low high
```

Window ka size still:

```text
k = 3
```

hai.

---

# 🔥 Step 5 - Outgoing Character Remove

Window slide karne par old left character bahar gaya.

Code:

```cpp
mp1[s[low-1]]--;
```

Example:

```text
[cba] → [bae]
```

Outgoing character:

```text
c
```

Before:

```text
c → 1
b → 1
a → 1
```

After frequency decrease:

```text
c → 0
b → 1
a → 1
```

---

# 🔥 Frequency 0 Hone Par Erase

```cpp
if(mp1[s[low-1]]==0){
    mp1.erase(s[low-1]);
}
```

Agar:

```text
c → 0
```

ho gaya, iska matlab current window me `c` ab present nahi hai.

Isliye map se `c` ko erase kar denge.

Map:

```text
b → 1
a → 1
```

---

# ⚠️ Important: `erase()` Me Key Dete Hain

Wrong:

```cpp
mp1.erase(mp1[s[low-1]]);
```

Correct:

```cpp
mp1.erase(s[low-1]);
```

Why?

Suppose outgoing character:

```text
c
```

and after decrement:

```text
mp1['c'] = 0
```

Then:

```cpp
mp1[s[low-1]]
```

gives:

```text
0
```

Agar likhoge:

```cpp
mp1.erase(mp1[s[low-1]]);
```

to tum basically:

```cpp
mp1.erase(0);
```

kar rahe ho ❌

Hume character/key:

```text
c
```

erase karna hai.

Therefore:

```cpp
mp1.erase(s[low-1]);
```

---

# 🔥 Step 6 - Incoming Character Add

New `high` par jo character hai wo window me enter hua.

```cpp
mp1[s[high]]++;
```

Example:

```text
[cba] → [bae]
```

Incoming:

```text
e
```

Map before:

```text
b → 1
a → 1
```

After:

```text
b → 1
a → 1
e → 1
```

Ye new window:

```text
"bae"
```

ki frequency hai.

---

# 🔥 Step 7 - Current Window Check

```cpp
if(mp1==mp2){
    ans.push_back(low);
}
```

Har new window ke baad current frequency ko required frequency se compare karenge.

Agar same:

```text
mp1 == mp2
```

then current window valid hai.

So:

```cpp
ans.push_back(low);
```

---

# 🔄 Dry Run

Given:

```text
s = "cbaebabacd"
p = "abc"
k = 3
```

Required:

```text
mp2:

a → 1
b → 1
c → 1
```

---

## Window 1

```text
[cba]ebabacd
```

Current:

```text
c → 1
b → 1
a → 1
```

Same ✅

```text
low = 0
```

So:

```text
ans = [0]
```

---

## Window 2

```text
c[bae]babacd
```

Current:

```text
b → 1
a → 1
e → 1
```

Not same ❌

---

## Window 3

```text
cb[aeb]abacd
```

Current:

```text
a → 1
e → 1
b → 1
```

Not same ❌

---

## Window 4

```text
cba[eba]bacd
```

Not same ❌

---

## Window 5

```text
cbae[bab]acd
```

Current:

```text
b → 2
a → 1
```

Required:

```text
a → 1
b → 1
c → 1
```

Not same ❌

---

## Window 6

```text
cbaeb[aba]cd
```

Current:

```text
a → 2
b → 1
```

Not same ❌

---

## Window 7

```text
cbaeba[bac]d
```

Current:

```text
b → 1
a → 1
c → 1
```

Same as required ✅

Current:

```text
low = 6
```

So:

```text
ans = [0,6]
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {

        int k=p.size();
        int n=s.size();
        int low=0;
        int high=k-1;

        vector<int> ans;

        if(k>n){
            return ans;
        }

        unordered_map<char,int> mp1;
        unordered_map<char,int> mp2;

        // First window frequency
        for(int i=0;i<k;i++){
            mp1[s[i]]++;
        }

        // Required frequency
        for(int i=0;i<k;i++){
            mp2[p[i]]++;
        }

        // First window check
        if(mp1==mp2){
            ans.push_back(low);
        }

        // Remaining windows
        while(high<n-1){

            low++;
            high++;

            // Outgoing
            mp1[s[low-1]]--;

            if(mp1[s[low-1]]==0){
                mp1.erase(s[low-1]);
            }

            // Incoming
            mp1[s[high]]++;

            // Check
            if(mp1==mp2){
                ans.push_back(low);
            }
        }

        return ans;
    }
};
```

---

# 🔥 LC 567 Se Connection

Ye LC 567 ke almost same hai.

### LC 567

Match milte hi:

```cpp
if(mp1==mp2){
    return true;
}
```

Kyunki bas poocha tha:

```text
Koi matching window exist karti hai?
```

---

### LC 438

Match milne par:

```cpp
if(mp1==mp2){
    ans.push_back(low);
}
```

Kyunki hume:

```text
ALL matching starting indices
```

chahiye.

Isliye first match par rukte nahi hain.

---

# 🧠 LC 567 vs LC 438

```text
LC 567
------
Matching window mili?
        ↓
return true


LC 438
------
Matching window mili?
        ↓
ans.push_back(low)
        ↓
continue searching
```

Sliding-window logic **same** hai.

---

# ⏱️ Time Complexity

Hum `s` par fixed sliding window chala rahe hain.

Lowercase English characters limited hain.

```text
Time Complexity = O(n)
```

---

# 💾 Space Complexity

Frequency maps maximum lowercase English characters store karte hain:

```text
26
```

So:

```text
Space Complexity = O(1)
```

---

# ⚠️ Common Mistakes

### 1. Loop condition

Wrong:

```cpp
while(high<n)
```

Kyunki loop ke andar `high++` karne ke baad `high == n` ho sakta hai.

Correct:

```cpp
while(high<n-1)
```

---

### 2. Wrong erase

Wrong:

```cpp
mp1.erase(mp1[s[low-1]]);
```

Correct:

```cpp
mp1.erase(s[low-1]);
```

`erase()` ko **key/character** chahiye.

---

### 3. Frequency check

Correct:

```cpp
if(mp1[s[low-1]]==0)
```

Meaning:

```text
Outgoing character ki frequency 0 ho gayi?
```

Then erase.

---

### 4. First window ko bhoolna

First window `while` se pehle bani hai.

Isliye:

```cpp
if(mp1==mp2){
    ans.push_back(low);
}
```

`while` ke pehle bhi hona chahiye.

---

# 🔥 Quick Revision

```text
p ki size = k
       ↓
s ki first k-size window
       ↓
mp1 = current window frequency
mp2 = required frequency
       ↓
mp1 == mp2 ?
YES → low store
       ↓
window slide
       ↓
outgoing frequency--
       ↓
frequency 0 → erase
       ↓
incoming frequency++
       ↓
mp1 == mp2 ?
YES → low store
       ↓
continue till end
       ↓
return ans
```

---

# 🧠 One-Line Revision

```text
p ki frequency aur s ki har k-size window ki frequency compare karo; jab dono same ho, current window ka starting index low answer me store karo.
```

---

# ⭐ Interview Revision

```cpp
// Remove old
mp1[s[low-1]]--;

if(mp1[s[low-1]]==0){
    mp1.erase(s[low-1]);
}

// Add new
mp1[s[high]]++;

// Match
if(mp1==mp2){
    ans.push_back(low);
}
```

Pattern:

```text
Fixed Sliding Window
+
Two Frequency Maps
+
Store Matching Starting Indices
```
