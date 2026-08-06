# LeetCode 567 - Permutation in String

## 📌 Problem

Hume do strings di gayi hain:

```text
s1
s2
```

Hume check karna hai ki `s2` ke andar koi aisi **continuous substring** present hai kya jisme exactly wahi characters aur wahi frequencies hain jo `s1` me hain.

Agar milti hai:

```text
true
```

Otherwise:

```text
false
```

---

# 🔹 Example

```text
s1 = "ab"
s2 = "eidbaooo"
```

`s1` me:

```text
a → 1
b → 1
```

`s1` ki length:

```text
k = 2
```

Isliye `s2` me size `2` ki windows check karenge:

```text
[ei]dbaooo  ❌
e[id]baooo  ❌
ei[db]aooo  ❌
eid[ba]ooo  ✅
```

Window:

```text
"ba"
```

me:

```text
b → 1
a → 1
```

Ye exactly `s1 = "ab"` ki frequencies ke same hain.

Therefore:

```text
Output = true
```

---

# 🧠 Main Idea

Is problem me hume do cheezein compare karni hain:

```text
1. s1 me kya chahiye
2. Current window me kya hai
```

Isliye hum **2 frequency maps** use karenge:

```cpp
unordered_map<char,int> mp1;
unordered_map<char,int> mp2;
```

Easy way to remember:

```text
mp1 = REQUIREMENT
mp2 = CURRENT WINDOW
```

---

# 🔥 Map Kya Store Karta Hai?

Map har character ki frequency store karta hai.

Example:

```text
"aab"
```

Frequency:

```text
a → 2
b → 1
```

Code:

```cpp
mp['a']++;
mp['a']++;
mp['b']++;
```

Map becomes:

```text
a → 2
b → 1
```

---

# ❓ 2 Maps Kyu?

Suppose:

```text
s1 = "ab"
```

Requirement:

```text
mp1:

a → 1
b → 1
```

Current window:

```text
"ei"
```

Current map:

```text
mp2:

e → 1
i → 1
```

Ab hum simply compare kar sakte hain:

```cpp
if(mp1 == mp2)
```

Agar same:

```text
Requirement == Current Window
```

then answer:

```text
true
```

---

# 🔹 Duplicate Character Example

Suppose:

```text
s1 = "aab"
```

Then:

```text
mp1:

a → 2
b → 1
```

Current window:

```text
"aba"
```

Then:

```text
mp2:

a → 2
b → 1
```

Both same:

```text
mp1 == mp2
```

So valid ✅

But window:

```text
"abb"
```

has:

```text
a → 1
b → 2
```

Frequencies same nahi hain.

So invalid ❌

Isliye sirf characters present hona enough nahi hai.

**Character frequencies bhi exactly same honi chahiye.**

---

# 🔥 Why Fixed Size Sliding Window?

Suppose:

```text
s1 = "abc"
```

Then:

```text
k = s1.size()
  = 3
```

Agar `s2` me matching substring hogi, uski length bhi exactly `3` hogi.

So:

```text
Window size = k
```

hamesha fixed rahega.

Therefore ye:

```text
Fixed Size Sliding Window
```

problem hai.

---

# 🔥 Variables

```cpp
int k=s1.size();
int n=s2.size();
int low=0;
int high=k-1;
```

Meaning:

```text
k    → required window size

n    → s2 ki length

low  → current window ka left index

high → current window ka right index
```

---

# 🔥 Edge Case - `k > n`

```cpp
if(k>n){
    return false;
}
```

Suppose:

```text
s1 = "abcd"
s2 = "ab"
```

Then:

```text
s1 size = 4
s2 size = 2
```

4-size substring ko 2-size string me find karna impossible hai.

So:

```text
false
```

---

# 🔥 Step 1 - `s1` Ka Frequency Map

```cpp
for(int i=0;i<k;i++){
    mp1[s1[i]]++;
}
```

Example:

```text
s1 = "ab"
```

Initially:

```text
mp1 = empty
```

After `'a'`:

```text
a → 1
```

