# Longest Substring with K Unique Characters

## 📌 Problem

Hume ek string `s` aur integer `k` diya gaya hai.

Hume **longest substring** ki length find karni hai jisme:

```text
exactly k distinct / unique characters
```

hone chahiye.

Agar exactly `k` unique characters wala koi substring exist nahi karta, to:

```text
-1
```

return karna hai.

---

# 🔹 Example 1

```text
s = "aabacbebebe"
k = 3
```

Longest substring jisme exactly `3` unique characters hain:

```text
"cbebebe"
```

Isme unique characters:

```text
c
b
e
```

Total:

```text
3 unique characters
```

Length:

```text
7
```

So:

```text
Output = 7
```

---

# 🔹 Example 2

```text
s = "aaaa"
k = 2
```

String me sirf:

```text
'a'
```

unique character hai.

Hume exactly:

```text
2 unique characters
```

chahiye.

Aisa koi substring possible nahi hai.

So:

```text
Output = -1
```

---

# 🔹 Example 3

```text
s = "aabaaab"
k = 2
```

Complete string me unique characters:

```text
a
b
```

Exactly `2` unique characters hain.

Complete string:

```text
"aabaaab"
```

Length:

```text
7
```

So:

```text
Output = 7
```

---

# 🧠 Approach - Variable Size Sliding Window + Frequency Map

Is problem me window ka size fixed nahi hai.

Hume longest possible substring find karni hai jisme exactly `k` unique characters hon.

Isliye hum use karenge:

```text
Variable Size Sliding Window
```

Saath me hume current window ke characters ki frequency bhi maintain karni hai.

Isliye use karenge:

```cpp
unordered_map<char,int> mp;
```

---

# 🔥 Main Idea

Hum do pointers use karenge:

```text
low
high
```

### `high`

```text
Window ko EXPAND karega
```

### `low`

```text
Window ko SHRINK karega
```

Aur map:

```text
Current window ke har character ki frequency store karega.
```

---

# 🔹 Initial Variables

```cpp
int n=s.size();
int low=0;
int high=0;
int max_len=-1;

unordered_map<char,int> mp;
```

Meaning:

```text
n       → string ka size

low     → window ka starting index

high    → window ka ending index

max_len → longest valid substring ki length

mp      → current window ke characters ki frequency
```

---

# ❓ `max_len = -1` Kyu?

Problem bolta hai:

Agar exactly `k` unique characters wala koi substring nahi mila:

```text
return -1
```

Isliye starting me:

```cpp
int max_len=-1;
```

rakhte hain.

Agar valid substring milti hai to `max_len` update ho jayega.

Agar nahi milti:

```text
max_len = -1
```

hi rahega.

---

# 🔥 Frequency Map Kya Karega?

Suppose current window hai:

```text
"aaba"
```

Characters:

```text
a a b a
```

Map:

```text
a → 3
b → 1
```

Yahan:

```cpp
mp.size()
```

hoga:

```text
2
```

Kyunki map me total unique keys hain:

```text
a
b
```

So:

```text
mp.size() = number of unique characters
```

---

# ⚠️ Frequency vs Unique Characters

Ye difference important hai.

Suppose:

```text
window = "aaabb"
```

Map:

```text
a → 3
b → 2
```

Total characters:

```text
5
```

Lekin unique characters:

```text
2
```

So:

```text
mp.size() = 2
```

not `5`.

---

# 🔥 Step 1 - Window Expand

Outer loop:

```cpp
while(high<n)
```

Har iteration me `high` wala character current window me add karenge:

```cpp
mp[s[high]]++;
```

Example:

```text
s = "aaba"
```

Initially:

```text
high = 0
```

Character:

```text
s[0] = 'a'
```

Add:

```cpp
mp['a']++;
```

Map:

```text
a → 1
```

---

Next `a`:

```text
a → 2
```

Next `b`:

```text
a → 2
b → 1
```

So:

```text
mp.size() = 2
```

---

