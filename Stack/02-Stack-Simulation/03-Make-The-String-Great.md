# LeetCode 1544 — Make The String Great

**Pattern:** Stack — Stack Simulation

---

# Problem

Hume ek string `s` di gayi hai.

String ko **good** tab kaha jayega jab usme koi adjacent pair aisa na ho jahan:

```text
same letter
+
one uppercase
+
one lowercase
```

Example:

```text
aA
Aa
bB
Bb
```

Ye sab bad pairs hain.

Jab bhi aisa bad adjacent pair mile:

```text
dono characters remove karo
```

Ye process tab tak repeat karo jab tak koi bad pair na bache.

---

# Example 1

```text
s = "leEeetcode"
```

Yahan:

```text
E e
```

adjacent hain.

Dono same letter hain:

```text
E
e
```

Bas case different hai.

So dono remove:

```text
leEeetcode
   ↓
leetcode
```

Output:

```text
"leetcode"
```

---

# Example 2

```text
s = "abBAcC"
```

Pehle:

```text
bB
```

remove:

```text
aBAcC
```

Actually stack processing ke according:

```text
abBAcC

a → stack
b → stack

B → bB bad pair
    b remove

Stack:
[a]

A → aA bad pair
    a remove

Stack:
[]

c → stack
C → cC bad pair
    c remove

Final:
""
```

Output:

```text
""
```

---

# Main Observation

Hume string ko:

```text
LEFT → RIGHT
```

process karna hai.

Har current character ke liye sirf ek cheez important hai:

```text
Stack ka TOP
```

Kyun?

Kyuki agar current character kisi previous character ke saath adjacent pair banayega, to wo previous **remaining** character Stack ke top par hi hoga.

---

# Stack Idea

Hum:

```cpp
stack<char> st;
```

use karenge.

Stack mein:

```text
ab tak ke valid characters
```

store honge.

Example:

```text
s = "abc"
```

process hone ke baad:

```text
st = [a,b,c]
```

---

# Main Rule

Har character `ch` ke liye:

```text
Stack empty?
    ↓
YES → push ch

NO
    ↓
Stack top aur current character
same letter ke opposite cases hain?
    ↓
YES → top pop
NO  → current push
```

Bas ye poora algorithm hai.

---

# Opposite Case Kaise Check Karein?

Example:

```text
a  and  A
b  and  B
c  and  C
```

ASCII values mein uppercase aur lowercase letter ke beech difference:

```text
32
```

hota hai.

Example:

```text
'a' = 97
'A' = 65
```

Difference:

```text
97 - 65 = 32
```

So:

```cpp
abs(st.top() - ch) == 32
```

check kar sakte hain.

Agar true:

```text
same letter + opposite case
```

So pair bad hai.

---

# Example

```text
'a' and 'A'
```

Check:

```text
abs('a' - 'A')
=
32
```

So bad pair.

---

# Example

```text
'a' and 'b'
```

Difference:

```text
1
```

So:

```text
not bad
```

Push current character.

---

# Why Stack Works?

Important point:

Suppose:

```text
s = "abBAc"
```

Start:

```text
a → [a]
b → [a,b]
```

Now `B` comes.

Stack top:

```text
b
```

Current:

```text
B
```

Ye:

```text
bB
```

bad pair hai.

So:

```text
b
B
```

dono remove.

Stack:

```text
[a]
```

Ab current next character `A` aata hai.

Stack top:

```text
a
```

So:

```text
aA
```

bad pair ban gaya.

Dono remove:

```text
[]
```

Ye exactly wahi behavior hai jo problem chahti hai.

---

# Important Observation

Ek pair remove hone ke baad:

```text
new adjacent pair
```

ban sakta hai.

Ye Stack automatically handle karta hai.

Example:

```text
abBA
```

### `a`

```text
[a]
```

### `b`

```text
[a,b]
```

### `B`

`bB` bad:

```text
[a]
```

### `A`

Ab `A` ke just pehle Stack top:

```text
a
```

So `aA` bad:

```text
[]
```

Final:

```text
""
```

Without Stack, ye chaining manually manage karna harder hota.

---

# Why We Compare Only With Stack Top?

Suppose Stack:

```text
[a,b,c]
```

Current:

```text
C
```

