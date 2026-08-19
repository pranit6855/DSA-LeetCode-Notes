# LeetCode 1249 — Minimum Remove to Make Valid Parentheses

**Pattern:** Stack — Matching / Pairing

**Filename:** `03-Minimum-Remove-to-Make-Valid-Parentheses.md`

**Folder:** `Stack/01-Matching-Pairing/`

---

# Problem

Hume ek string `s` di gayi hai jisme:

```text
lowercase letters
(
)
```

ho sakte hain.

Hume **minimum number of parentheses remove** karne hain taaki final string valid parentheses string ban jaye.

Letters ko remove nahi karna hai.

---

# Examples

## Example 1

```text
Input:
s = "lee(t(c)o)de)"

Output:
"lee(t(c)o)de"
```

Last `)` ka koi matching `(` nahi hai.

Isliye us `)` ko remove karna hai.

---

## Example 2

```text
Input:
s = "a)b(c)d"

Output:
"ab(c)d"
```

Index `1` wala `)` invalid hai.

So usko remove karenge.

---

## Example 3

```text
Input:
s = "))("

Output:
""
```

Dono `)` ke liye koi opening bracket nahi hai.

Aur last `(` ka koi closing bracket nahi hai.

Isliye teeno parentheses remove ho jayenge.

---

# Pattern Identification

Agar question mein:

```text
matching
parentheses
balanced
opening / closing
valid parentheses
remove invalid brackets
```

jaise words aayein, to:

```text
Stack
```

ka thought aana chahiye.

Ye:

```text
Stack Pattern #1 — Matching / Pairing
```

ka question hai.

---

# Why Stack?

Hume opening `(` ko yaad rakhna hai taaki future mein aane wale `)` se match kar sakein.

Lekin is question mein ek extra problem hai:

> Agar bracket invalid hai to usko **remove** karna hai.

Isliye hume sirf bracket nahi, uska **index** store karna useful hai.

Example:

```text
s = "a)b(c)d"

index:
0 1 2 3 4 5 6

char:
a ) b ( c ) d
```

Agar index `1` wala `)` invalid hai, to hume specifically:

```text
index = 1
```

remove karna hai.

Isliye:

```cpp
stack<int> st;
```

use karenge.

---

# Main Idea

Hum do cheezein maintain karenge:

### 1. Stack

Unmatched `(` ke **indexes** store karega.

```cpp
stack<int> st;
```

### 2. Remove Array

Kaunsa character remove karna hai, usko mark karega.

```cpp
vector<bool> remove(s.size(), false);
```

Meaning:

```text
false → keep this character
true  → remove this character
```

---

# Algorithm

```text
Start
  ↓
Empty stack banao
  ↓
Remove array banao
  ↓
String traverse karo
  ↓
'(' ?
 ├── YES → index push
 └── NO → ')'
           ↓
      Stack empty?
      ├── YES → remove[i] = true
      └── NO  → pop()
  ↓
Loop complete
  ↓
Stack mein bache '('
  ↓
Unke indexes bhi remove mark karo
  ↓
New answer string banao
  ↓
Marked indexes skip karo
  ↓
Return answer
```

---

# Step 1 — Stack

```cpp
stack<int> st;
```

Yahan:

```text
int
```

isliye use hua hai kyunki hume bracket ka **index** store karna hai.

Example:

```text
s = "a(b"
```

`(` index `1` pe hai.

To:

```cpp
st.push(1);
```

Stack:

```text
[1]
```

---

# Step 2 — Remove Array

```cpp
vector<bool> remove(s.size(), false);
```

Suppose:

```text
s = "a)b(c)d"
```

Length = `7`.

Initially:

```text
index:   0 1 2 3 4 5 6
remove:  F F F F F F F
```

Sabko initially keep karna hai.

---

# Step 3 — Traverse String

```cpp
for(int i = 0; i < s.size(); i++)
```

Har character ke saath uska index bhi milega.

---

# Case 1 — Opening Bracket

```cpp
if(s[i] == '(') {
    st.push(i);
}
```

Agar `(` mila:

```text
index = 3
```

to:

```cpp
st.push(3);
```

