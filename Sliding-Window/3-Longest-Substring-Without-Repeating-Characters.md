# LeetCode 3 - Longest Substring Without Repeating Characters

## 📌 Problem

Hume ek string `s` di gayi hai.

Hume longest **continuous substring** ki length find karni hai jisme:

```text
koi bhi character repeat na ho
```

Matlab har character ki frequency maximum:

```text
1
```

honi chahiye.

---

# 🔹 Example 1

```text
s = "abcabcbb"
```

Starting substring:

```text
"a"     → valid
"ab"    → valid
"abc"   → valid
"abca"  → invalid ❌
```

`"abca"` me:

```text
a
```

2 baar aa gaya.

Longest substring without repeating characters:

```text
"abc"
```

Length:

```text
3
```

So:

```text
Output = 3
```

---

# 🔹 Example 2

```text
s = "bbbbb"
```

Ek character se zyada lete hi `b` repeat ho jayega.

Longest valid substring:

```text
"b"
```

Length:

```text
1
```

So:

```text
Output = 1
```

---

# 🔹 Example 3

```text
s = "pwwkew"
```

Longest valid substrings:

```text
"wke"
```

or:

```text
"kew"
```

Length:

```text
3
```

So:

```text
Output = 3
```

---

# 🧠 Approach

Ye problem:

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
```

### `high`

New character ko window me add karega:

```text
high++ → EXPAND
```

### `low`

Duplicate aane par characters remove karega:

```text
low++ → SHRINK
```

### Frequency Map

Current window me har character kitni baar present hai, wo store karega.

---

# 🔥 Frequency Map

```cpp
unordered_map<char,int> mp;
```

Suppose current window:

```text
"abc"
```

Map:

```text
a → 1
b → 1
c → 1
```

Ye valid hai because kisi ki frequency:

```text
> 1
```

nahi hai.

---

# ❌ Duplicate Kab Hoga?

Suppose next character:

```text
a
```

aa gaya.

Map:

```text
a → 2
b → 1
c → 1
```

Ab:

```text
a → 2
```

Matlab `a` repeat ho gaya.

Window invalid ❌

---

# 🔥 Initial Variables

```cpp
int n = s.size();
int low = 0;
int high = 0;
int max_len = 0;

unordered_map<char,int> mp;
```

Meaning:

```text
n       → string ki length

low     → window ka starting point

high    → window ka ending point

max_len → longest valid substring ki length

mp      → character frequencies
```

---

# ❓ `max_len = 0` Kyu?

Hum longest valid substring ki **length** find kar rahe hain.

Agar string empty ho:

```text
""
```

to answer naturally:

```text
0
```

hoga.

Isliye:

```cpp
int max_len = 0;
```

rakhna safe hai.

---

# 🔥 Step 1 - Character Add Karo

Outer loop:

```cpp
while(high < n)
```

Current `high` character ko window me add karenge:

```cpp
mp[s[high]]++;
```

Example:

```text
s = "abc"
```

First:

```text
a → 1
```

Then:

```text
a → 1
b → 1
```

Then:

```text
a → 1
b → 1
c → 1
```

Sabki frequency `1` hai.

So window valid hai.

---

# 🔥 Step 2 - Duplicate Check

Humne jo current character add kiya hai:

```text
s[high]
```

agar uski frequency:

```text
> 1
```

ho gayi, to duplicate aa gaya.

Condition:

```cpp
while(mp[s[high]] > 1)
```

Meaning:

```text
Jab tak current character duplicate hai,
window ko left se shrink karte raho.
```

---

# ❓ `mp.size() > something` Kyu Nahi?

Pichle questions me hum unique character types count kar rahe the.

Example:

```cpp
mp.size() > 2
```

Fruit Into Baskets me useful tha.

Lekin yahan problem unique character **count** ki nahi hai.

Problem hai:

```text
koi character repeat nahi hona chahiye
```

Example:

```text
"abca"
```

Map:

```text
a → 2
b → 1
c → 1
```

Map size:

```text
3
```

Map size dekh kar directly duplicate pata nahi chalega.

Actual problem:

```text
a ki frequency = 2
```

hai.

Isliye frequency check karte hain:

```cpp
mp[s[high]] > 1
```

---

# 🔥 Step 3 - Duplicate Remove Karna

Duplicate mil gaya to leftmost character ki frequency decrease:

```cpp
mp[s[low]]--;
```

Then agar frequency:

```text
0
```

ho gayi:

```cpp
if(mp[s[low]] == 0){
    mp.erase(s[low]);
}
```

Then:

```cpp
low++;
```

Window shrink ho jayegi.

---

# ❓ `while` Kyu? `if` Kyu Nahi?

Ye important hai.

Suppose:

```text
s = "abba"
```

Current valid window:

```text
"ab"
```

Next:

```text
b
```

add hua.

Window:

```text
"abb"
```

Map:

```text
a → 1
b → 2
```

Duplicate:

```text
b → 2
```

Ab left se remove:

```text
a
```

Window:

```text
"bb"
```

Map:

```text
a → 0
b → 2
```

`a` remove ho gaya, lekin:

```text
b → 2
```

abhi bhi duplicate hai!

Agar hum sirf:

```cpp
if(mp[s[high]] > 1)
```

use karte, sirf ek baar shrink hota aur `"bb"` invalid reh jata.

Isliye:

```cpp
while(mp[s[high]] > 1)
```

use karte hain.

Dobara shrink:

```text
"bb"
 ↓