Current `C` sirf:

```text
c
```

ke saath adjacent ban sakta hai.

`a` ya `b` ke saath direct adjacent nahi hai because `c` beech mein hai.

So sirf:

```cpp
st.top()
```

check karna enough hai.

---

# Algorithm

```text
Start
  ↓
Empty Stack
  ↓
String ke characters left → right process karo
  ↓
Current character ch
  ↓
Stack empty?
  ├── YES → push ch
  │
  └── NO
        ↓
    Kya top aur ch
    opposite-case same letter hain?
        ↓
      YES
        ↓
      pop
        ↓
      NO
        ↓
      push ch
  ↓
Next character
  ↓
End
  ↓
Stack ke characters ko string mein convert karo
```

---

# Complete C++ Code

```cpp
class Solution {
public:
    string makeGood(string s) {

        stack<char> st;

        for (char ch : s) {

            // Stack empty hai to current character push karo
            if (st.empty()) {
                st.push(ch);
            }

            // Top aur current same letter ke
            // opposite cases hain
            else if (abs(st.top() - ch) == 32) {

                // Dono cancel ho gaye
                st.pop();
            }

            // Otherwise current character valid hai
            else {
                st.push(ch);
            }
        }

        string ans;

        // Stack se characters nikalo
        while (!st.empty()) {

            ans += st.top();
            st.pop();
        }

        // Stack reverse order mein characters deta hai
        reverse(ans.begin(), ans.end());

        return ans;
    }
};
```

---

# Code Explanation

## 1. Stack

```cpp
stack<char> st;
```

Character Stack banaya.

Example:

```text
[a,b,c]
```

---

# 2. String Traverse

```cpp
for (char ch : s)
```

String ke har character ko left se right process karenge.

Example:

```text
s = "abBAc"
```

Sequence:

```text
a
b
B
A
c
```

---

# 3. Stack Empty

```cpp
if (st.empty()) {
    st.push(ch);
}
```

Agar Stack mein kuch nahi hai to current character ke saath pair banane ke liye koi previous valid character nahi hai.

So directly push.

---

# 4. Bad Pair Check

```cpp
else if (abs(st.top() - ch) == 32)
```

Ye main condition hai.

Check:

```text
Stack Top
vs
Current Character
```

Agar ASCII difference `32` hai:

```text
same alphabet
opposite case
```

So pair bad hai.

---

# 5. Bad Pair Mila

```cpp
st.pop();
```

Current character ko push nahi karte.

Kyun?

Because:

```text
top + current
```

dono remove hone hain.

Current string ko represent karne ke liye Stack mein:

```text
top
```

remove karna enough hai.

Current `ch` already process ho gaya, so separately push nahi karna.

---

# Example

```text
Stack = [a,b]
Current = B
```

Check:

```text
abs('b' - 'B') == 32
```

True.

Then:

```text
pop b
```

Stack:

```text
[a]
```

`B` bhi final result mein nahi aayega.

---

# 6. Normal Character

```cpp
else {
    st.push(ch);
}
```

Agar bad pair nahi bana:

```text
current character valid hai
```

So Stack mein push.

---

# 7. Answer String

Processing ke baad:

```cpp
string ans;
```

Final remaining characters Stack mein hain.

---

# 8. Stack Se String Banana

```cpp
while (!st.empty()) {

    ans += st.top();
    st.pop();
}
```

Stack LIFO hai.

Suppose:

```text
st = [l,e,e,t,c,o,d,e]
```

Top se niklega:

```text
e,d,o,c,t,e,e,l
```

So `ans` reverse order mein banega.

---

# 9. Reverse

```cpp
reverse(ans.begin(), ans.end());
```

Original order restore karne ke liye reverse karte hain.

---

# Detailed Dry Run

Input:

```text
s = "abBAcC"
```

Initial:

```text
st = []
```

---

## Character `a`

Stack empty.

Push:

```text
[a]
```

---

## Character `b`

Top:

```text
a
```

Current:

```text
b
```

Check:

```text
abs('a' - 'b')
= 1
```

Not `32`.

So push:

```text
[a,b]
```

---

## Character `B`

Top:

```text
b
```

Current:

```text
B
```

Check:

```text
abs('b' - 'B')
= 32
```