Matlab:

> Index 3 wala `(` abhi unmatched hai.

---

# Case 2 — Closing Bracket

```cpp
else if(s[i] == ')')
```

Ab check karenge:

```cpp
if(st.empty())
```

---

# Case 2A — Stack Empty

Agar:

```text
stack = []
```

aur current character `)` hai, to iska koi matching `(` nahi hai.

Therefore:

```cpp
remove[i] = true;
```

Matlab current `)` ko remove karna hai.

---

# Example

```text
s = "a)b"
```

Indexes:

```text
0 1 2
a ) b
```

Index `1` par:

```text
)
```

Stack empty hai.

So:

```cpp
remove[1] = true;
```

Now:

```text
index:   0 1 2
remove:  F T F
```

Matlab index `1` remove hoga.

---

# Case 2B — Stack Non-Empty

Agar Stack mein koi `(` available hai:

```text
stack = [3]
```

aur current character `)` hai.

To current `)` us `(` se match karega.

So:

```cpp
st.pop();
```

Example:

```text
index 3 → (
index 5 → )
```

Pair:

```text
( )
```

Complete ho gaya.

Isliye index `3` Stack se hata denge.

---

# Important Rule

Closing bracket ke time:

```text
')'
 ↓
Stack empty?
```

### Yes

```text
No matching '('
↓
remove current ')'
```

### No

```text
Matching '(' available
↓
pop()
```

---

# Step 4 — Remaining Opening Brackets

Loop complete hone ke baad Stack mein kuch indexes bach sakte hain.

Example:

```text
s = "a(b(c"
```

Indexes:

```text
0 1 2 3 4
a ( b ( c
```

Processing ke baad:

```text
stack = [1, 3]
```

Matlab:

```text
index 1 → unmatched '('
index 3 → unmatched '('
```

Dono ko remove karna padega.

---

# While Loop

```cpp
while(!st.empty()) {
    remove[st.top()] = true;
    st.pop();
}
```

### `st.top()`

Stack ka top index deta hai.

Suppose:

```text
stack = [1, 3]
```

Top:

```text
3
```

So:

```cpp
remove[3] = true;
```

Then:

```cpp
st.pop();
```

Ab:

```text
stack = [1]
```

Again:

```text
top = 1
```

So:

```cpp
remove[1] = true;
```

Then `pop()`.

Ab:

```text
stack = []
```

Done.

---

# Why Do We Need This While Loop?

Because loop ke end mein bhi kuch unmatched `(` ho sakte hain.

Example:

```text
s = "((("
```

Stack:

```text
[0, 1, 2]
```

Koi closing bracket nahi mila.

Therefore teeno opening brackets invalid hain.

We must mark all three for removal.

---

# Step 5 — Answer String

Ab invalid brackets mark ho chuke hain.

Actual result banane ke liye:

```cpp
string ans = "";
```

Phir:

```cpp
for(int i = 0; i < s.size(); i++) {
    if(!remove[i]) {
        ans += s[i];
    }
}
```

Meaning:

```text
remove[i] == false
        ↓
character valid
        ↓
answer mein add karo
```

---

# What Does `!remove[i]` Mean?

Agar:

```text
remove[i] = false
```

then:

```text
!false = true
```

Character add hoga.

Agar:

```text
remove[i] = true
```

then:

```text
!true = false
```

Character skip hoga.

Simple:

```text
true  → remove
false → keep
```

---

# Detailed Dry Run

## Input

```text
s = "a)b(c)d"
```

Indexes:

```text
0 1 2 3 4 5 6
a ) b ( c ) d
```

Initial:

```text
stack = []
remove = [F F F F F F F]
```

---

## `i = 0`

Character:

```text
a
```

Letter hai.

Ignore.

---

## `i = 1`

Character:

```text
)
```

Stack:

```text
[]
```

Empty hai.

Therefore:

```cpp
remove[1] = true;
```

Now:

```text
remove = [F T F F F F F]
```

---

## `i = 2`

Character:

```text
b
```

Ignore.

---

## `i = 3`

Character:

```text
(
```

Push index:

```cpp
st.push(3);
```

Stack:

```text
[3]
```

---

## `i = 4`

Character:

```text
c
```

Ignore.

---

## `i = 5`

Character:

```text
)
```

Stack:

```text
[3]
```

Empty nahi hai.

So:

```cpp
st.pop();
```

Stack:

```text
[]
```

Pair successfully matched.

---

## `i = 6`

Character:

```text
d
```

Ignore.

---

# After First Loop

Stack:

```text
[]
```

Remove array:

```text
index:   0 1 2 3 4 5 6
remove:  F T F F F F F
```

Only index `1` remove karna hai.

---

# Build Answer

Characters:

```text
0 → a → keep
1 → ) → skip
2 → b → keep
3 → ( → keep
4 → c → keep
5 → ) → keep
6 → d → keep
```

Answer:

```text
ab(c)d
```

---

# Another Dry Run

## Input

```text
s = "a(b(c"
```

Indexes:

```text
0 1 2 3 4
a ( b ( c
```

Processing:

```text
a → ignore
( → push 1
b → ignore
( → push 3
c → ignore
```

Stack:

```text
[1, 3]
```

No closing bracket came.

So while loop:

```text
remove[3] = true
pop()

remove[1] = true
pop()
```

Final:

```text
remove = [F T F T F]
```

Build answer:

```text
a b c
```

Final:

```text
"abc"
```

---

# Another Dry Run

## Input

```text
s = "))("
```

Indexes:

```text
0 1 2
) ) (
```

### Index 0

`)`

Stack empty:

```text
remove[0] = true
```

### Index 1

`)`

Stack empty:

```text
remove[1] = true
```

### Index 2

`(`

Push:

```text
stack = [2]
```

Loop complete.

Remaining `(`:

```text
remove[2] = true
```

Final:

```text
remove = [T T T]
```

Answer:

```text
""
```

---

# Complete C++ Code

```cpp
class Solution {
public:
    string minRemoveToMakeValid(string s) {

        stack<int> st;
        vector<bool> remove(s.size(), false);

        for (int i = 0; i < s.size(); i++) {

            if (s[i] == '(') {
                st.push(i);
            }

            else if (s[i] == ')') {

                if (st.empty()) {
                    remove[i] = true;
                }

                else {
                    st.pop();
                }
            }
        }

        // Remaining '(' are unmatched
        while (!st.empty()) {
            remove[st.top()] = true;
            st.pop();
        }

        string ans = "";

        for (int i = 0; i < s.size(); i++) {

            if (!remove[i]) {
                ans += s[i];
            }
        }

        return ans;
    }
};
```

---

# Code Breakdown

## Stack

```cpp
stack<int> st;
```

Unmatched `(` ke indexes store karta hai.

---

## Remove Array

```cpp
vector<bool> remove(s.size(), false);
```

Invalid indexes ko mark karta hai.

---

## Opening

```cpp
if(s[i] == '(') {
    st.push(i);
}
```

Opening bracket ka index save karo.

---

## Closing

```cpp
else if(s[i] == ')')
```

Agar closing bracket hai:

### Stack empty

```cpp
remove[i] = true;
```

Current `)` invalid hai.

### Stack non-empty

```cpp
st.pop();
```

Current `)` ek previous `(` ko match kar raha hai.

---

## Remaining Opening

```cpp
while(!st.empty()) {
    remove[st.top()] = true;
    st.pop();
}
```

End mein bache unmatched `(` ko remove mark karo.

---

## Build Answer

```cpp
for(int i = 0; i < s.size(); i++) {
    if(!remove[i]) {
        ans += s[i];
    }
}
```

Sirf valid characters answer mein add karo.

---

# Common Mistake

### Mistake 1

```cpp
stack<char> st;
```

Is problem mein ye possible hai, but index store karna convenient nahi rahega.

Better:

```cpp
stack<int> st;
```

---

### Mistake 2

Unmatched `)` ko Stack mein push kar dena.

Wrong:

```text
)
↓
push
```

Correct:

```text
)
↓
stack empty?
↓
remove it
```

---

### Mistake 3

End mein Stack mein bache `(` ko ignore kar dena.

Example:

```text
"((("
```

Ye valid nahi hai.

Isliye:

```cpp
while(!st.empty())
```

se unmatched opening brackets ko remove mark karna zaroori hai.

---

# Why Index Store Karna Important Hai?

Example:

```text
s = "a)b"
```

Hume sirf ye nahi pata hona chahiye ki:

```text
')' invalid hai
```

Hume ye bhi pata hona chahiye ki:

```text
index = 1
```

par invalid `)` hai.

Taaki final string banate waqt us exact character ko skip kar sakein.

---

# Why Do We Use Two Passes?

### First pass

Decide:

```text
Kaunsa bracket invalid hai?
```

### Second pass

Actually answer banao:

```text
Invalid brackets skip karo.
Valid characters add karo.
```

Ye approach code ko simple rakhti hai.

---

# Complexity

Let:

```text
n = s.size()
```

### Time Complexity

First traversal:

```text
O(n)
```

Remaining Stack process:

```text
O(n)
```

Final answer traversal:

```text
O(n)
```

Overall:

```text
O(n)
```

### Space Complexity

Stack worst case:

```text
O(n)
```

Remove array:

```text
O(n)
```

Answer string:

```text
O(n)
```

Overall auxiliary/storage:

```text
O(n)
```

---

# Interview Explanation

> I use a stack to store the indexes of unmatched opening parentheses. When I encounter a closing parenthesis, if the stack is empty, that closing parenthesis is invalid, so I mark its index for removal. Otherwise, I pop the matching opening parenthesis. After traversing the string, any indexes left in the stack represent unmatched opening parentheses, so I mark them for removal as well. Finally, I build the result by skipping all marked indexes. The time complexity is O(n) and the space complexity is O(n).

---

# Pattern Recognition

Mental template:

```text
'('
 ↓
store index

')'
 ↓
stack empty?
 ├── YES → mark ')' for removal
 └── NO  → pop '('

End
 ↓
remaining '(' indexes
 ↓
mark them for removal

Finally
 ↓
skip marked indexes
 ↓
build answer
```

---

# LC 921 vs LC 1249

## LeetCode 921

```text
Minimum Add to Make Parentheses Valid
```

Invalid bracket:

```text
ADD
```

---

## LeetCode 1249

```text
Minimum Remove to Make Valid Parentheses
```

Invalid bracket:

```text
REMOVE
```

---

# Key Takeaways

* Matching / Pairing pattern use hua.
* `(` ka **index** Stack mein store kiya.
* Invalid `)` ko directly remove mark kiya.
* End mein Stack mein bache `(` bhi remove mark kiye.
* `remove[i] = true` means character delete karna hai.
* `!remove[i]` means character answer mein rakhna hai.
* Final answer second traversal se banta hai.
* Time: `O(n)`
* Space: `O(n)`

---

# One-Line Revision

> **`(` ka index push karo, `)` ko stack se match karo; unmatched `)` ko mark karo, end mein bache unmatched `(` ko bhi mark karo, phir marked indexes skip karke answer banao.**

---

# Stack Roadmap Progress

## Pattern #1 — Matching / Pairing

* [x] LeetCode 20 — Valid Parentheses
* [x] LeetCode 921 — Minimum Add to Make Parentheses Valid
* [x] LeetCode 1249 — Minimum Remove to Make Valid Parentheses

## Current Folder

```text
Stack/01-Matching-Pairing/
```

## Files

```text
01-Valid-Parentheses.md
02-Minimum-Add-to-Make-Parentheses-Valid.md
03-Minimum-Remove-to-Make-Valid-Parentheses.md
```

---

# Stack Folder Structure

```text
Stack/
│
├── 01-Matching-Pairing/
│   ├── 01-Valid-Parentheses.md
│   ├── 02-Minimum-Add-to-Make-Parentheses-Valid.md
│   └── 03-Minimum-Remove-to-Make-Valid-Parentheses.md
│
├── 02-Stack-Simulation/
│
├── 03-Monotonic-Stack/
│
├── 04-Next-Previous-Greater-Smaller/
│
├── 05-Boundary-Contribution/
│
└── 06-Stack-Design/
```