first b remove
 ↓
"b"
```

Now:

```text
b → 1
```

Valid ✅

---

# 🔥 Step 4 - Frequency Zero Par Erase

Code:

```cpp
if(mp[s[low]] == 0){
    mp.erase(s[low]);
}
```

Suppose:

```text
a → 1
```

Left se `a` remove kiya:

```cpp
mp['a']--;
```

Now:

```text
a → 0
```

Current window me `a` exist nahi karta.

So:

```cpp
mp.erase('a');
```

kar dete hain.

---

# 🔥 Step 5 - `low++`

Character remove hone ke baad:

```cpp
low++;
```

Meaning:

```text
window shrink
```

---

# 🔥 Step 6 - Length Calculate

Jab inner:

```cpp
while(mp[s[high]] > 1)
```

finish ho gaya, current window me duplicate nahi hai.

So current window valid hai.

Length:

```cpp
int res = high-low+1;
```

---

# 🔥 Step 7 - Maximum Update

```cpp
max_len = max(res,max_len);
```

Current valid substring aur previous longest substring compare karenge.

---

# 🔄 Dry Run

Consider:

```text
s = "abcabcbb"
```

Initially:

```text
low = 0
high = 0

max_len = 0
```

---

## Step 1 - `a`

Add:

```text
a → 1
```

Window:

```text
[a]
```

Duplicate?

```text
No
```

Length:

```text
1
```

So:

```text
max_len = 1
```

---

## Step 2 - `b`

Add:

```text
a → 1
b → 1
```

Window:

```text
[ab]
```

Valid.

Length:

```text
2
```

So:

```text
max_len = 2
```

---

## Step 3 - `c`

Add:

```text
a → 1
b → 1
c → 1
```

Window:

```text
[abc]
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

## Step 4 - Next `a`

Add:

```text
a → 2
b → 1
c → 1
```

Current window:

```text
[abca]
```

Duplicate:

```text
a → 2 ❌
```

Condition:

```cpp
while(mp[s[high]] > 1)
```

true.

Current:

```text
s[high] = 'a'
```

So:

```text
mp['a'] = 2
```

---

## Shrink

`low` par:

```text
a
```

hai.

Remove:

```cpp
mp[s[low]]--;
```

So:

```text
a → 1
```

Then:

```cpp
low++;
```

Current window:

```text
a [bca]
   ↑
  low
```

Now:

```text
a → 1
```

Duplicate gone.

Window:

```text
"bca"
```

Valid.

Length:

```text
3
```

Maximum remains:

```text
3
```

---

# 🔥 Another Important Dry Run - `"abba"`

Consider:

```text
s = "abba"
```

Start:

```text
"a"
```

Map:

```text
a → 1
```

Valid.

---

Next:

```text
"ab"
```

Map:

```text
a → 1
b → 1
```

Valid.

Length:

```text
2
```

---

Next `b`:

```text
"abb"
```

Map:

```text
a → 1
b → 2
```

Duplicate ❌

Shrink:

```text
a remove
```

Map:

```text
a → 0
b → 2
```

Erase `a`.

Window:

```text
"bb"
```

But:

```text
b → 2
```

still duplicate.

So inner `while` **again** chalega.

Remove first `b`:

```text
b → 1
```

Window:

```text
"b"
```

Valid.

Yehi reason hai ki:

```cpp
while(...)
```

chahiye, `if` nahi.

---