Bad pair.

So:

```text
pop b
```

Stack:

```text
[a]
```

`B` also disappear ho gaya.

---

## Character `A`

Top:

```text
a
```

Current:

```text
A
```

Check:

```text
abs('a' - 'A')
= 32
```

Bad pair.

Pop:

```text
[]
```

---

## Character `c`

Stack empty.

Push:

```text
[c]
```

---

## Character `C`

Top:

```text
c
```

Current:

```text
C
```

Check:

```text
abs('c' - 'C')
= 32
```

Bad pair.

Pop:

```text
[]
```

---

# Final

Stack:

```text
[]
```

So:

```text
ans = ""
```

Return:

```text
""
```

---

# Another Dry Run

```text
s = "leEeetcode"
```

Process:

```text
l → [l]
e → [l,e]
E → eE bad → [l]
e → [l,e]
e → [l,e,e]
t → [l,e,e,t]
c → [l,e,e,t,c]
o → [l,e,e,t,c,o]
d → [l,e,e,t,c,o,d]
e → [l,e,e,t,c,o,d,e]
```

Final answer:

```text
"leetcode"
```

---

# Why No Explicit "Delete Current Character"?

Suppose:

```text
Stack top = b
Current = B
```

Bad pair.

Problem says:

```text
bB
```

dono remove karo.

We do:

```cpp
st.pop();
```

This removes `b`.

And current `B` ko humne Stack mein push hi nahi kiya.

So effectively:

```text
b + B
```

dono gone.

---

# Why Stack Is Perfect Here

String mein pair removal se:

```text
new adjacency
```

create ho sakti hai.

Stack mein:

```text
last remaining character
```

always top par hota hai.

So current character ke saath sirf top ko compare karke problem solve ho jaati hai.

---

# Complexity

## Time

```text
O(n)
```

Har character:

```text
push → maximum once
pop  → maximum once
```

So total operations linear hain.

---

## Space

```text
O(n)
```

Worst case mein koi pair remove nahi hota.

Example:

```text
abcdef
```

Then Stack mein saare characters rahenge.

---

# Common Mistakes

## Mistake 1 — Current character ko pop ke baad push karna

Wrong:

```cpp
if (badPair) {
    st.pop();
    st.push(ch);
}
```

Correct:

```cpp
if (badPair) {
    st.pop();
}
```

Kyun?

Because current character bhi bad pair ka part hai, so usse bhi remove hona hai.

---

## Mistake 2 — Sirf lowercase/uppercase compare karna

Sirf case different hona enough nahi hai.

Example:

```text
aB
```

Ye bad pair nahi hai.

Need:

```text
same letter
+
opposite case
```

`abs(st.top() - ch) == 32` exactly ye check karta hai.

---

## Mistake 3 — String ko repeatedly erase karna

Repeated string deletion expensive ho sakta hai.

Stack:

```text
push/pop = O(1)
```

isliye efficient hai.

---

## Mistake 4 — Sirf adjacent characters scan karte rehna

Ek removal ke baad new pair ban sakta hai.

Stack automatically previous remaining character ko top par rakhta hai.

---

# Pattern Recognition

Agar question mein:

```text
Characters remove
Adjacent pair
Undo / cancel
Last remaining character
Repeated removal
```

jaise words aayein:

```text
STACK SIMULATION
```

ka thought aana chahiye.

---

# Interview Explanation

> I process the string from left to right using a character stack. For every character, I compare it with the current stack top. If they are the same letter in opposite cases, they form a bad pair, so I pop the stack top and do not push the current character. Otherwise, I push the current character. This automatically handles new adjacent pairs created after removals because the previous remaining character becomes the new stack top. Finally, I reverse the stack contents to build the result string. The time complexity is O(n) and space complexity is O(n).

---

# One-Line Revision

> **Current character ko Stack top se compare karo; same letter ke opposite cases hain to top pop karo aur current ko push mat karo, warna current ko push karo.**

---

# Pattern #2 Progress

```text
[ ] LC 844 — Backspace String Compare     ⏭️ SKIPPED
[x] LC 946 — Validate Stack Sequences     ✅
[x] LC 1544 — Make The String Great       ✅
```

**Pattern #2 — Stack Simulation COMPLETE ✅**