# 🔥 Three Important Cases

Current window me unique characters ki count ke according 3 situations ho sakti hain.

```text
1. mp.size() < k

2. mp.size() == k

3. mp.size() > k
```

In teen cases ko samajhna is problem ka main concept hai.

---

# 🟡 Case 1 - `mp.size() < k`

Suppose:

```text
k = 3
```

Current map:

```text
a → 2
b → 1
```

Then:

```text
mp.size() = 2
```

So:

```text
2 < 3
```

Abhi enough unique characters nahi hain.

Hume window ko aur bada karna hai.

So:

```cpp
high++;
```

Meaning:

```text
EXPAND WINDOW
```

---

# 🟢 Case 2 - `mp.size() == k`

Suppose:

```text
k = 3
```

Map:

```text
a → 2
b → 1
c → 1
```

Then:

```text
mp.size() = 3
```

Exactly `k` unique characters mil gaye.

Current window valid hai.

So:

```cpp
if(mp.size()==k)
```

ke andar length calculate karenge.

---

# 🔹 Window Length

Formula:

```cpp
int res=high-low+1;
```

Why `+1`?

Suppose:

```text
low = 2
high = 5
```

Indices:

```text
2 3 4 5
```

Total elements:

```text
4
```

Formula:

```text
high-low+1

= 5-2+1

= 4
```

So:

```cpp
int res=high-low+1;
```

---

# 🔹 Maximum Length Update

Current valid window ki length `res` me hai.

Previous maximum:

```text
max_len
```

So:

```cpp
max_len=max(res,max_len);
```

Example:

```text
max_len = 5
res = 7
```

Then:

```text
max(7,5) = 7
```

So:

```text
max_len = 7
```

---

# 🔴 Case 3 - `mp.size() > k`

Suppose:

```text
k = 3
```

Map:

```text
a → 2
b → 1
c → 1
e → 1
```

Unique characters:

```text
4
```

So:

```text
4 > 3
```

Current window invalid hai.

Ab aur expand karne se problem solve nahi hogi.

Hume window ko:

```text
SHRINK
```

karna padega.

Isliye:

```cpp
while(mp.size()>k)
```

use karenge.

---

# ❓ `while` Kyu, `if` Kyu Nahi?

Suppose unique characters:

```text
5
```

hain aur:

```text
k = 3
```

Sirf ek character remove karne se zaroori nahi ki unique count `3` ho jaye.

Hume repeatedly left side se remove karna pad sakta hai.

Isliye:

```cpp
while(mp.size()>k)
```

Meaning:

```text
Jab tak unique characters k se jyada hain,
window ko shrink karte raho.
```

---

# 🔥 Character Remove Karna

Window ke leftmost character ki frequency decrease karenge:

```cpp
mp[s[low]]--;
```

Suppose:

```text
a → 3
```

Aur left side se ek `a` remove kiya.

Then:

```text
a → 2
```

`a` abhi bhi current window me present hai.

So map me `a` rehna chahiye.

---

# 🔥 Frequency Zero Hone Par `erase`

Suppose map:

```text
a → 1
b → 2
c → 3
e → 1
```

Aur:

```text
s[low] = 'a'
```

Remove:

```cpp
mp[s[low]]--;
```

Now:

```text
a → 0
```

Matlab current window me ab ek bhi `a` nahi hai.

Isliye map se `a` ko completely remove karna padega:

```cpp
if(mp[s[low]]==0){
    mp.erase(s[low]);
}
```

---

# ❓ `erase()` Important Kyu Hai?

Suppose hum erase nahi karte.

Map:

```text
a → 0
b → 2
c → 1
e → 3
```

Technically `a` ki frequency `0` hai.

Matlab:

```text
'a' current window me exist nahi karta.
```

Lekin map me key `'a'` abhi bhi present hai.

So:

```cpp
mp.size()
```

still:

```text
4
```

dega ❌

Jabki actual unique characters:

```text
b
c
e
```