After `'b'`:

```text
a → 1
b → 1
```

So:

```text
mp1 = requirement
```

---

# 🔥 Step 2 - First Window Ka Map

```cpp
for(int i=0;i<k;i++){
    mp2[s2[i]]++;
}
```

Given:

```text
s2 = "eidbaooo"
k = 2
```

First window:

```text
[ei]dbaooo
```

So:

```text
mp2:

e → 1
i → 1
```

---

# 🔥 Step 3 - First Window Check

```cpp
if(mp1==mp2){
    return true;
}
```

Currently:

```text
mp1:

a → 1
b → 1


mp2:

e → 1
i → 1
```

Not equal ❌

So next window check karenge.

First window ko separately check karna important hai because first window `for` loop se bani hai.

---

# 🔥 Step 4 - Window Slide

```cpp
while(high<n-1){

    low++;
    high++;
```

Suppose:

```text
[ei]dbaooo
 ↑↑
```

After slide:

```text
e[id]baooo
  ↑↑
```

Changes:

```text
e → OUT
d → IN
```

Window ka size still `k = 2` hai.

---

# 🔥 Step 5 - Outgoing Character

Code:

```cpp
mp2[s2[low-1]]--;
```

Current window:

```text
[ei]
```

Next:

```text
[id]
```

Character:

```text
e
```

bahar gaya.

Before:

```text
mp2:

e → 1
i → 1
```

After:

```text
e → 0
i → 1
```

---

# 🔥 Frequency 0 Hone Par Erase

```cpp
if(mp2[s2[low-1]]==0){
    mp2.erase(s2[low-1]);
}
```

Agar:

```text
e → 0
```

hai, iska matlab current window me `e` present hi nahi hai.

So usko map se remove karenge.

After erase:

```text
mp2:

i → 1
```

---

# ⚠️ Important Mistake

Wrong:

```cpp
if(s2[low-1]==0){
    mp2.erase(s2[low-1]);
}
```

Ye character ko `0` se compare kar raha hai.

Hume **character nahi, uski frequency** check karni hai.

Correct:

```cpp
if(mp2[s2[low-1]]==0){
    mp2.erase(s2[low-1]);
}
```

Remember:

```text
s2[low-1]
        ↓
character

mp2[s2[low-1]]
        ↓
us character ki frequency
```

---

# 🔥 Step 6 - Incoming Character

```cpp
mp2[s2[high]]++;
```

New window:

```text
[id]
```

`d` enter hua.

Current map before adding:

```text
i → 1
```

After adding `d`:

```text
i → 1
d → 1
```

Ye exactly `"id"` ki frequency hai.

---

# 🔥 Step 7 - Compare Maps

```cpp
if(mp1==mp2){
    return true;
}
```

Meaning:

```text
REQUIREMENT == CURRENT WINDOW ?
```

If yes:

```text
true
```

Otherwise next window.

---

# 🔄 Dry Run

Given:

```text
s1 = "ab"
s2 = "eidbaooo"
```

Requirement:

```text
mp1:

a → 1
b → 1
```

---

## Window 1

```text
[ei]dbaooo
```

```text
mp2:

e → 1
i → 1
```

Compare:

```text
mp1 != mp2
```

❌

---

## Window 2

```text
e[id]baooo
```

Outgoing:

```text
e
```

Incoming:

```text
d
```

Map:

```text
i → 1
d → 1
```

Not same ❌

---

## Window 3

```text
ei[db]aooo
```

Map:

```text
d → 1
b → 1
```

Requirement:

```text
a → 1
b → 1
```

Not same ❌

---

## Window 4

```text
eid[ba]ooo
```

Map:

```text
b → 1
a → 1
```

Requirement:

```text
a → 1
b → 1
```

Same frequencies ✅

Therefore:

```cpp
return true;
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    bool checkInclusion(string s1, string s2) {

        int k=s1.size();
        int n=s2.size();

        if(k>n){
            return false;
        }

        int low=0;
        int high=k-1;

        unordered_map<char,int> mp1;
        unordered_map<char,int> mp2;

        // s1 frequency = requirement
        for(int i=0;i<k;i++){
            mp1[s1[i]]++;
        }

        // First window frequency
        for(int i=0;i<k;i++){
            mp2[s2[i]]++;
        }

        // First window check
        if(mp1==mp2){
            return true;
        }

        // Remaining windows
        while(high<n-1){

            low++;
            high++;

            // Outgoing character
            mp2[s2[low-1]]--;

            if(mp2[s2[low-1]]==0){
                mp2.erase(s2[low-1]);
            }

            // Incoming character
            mp2[s2[high]]++;

            // Compare requirement and current window
            if(mp1==mp2){
                return true;
            }
        }

        return false;
    }
};
```

---

# 🔥 Most Important Logic

Think:

```text
mp1 = REQUIREMENT
mp2 = CURRENT
```

Then:

```cpp
if(mp1==mp2){
    return true;
}
```

means:

```text
Jo characters + frequencies chahiye
            ==
Current window ke characters + frequencies
```

Match mil gaya → `true`.

---

# 🔥 Sliding Window Part

```cpp
low++;
high++;
```

Window right move.

Then:

```cpp
mp2[s2[low-1]]--;
```

Outgoing character remove.

Then:

```cpp
if(mp2[s2[low-1]]==0){
    mp2.erase(s2[low-1]);
}
```

Window me character nahi bacha → map se erase.

Then:

```cpp
mp2[s2[high]]++;
```

Incoming character add.

---

# 🧠 Visual Pattern

```text
        s1
         ↓
   REQUIREMENT MAP
   ┌───────────┐
   │ a → 1     │
   │ b → 1     │
   └───────────┘
         mp1


s2 → e i d [b a] o o o
              ↓
       CURRENT MAP
       ┌───────────┐
       │ b → 1     │
       │ a → 1     │
       └───────────┘
             mp2

              ↓

          mp1 == mp2

              ↓

            TRUE ✅
```

---

# ⏱️ Time Complexity

Hum `s2` par sliding window chala rahe hain.

Since strings lowercase English letters ki hain, frequency map ka character set bounded hai.

```text
Time Complexity = O(n)
```

---

# 💾 Space Complexity

Maximum lowercase English letters:

```text
26
```

So maps bounded space use karte hain:

```text
Space Complexity = O(1)
```

---

# ⚠️ Common Mistakes

### 1. `k > n` check bhoolna

```cpp
if(k>n){
    return false;
}
```

Otherwise first `k` characters access karte waqt problem ho sakti hai.

### 2. Character ko `0` se compare karna

Wrong:

```cpp
if(s2[low-1]==0)
```

Correct:

```cpp
if(mp2[s2[low-1]]==0)
```

### 3. Frequency 0 hone par erase na karna

```cpp
if(mp2[s2[low-1]]==0){
    mp2.erase(s2[low-1]);
}
```

Ye important hai because maps ko directly:

```cpp
mp1 == mp2
```

compare kar rahe hain.

### 4. First window check bhoolna

```cpp
if(mp1==mp2){
    return true;
}
```

`while` se pehle first window check honi chahiye.

---

# 🔥 Quick Revision

```text
s1 ka map banao
      ↓
mp1 = requirement
      ↓
s2 ke first k characters ka map
      ↓
mp2 = current window
      ↓
mp1 == mp2 ?
YES → true
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
YES → true
      ↓
repeat
      ↓
koi match nahi
      ↓
false
```

---

# 🧠 One-Line Revision

```text
s1 ki frequency ko requirement map me rakho aur s2 ki har k-size window ki frequency ko current map me maintain karo; dono maps equal hote hi true return karo.
```

---

# ⭐ Interview Revision

```text
mp1 → Required frequency
mp2 → Current window frequency

OLD → frequency--
0 → erase

NEW → frequency++

mp1 == mp2 → true
```

**Pattern:**

```text
Fixed Sliding Window + Two Frequency Maps + Map Comparison
```