# 💻 C++ Code

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {

        int n = s.size();
        int low = 0;
        int high = 0;
        int max_len = 0;

        unordered_map<char,int> mp;

        while(high < n){

            mp[s[high]]++;

            while(mp[s[high]] > 1){

                mp[s[low]]--;

                if(mp[s[low]] == 0){
                    mp.erase(s[low]);
                }

                low++;
            }

            int res = high-low+1;

            max_len = max(res,max_len);

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
int n = s.size();
```

String ki total length.

---

## Left Pointer

```cpp
int low = 0;
```

Window ka starting point.

```text
low++ → SHRINK
```

---

## Right Pointer

```cpp
int high = 0;
```

Window ka ending point.

```text
high++ → EXPAND
```

---

## Maximum Length

```cpp
int max_len = 0;
```

Longest valid substring ki length.

---

## Frequency Map

```cpp
unordered_map<char,int> mp;
```

Har character ki current window me frequency store karega.

---

## Character Add

```cpp
mp[s[high]]++;
```

Current character window me add hota hai.

---

## Duplicate Condition

```cpp
while(mp[s[high]] > 1)
```

Agar newly added character ki frequency `1` se jyada hai:

```text
duplicate exists
```

Window shrink karo.

---

## Left Character Remove

```cpp
mp[s[low]]--;
```

Leftmost character ki frequency decrease.

---

## Erase

```cpp
if(mp[s[low]] == 0){
    mp.erase(s[low]);
}
```

Frequency zero hui to map se remove.

---

## Shrink

```cpp
low++;
```

Window left side se chhoti hoti hai.

---

## Length

```cpp
int res = high-low+1;
```

Current valid window ki length.

---

## Maximum

```cpp
max_len = max(res,max_len);
```

Longest valid window update.

---

## Expand

```cpp
high++;
```

Next character par move.

---

# 🆚 Previous Sliding Window Questions

## Longest K Unique

Requirement:

```text
Exactly K unique characters
```

Invalid:

```cpp
mp.size() > k
```

Answer:

```cpp
if(mp.size() == k)
```

---

## Fruit Into Baskets

Requirement:

```text
At most 2 unique fruit types
```

Invalid:

```cpp
mp.size() > 2
```

Answer:

```text
while ke baad direct length
```

because:

```text
size <= 2
```

valid hai.

---

## Longest Substring Without Repeating Characters

Requirement:

```text
Every character frequency <= 1
```

Invalid:

```cpp
mp[s[high]] > 1
```

Answer:

```text
duplicate remove hone ke baad direct length
```

---

# ⭐ Comparison Table

| Problem | Valid Condition | Invalid Condition |
|---|---|---|
| Longest K Unique | Exactly `k` unique | `mp.size() > k` |
| Fruit Into Baskets | At most `2` unique | `mp.size() > 2` |
| No Repeating Characters | Every frequency ≤ 1 | `mp[s[high]] > 1` |

---

# 🔥 Main Pattern

```cpp
while(high<n){

    // EXPAND
    mp[s[high]]++;

    // INVALID WINDOW
    while(mp[s[high]]>1){

        // REMOVE LEFT
        mp[s[low]]--;

        if(mp[s[low]]==0){
            mp.erase(s[low]);
        }

        // SHRINK
        low++;
    }

    // WINDOW IS NOW VALID
    int res=high-low+1;

    max_len=max(res,max_len);

    high++;
}
```

---

# ⏱️ Time Complexity

```text
O(n)
```

Why?

`high` pointer poori string me maximum:

```text
n
```

steps move karta hai.

`low` pointer bhi maximum:

```text
n
```

steps move karta hai.

Dono kabhi backward nahi jaate.

So:

```text
n + n = 2n
```

Big-O:

```text
O(2n) = O(n)
```

Nested `while` hone ke baad bhi `O(n²)` nahi hai because `low` baar-baar `0` se restart nahi hota.

---

# 💾 Space Complexity

Map me characters ki frequencies store hoti hain.

General case:

```text
O(number of unique characters)
```

For a fixed character set, auxiliary space is bounded by that character set.

---

# 🔥 Quick Revision

```text
high se character add
        ↓
frequency > 1 ?
        ↓ YES
duplicate aa gaya
        ↓
low se character remove
        ↓
frequency--
        ↓
frequency 0?
        ↓
erase
        ↓
low++
        ↓
duplicate hatne tak repeat
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

# 🧠 One-Line Revision

```text
High se characters add karo → duplicate aaye to low se remove karte raho → duplicate hatne ke baad current window ki length se maximum update karo.
```

---

# ⭐ Most Important Takeaway

```text
Fruit Into Baskets
→ unique TYPES control kar rahe the
→ mp.size()

Longest Without Repeating
→ character FREQUENCY control kar rahe hain
→ mp[s[high]]
```

Main duplicate condition:

```cpp
while(mp[s[high]] > 1)
```

Ye is question ki sabse important line hai.