sirf:

```text
3
```

hain.

Isliye zero frequency par:

```cpp
mp.erase(s[low]);
```

karna mandatory hai.

---

# 🔹 `low++`

Character remove karne ke baad:

```cpp
low++;
```

kar denge.

Meaning:

```text
Window ka left boundary right side move karega.
```

So:

```text
low++ → SHRINK
```

---

# 🔥 Complete Core Logic

```cpp
mp[s[high]]++;

while(mp.size()>k){

    mp[s[low]]--;

    if(mp[s[low]]==0){
        mp.erase(s[low]);
    }

    low++;
}

if(mp.size()==k){

    int res=high-low+1;

    max_len=max(res,max_len);
}

high++;
```

---

# 🔄 Dry Run

Consider:

```text
s = "aabacbebebe"
k = 3
```

Initially:

```text
low = 0
high = 0

mp = {}

max_len = -1
```

---

## Step 1

Current:

```text
s[high] = 'a'
```

Add:

```text
a → 1
```

So:

```text
mp.size() = 1
```

Compare:

```text
1 < 3
```

Not enough unique characters.

So:

```text
high++
```

---

## Step 2

Next character:

```text
'a'
```

Map:

```text
a → 2
```

Still:

```text
mp.size() = 1
```

So expand.

---

## Step 3

Next:

```text
'b'
```

Map:

```text
a → 2
b → 1
```

Unique:

```text
2
```

Still:

```text
2 < 3
```

Expand.

---

## Step 4

Next:

```text
'a'
```

Map:

```text
a → 3
b → 1
```

Unique:

```text
2
```

Still expand.

---

## Step 5

Next character:

```text
'c'
```

Map:

```text
a → 3
b → 1
c → 1
```

Now:

```text
mp.size() = 3
```

Exactly:

```text
k = 3
```

So current window valid.

Current substring:

```text
"aabac"
```

Length:

```text
high-low+1
= 4-0+1
= 5
```

So:

```text
max_len = 5
```

---

## Continue Expanding

Next:

```text
'b'
```

Map:

```text
a → 3
b → 2
c → 1
```

Unique still:

```text
3
```

Window valid.

Length increases.

---

## Problem Kab Aayegi?

Eventually character:

```text
'e'
```

add hoga.

Map kuch aisa ho jayega:

```text
a
b
c
e
```

So:

```text
mp.size() = 4
```

But:

```text
k = 3
```

Now:

```text
4 > 3
```

Window invalid.

---

# 🔥 Shrinking Start

Code:

```cpp
while(mp.size()>k)
```

Left side se characters remove karenge.

Current `low` par `'a'` hai.

Frequency decrease:

```text
a → 2
```

Still `a` exists.

Unique still:

```text
4
```

So again shrink.

Next `a`:

```text
a → 1
```

Still unique:

```text
4
```

Again shrink.

Eventually last `a` remove hoga:

```text
a → 0
```

Then:

```cpp
mp.erase('a');
```

Now map:

```text
b
c
e
```

So:

```text
mp.size() = 3
```

Now:

```text
3 > 3 ❌
```

Shrinking stop.

---

# 🟢 Valid Window Again

Now exactly:

```text
3 unique characters
```

hain.

So:

```cpp
if(mp.size()==k)
```

true.

Current length calculate:

```cpp
int res=high-low+1;
```

Then:

```cpp
max_len=max(res,max_len);
```

---

# 🔥 Eventually Longest Window

Algorithm eventually substring find karega:

```text
"cbebebe"
```

Unique:

```text
c
b
e
```

Exactly:

```text
3
```

Length:

```text
7
```

So:

```text
max_len = 7
```

Final:

```text
return 7
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    int longestKSubstr(string &s, int k) {

        int n=s.size();
        int low=0;
        int high=0;
        int max_len=-1;

        unordered_map<char,int> mp;

        while(high<n){

            mp[s[high]]++;

            while(mp.size()>k){

                mp[s[low]]--;

                if(mp[s[low]]==0){
                    mp.erase(s[low]);
                }

                low++;
            }

            if(mp.size()==k){

                int res=high-low+1;

                max_len=max(res,max_len);
            }

            high++;
        }

        return max_len;
    }
};
```

---

# 🧠 Code Line-by-Line

## String Size

```cpp
int n=s.size();
```

String ki total length.

---

## Left Pointer

```cpp
int low=0;
```

Current window ka starting point.

```text
low++ → shrink
```

---

## Right Pointer

```cpp
int high=0;
```

Current window ka ending point.

```text
high++ → expand
```

---

## Maximum Length

```cpp
int max_len=-1;
```

Longest valid substring ki length store karega.

`-1` because agar valid substring nahi mili to answer `-1` hona chahiye.

---

## Frequency Map

```cpp
unordered_map<char,int> mp;
```

Current window ke har character ki frequency store karega.

Example:

```text
"aabbc"
```

Map:

```text
a → 2
b → 2
c → 1
```

And:

```text
mp.size() = 3
```

---

## Outer Loop

```cpp
while(high<n)
```

`high` pointer se complete string traverse karenge.

---

## Character Add

```cpp
mp[s[high]]++;
```

Current character ko window me add karta hai.

---

## Too Many Unique Characters

```cpp
while(mp.size()>k)
```

Agar unique characters allowed `k` se jyada ho gaye, window shrink karenge.

---

## Frequency Decrease

```cpp
mp[s[low]]--;
```

Leftmost character ko window se remove karta hai.

---

## Character Completely Remove

```cpp
if(mp[s[low]]==0){
    mp.erase(s[low]);
}
```

Agar character ki frequency `0` ho gayi, wo current window me exist nahi karta.

Isliye map se erase karte hain.

---

## Window Shrink

```cpp
low++;
```

Left boundary right side move hoti hai.

---

## Exactly K Unique

```cpp
if(mp.size()==k)
```

Current window valid hai.

---

## Length

```cpp
int res=high-low+1;
```

Current valid substring ki length.

---

## Maximum

```cpp
max_len=max(res,max_len);
```

Longest valid substring ki length update karta hai.

---

## Window Expand

```cpp
high++;
```

Next character ki taraf move karta hai.

---

# ⭐ Three Cases - Most Important

## Case 1

```text
mp.size() < k
```

Meaning:

```text
Unique characters kam hain.
```

Action:

```text
EXPAND
```

So eventually:

```cpp
high++;
```

---

## Case 2

```text
mp.size() == k
```

Meaning:

```text
Perfect valid window.
```

Action:

```cpp
int res=high-low+1;

max_len=max(res,max_len);
```

---

## Case 3

```text
mp.size() > k
```

Meaning:

```text
Too many unique characters.
```

Action:

```text
SHRINK
```

Using:

```cpp
while(mp.size()>k)
```

and:

```cpp
low++;
```

---

# 🔥 Visual Pattern

```text
                    START
                      ↓
              Add s[high] to map
                      ↓
                mp.size() ?
                      ↓

        ┌─────────────┼─────────────┐
        ↓             ↓             ↓
      < k            == k          > k
        ↓             ↓             ↓
     EXPAND       Valid Window    SHRINK
        ↓             ↓             ↓
     high++       Calculate len    low++
                      ↓             ↓
                  Update max    until <= k
                      ↓
                   high++
```

---

# 🆚 LC 209 vs This Problem

Both are **Variable Size Sliding Window**.

---

## LC 209 - Minimum Size Subarray Sum

We tracked:

```text
sum
```

Expand:

```cpp
sum += nums[high];
```

Shrink condition:

```cpp
while(sum >= target)
```

Answer:

```text
minimum length
```

---

## Longest Substring with K Unique

We track:

```text
frequency map
```

Expand:

```cpp
mp[s[high]]++;
```

Shrink condition:

```cpp
while(mp.size()>k)
```

Answer condition:

```cpp
if(mp.size()==k)
```

Answer:

```text
maximum length
```

---

# ⭐ General Variable Sliding Window Pattern

```cpp
int low=0;
int high=0;

while(high<n){

    // Add current element
    add(s[high]);

    // Window invalid ho gayi
    while(invalid_condition){

        // Remove left element
        remove(s[low]);

        low++;
    }

    // Valid window
    if(valid_condition){

        // Answer update
    }

    high++;
}
```

Is question me:

```text
add
→ mp[s[high]]++

invalid_condition
→ mp.size() > k

remove
→ mp[s[low]]--

valid_condition
→ mp.size() == k

answer
→ max_len
```

---

# ❓ `while(mp.size()>k)` Ke Baad `if(mp.size()==k)` Kyu?

Pehle ensure karte hain ki window me:

```text
k se jyada unique characters
```

na hon.

Agar hain:

```text
shrink
```

karte hain.

Shrinking ke baad possible hai:

```text
mp.size() == k
```

ho jaye.

Tab current window valid hai.

Isliye sequence:

```cpp
while(mp.size()>k){
    // shrink
}

if(mp.size()==k){
    // answer
}
```

---

# ❓ `max_len = 0` Kyu Nahi?

Technically end me separate check karke `0` ko `-1` convert kar sakte the.

Lekin problem directly bolti hai:

```text
No valid substring → -1
```

Isliye simple:

```cpp
int max_len=-1;
```

Agar valid substring mili:

```text
max_len update
```

Otherwise:

```text
-1
```

automatically return ho jayega.

---

# ⏱️ Time Complexity

Outer `high` pointer:

```text
0 → n-1
```

maximum `n` times move karta hai.

`low` pointer bhi:

```text
0 → n-1
```

maximum `n` times move karta hai.

Dono pointers sirf forward move karte hain.

So overall:

```text
O(n)
```

Average case me `unordered_map` operations:

```text
insert / update / erase → O(1)
```

So:

```text
Time Complexity = O(n)
```

---

# 💾 Space Complexity

Map me current window ke unique characters store hote hain.

Question me lowercase alphabets hain:

```text
a-z
```

Maximum:

```text
26
```

different characters.

So practical maximum space:

```text
O(26)
```

which is constant.

Generally frequency map ke terms me:

```text
O(k)
```

tak relevant entries ho sakti hain around a valid window.

---

# 📊 Complexity Summary

```text
Time Complexity  → O(n)

Space Complexity → O(k)
```

For lowercase English alphabet:

```text
Maximum 26 characters
```

---

# 🔥 Quick Revision

```text
high = 0
low = 0
map empty

        ↓

s[high] add karo

mp[s[high]]++

        ↓

unique > k ?

YES
 ↓
left character remove
 ↓
frequency--
 ↓
frequency == 0 ?
 ↓
erase
 ↓
low++
 ↓
repeat until unique <= k

        ↓

unique == k ?

YES
 ↓
len = high-low+1
 ↓
max_len update

        ↓

high++
```

---

# 🎯 Pattern To Remember

```text
HIGH → EXPAND

LOW → SHRINK

MAP → FREQUENCY

mp.size() → UNIQUE CHARACTERS
```

Main conditions:

```cpp
while(mp.size()>k){
    // shrink
}
```

and:

```cpp
if(mp.size()==k){
    // answer update
}
```

---

# 🧠 One-Line Revision

```text
High se characters add karo → unique > k ho to low se remove karo → exactly k unique mile to current length se maximum update karo.
```

---

# ⭐ Most Important Code

```cpp
mp[s[high]]++;

while(mp.size()>k){

    mp[s[low]]--;

    if(mp[s[low]]==0){
        mp.erase(s[low]);
    }

    low++;
}

if(mp.size()==k){

    int res=high-low+1;

    max_len=max(res,max_len);
}

high++;
```

Ye **Variable Sliding Window + Frequency Map** ka important pattern hai.
